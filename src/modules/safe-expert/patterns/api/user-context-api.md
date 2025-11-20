# User Context API Pattern

**Purpose**: Authenticated API routes with Row Level Security (RLS) enforcement for user-specific data operations

**When to use**: Dashboard data, user-owned CRUD operations, protected endpoints requiring authentication

## Implementation

### Step 1: Route Handler Setup

```typescript
import { auth } from '@clerk/nextjs/server';
import { withUserContext } from '@/lib/db/rls-helpers';
import { z } from 'zod';

const schema = z.object({
  // Define your request shape
  limit: z.number().optional().default(10),
});

export async function GET(request: Request) {
  // 1. Verify authentication
  const { userId } = await auth();
  if (!userId) {
    return Response.json({ error: 'Unauthorized' }, { status: 401 });
  }

  // 2. Validate input
  const url = new URL(request.url);
  const limit = parseInt(url.searchParams.get('limit') || '10');
  const validated = schema.parse({ limit });

  // 3. Execute with RLS context
  const data = await withUserContext(userId, async (prisma) => {
    return await prisma.YOUR_TABLE.findMany({
      where: { user_id: userId },  // CRITICAL: Always filter by user_id
      take: validated.limit,
    });
  });

  return Response.json({ data }, { status: 200 });
}
```

### Step 2: Customization Guide

Replace the following placeholders:
- `YOUR_TABLE` → Your Prisma model name (e.g., `payment`, `subscription`)
- Schema validation → Add your actual request parameters
- Query logic → Customize `findMany`, `create`, `update`, `delete` as needed

## Security Checklist

- [ ] ✅ Uses `withUserContext()` for ALL database operations
- [ ] ✅ Authentication check with Clerk `auth()` before data access
- [ ] ✅ All queries include `user_id` filter
- [ ] ✅ Input validation with Zod schema
- [ ] ✅ NO direct Prisma calls (linter will catch this)

## Validation

```bash
# Run before committing
yarn ci:validate

# Individual checks
yarn lint          # Catches direct Prisma usage
yarn type-check    # Validates types
yarn test:integration  # Tests RLS enforcement
```

## Examples from Codebase

**GET user payments**: `app/api/user/payments/route.ts`
**POST create subscription**: `app/api/user/subscriptions/route.ts`

## Common Mistakes

❌ **NEVER do direct Prisma calls**:
```typescript
const data = await prisma.payment.findMany({ where: { user_id: userId } });
```

✅ **ALWAYS use RLS context**:
```typescript
const data = await withUserContext(userId, async (prisma) => {
  return await prisma.payment.findMany({ where: { user_id: userId } });
});
```

## Related Patterns

- [Admin Context API](./admin-context-api.md) - For admin-only operations
- [Webhook Handler](./webhook-handler.md) - For system/webhook operations
- [Zod Validation API](./zod-validation-api.md) - Advanced validation patterns
