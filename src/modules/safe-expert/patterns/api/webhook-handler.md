# Webhook Handler Pattern

**Purpose**: Handle webhooks from external services (Stripe, Clerk, etc.) using system context

**When to use**: Webhook endpoints, background jobs, cron jobs, system operations without user context

## Implementation

### Step 1: Webhook Route with System Context

```typescript
// app/api/webhooks/stripe/route.ts
import { headers } from 'next/headers';
import { withSystemContext } from '@/lib/db/rls-helpers';
import Stripe from 'stripe';

const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!, {
  apiVersion: '2023-10-16',
});

export async function POST(request: Request) {
  const body = await request.text();
  const signature = headers().get('stripe-signature');

  if (!signature) {
    return Response.json({ error: 'Missing signature' }, { status: 400 });
  }

  let event: Stripe.Event;

  try {
    // 1. Verify webhook signature
    event = stripe.webhooks.constructEvent(
      body,
      signature,
      process.env.STRIPE_WEBHOOK_SECRET!
    );
  } catch (err) {
    console.error('Webhook signature verification failed:', err.message);
    return Response.json({ error: 'Invalid signature' }, { status: 400 });
  }

  // 2. Handle event with SYSTEM context (no user_id needed)
  try {
    await withSystemContext(async (prisma) => {
      switch (event.type) {
        case 'payment_intent.succeeded': {
          const paymentIntent = event.data.object as Stripe.PaymentIntent;

          // Log webhook event for audit trail
          await prisma.webhookEvent.create({
            data: {
              type: event.type,
              payload: JSON.stringify(event),
              processed_at: new Date(),
            },
          });

          // Update payment status
          await prisma.payment.update({
            where: { stripe_payment_intent_id: paymentIntent.id },
            data: { status: 'succeeded' },
          });
          break;
        }

        case 'payment_intent.payment_failed': {
          const paymentIntent = event.data.object as Stripe.PaymentIntent;

          await prisma.webhookEvent.create({
            data: {
              type: event.type,
              payload: JSON.stringify(event),
              processed_at: new Date(),
            },
          });

          await prisma.payment.update({
            where: { stripe_payment_intent_id: paymentIntent.id },
            data: {
              status: 'failed',
              failure_reason: paymentIntent.last_payment_error?.message,
            },
          });
          break;
        }

        default:
          console.log(`Unhandled event type: ${event.type}`);
      }
    });

    return Response.json({ received: true }, { status: 200 });
  } catch (error) {
    console.error('Webhook processing error:', error);
    return Response.json({ error: 'Webhook processing failed' }, { status: 500 });
  }
}
```

### Step 2: Clerk Webhook (User Events)

```typescript
// app/api/webhooks/clerk/route.ts
import { headers } from 'next/headers';
import { Webhook } from 'svix';
import { withSystemContext } from '@/lib/db/rls-helpers';

export async function POST(request: Request) {
  const WEBHOOK_SECRET = process.env.CLERK_WEBHOOK_SECRET;
  if (!WEBHOOK_SECRET) {
    throw new Error('Missing CLERK_WEBHOOK_SECRET');
  }

  // 1. Verify webhook signature
  const headerPayload = headers();
  const svix_id = headerPayload.get('svix-id');
  const svix_timestamp = headerPayload.get('svix-timestamp');
  const svix_signature = headerPayload.get('svix-signature');

  if (!svix_id || !svix_timestamp || !svix_signature) {
    return Response.json({ error: 'Missing svix headers' }, { status: 400 });
  }

  const payload = await request.json();
  const body = JSON.stringify(payload);

  const wh = new Webhook(WEBHOOK_SECRET);
  let evt;

  try {
    evt = wh.verify(body, {
      'svix-id': svix_id,
      'svix-timestamp': svix_timestamp,
      'svix-signature': svix_signature,
    });
  } catch (err) {
    console.error('Clerk webhook verification failed:', err);
    return Response.json({ error: 'Invalid signature' }, { status: 400 });
  }

  // 2. Handle Clerk events with SYSTEM context
  const eventType = evt.type;

  try {
    await withSystemContext(async (prisma) => {
      switch (eventType) {
        case 'user.created': {
          await prisma.user.create({
            data: {
              id: evt.data.id,
              email: evt.data.email_addresses[0]?.email_address,
              first_name: evt.data.first_name,
              last_name: evt.data.last_name,
              created_at: new Date(evt.data.created_at),
            },
          });
          break;
        }

        case 'user.updated': {
          await prisma.user.update({
            where: { id: evt.data.id },
            data: {
              email: evt.data.email_addresses[0]?.email_address,
              first_name: evt.data.first_name,
              last_name: evt.data.last_name,
              updated_at: new Date(),
            },
          });
          break;
        }

        case 'user.deleted': {
          await prisma.user.delete({
            where: { id: evt.data.id },
          });
          break;
        }

        default:
          console.log(`Unhandled Clerk event: ${eventType}`);
      }
    });

    return Response.json({ received: true }, { status: 200 });
  } catch (error) {
    console.error('Clerk webhook processing error:', error);
    return Response.json({ error: 'Processing failed' }, { status: 500 });
  }
}
```

### Step 3: Background Job (Cron)

```typescript
// app/api/cron/cleanup/route.ts
import { withSystemContext } from '@/lib/db/rls-helpers';
import { NextRequest } from 'next/server';

export async function GET(request: NextRequest) {
  // 1. Verify cron secret (Vercel Cron jobs)
  const authHeader = request.headers.get('authorization');
  if (authHeader !== `Bearer ${process.env.CRON_SECRET}`) {
    return Response.json({ error: 'Unauthorized' }, { status: 401 });
  }

  try {
    // 2. Execute cleanup with SYSTEM context
    const result = await withSystemContext(async (prisma) => {
      // Delete expired sessions older than 30 days
      const deletedSessions = await prisma.session.deleteMany({
        where: {
          expires_at: {
            lt: new Date(Date.now() - 30 * 24 * 60 * 60 * 1000),
          },
        },
      });

      // Archive old webhook events
      const archivedEvents = await prisma.webhookEvent.updateMany({
        where: {
          processed_at: {
            lt: new Date(Date.now() - 90 * 24 * 60 * 60 * 1000),
          },
          archived: false,
        },
        data: {
          archived: true,
        },
      });

      return {
        deleted_sessions: deletedSessions.count,
        archived_events: archivedEvents.count,
      };
    });

    return Response.json({ success: true, result }, { status: 200 });
  } catch (error) {
    console.error('Cron job error:', error);
    return Response.json({ error: 'Cron job failed' }, { status: 500 });
  }
}
```

## Security Checklist

- [ ] ✅ Webhook signature verified (Stripe, Clerk, etc.)
- [ ] ✅ System context used: `withSystemContext()` (no user_id needed)
- [ ] ✅ Webhook events logged for audit trail
- [ ] ✅ Error handling with proper HTTP status codes
- [ ] ✅ Idempotency considered (webhooks can be sent multiple times)
- [ ] ✅ Cron jobs use authorization secret
- [ ] ✅ No direct Prisma calls (uses withSystemContext)

## Testing

```typescript
// __tests__/api/webhooks/stripe.test.ts
import { POST } from '@/app/api/webhooks/stripe/route';
import Stripe from 'stripe';

const constructEventMock = jest.fn();

jest.mock('stripe', () => {
  return jest.fn().mockImplementation(() => ({
    webhooks: {
      constructEvent: constructEventMock,
    },
  }));
});

describe('Stripe Webhook', () => {
  it('returns 400 if signature missing', async () => {
    const request = new Request('http://localhost/api/webhooks/stripe', {
      method: 'POST',
      body: JSON.stringify({}),
    });

    const response = await POST(request);
    expect(response.status).toBe(400);
  });

  it('processes payment_intent.succeeded event', async () => {
    const event = {
      type: 'payment_intent.succeeded',
      data: {
        object: {
          id: 'pi_123',
          amount: 1000,
        },
      },
    };

    constructEventMock.mockReturnValue(event);

    const request = new Request('http://localhost/api/webhooks/stripe', {
      method: 'POST',
      headers: {
        'stripe-signature': 'valid_signature',
      },
      body: JSON.stringify(event),
    });

    const response = await POST(request);
    expect(response.status).toBe(200);
    const data = await response.json();
    expect(data.received).toBe(true);
  });
});
```

## Common Mistakes

### ❌ Mistake 1: Using withUserContext for webhooks
```typescript
// NO USER_ID IN WEBHOOKS!
const data = await withUserContext(userId, async (prisma) => {
  // Where does userId come from? Webhooks don't have auth!
});
```

### ✅ Fix: Use withSystemContext
```typescript
const data = await withSystemContext(async (prisma) => {
  // System operations without user_id
});
```

### ❌ Mistake 2: Not verifying webhook signature
```typescript
export async function POST(request: Request) {
  const body = await request.json();
  // Process without verifying signature - SECURITY RISK!
}
```

### ✅ Fix: Always verify signature
```typescript
export async function POST(request: Request) {
  const signature = headers().get('stripe-signature');
  const event = stripe.webhooks.constructEvent(body, signature, secret);
  // Now process verified event
}
```

## Related Patterns

- [User Context API](./user-context-api.md) - User-specific operations
- [Admin Context API](./admin-context-api.md) - Admin operations
- [RLS Implementation Guide](../../data/RLS_IMPLEMENTATION_GUIDE.md) - Complete RLS reference
