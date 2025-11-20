# Admin Context API Pattern

**Purpose**: Create API endpoints requiring admin authentication with Row Level Security (RLS) admin context

**When to use**: Admin-only operations, viewing all users' data, system management endpoints

## Implementation

### Step 1: API Route with Admin Context

```typescript
// app/api/admin/users/route.ts
import { auth } from '@clerk/nextjs/server';
import { withAdminContext } from '@/lib/db/rls-helpers';
import { z } from 'zod';

const querySchema = z.object({
  limit: z.number().optional().default(50),
  offset: z.number().optional().default(0),
});

export async function GET(request: Request) {
  // 1. Verify authentication
  const { userId } = await auth();
  if (!userId) {
    return Response.json({ error: 'Unauthorized' }, { status: 401 });
  }

  // 2. Validate input
  const url = new URL(request.url);
  const limit = parseInt(url.searchParams.get('limit') || '50');
  const offset = parseInt(url.searchParams.get('offset') || '0');
  const validated = querySchema.parse({ limit, offset });

  try {
    // 3. Execute with ADMIN RLS context (verifies admin role)
    const data = await withAdminContext(userId, async (prisma) => {
      return await prisma.user.findMany({
        take: validated.limit,
        skip: validated.offset,
        // Admin can see ALL users - no user_id filter needed
        select: {
          id: true,
          email: true,
          created_at: true,
          // Don't expose sensitive fields unnecessarily
        },
      });
    });

    return Response.json({ data, count: data.length }, { status: 200 });
  } catch (error) {
    // withAdminContext throws if user doesn't have 'admin' role
    if (error.message?.includes('admin')) {
      return Response.json({ error: 'Forbidden: Admin role required' }, { status: 403 });
    }
    console.error('Admin API error:', error);
    return Response.json({ error: 'Internal server error' }, { status: 500 });
  }
}

export async function POST(request: Request) {
  const { userId } = await auth();
  if (!userId) {
    return Response.json({ error: 'Unauthorized' }, { status: 401 });
  }

  const body = await request.json();
  const validated = createUserSchema.parse(body);

  try {
    const data = await withAdminContext(userId, async (prisma) => {
      return await prisma.user.create({
        data: validated,
      });
    });

    return Response.json({ data }, { status: 201 });
  } catch (error) {
    if (error.message?.includes('admin')) {
      return Response.json({ error: 'Forbidden: Admin role required' }, { status: 403 });
    }
    console.error('Admin create error:', error);
    return Response.json({ error: 'Internal server error' }, { status: 500 });
  }
}
```

### Step 2: Zod Validation Schema

```typescript
// lib/validations/admin.ts
import { z } from 'zod';

export const createUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
  role: z.enum(['user', 'admin']).default('user'),
});

export const updateUserSchema = z.object({
  email: z.string().email().optional(),
  name: z.string().min(1).max(100).optional(),
  role: z.enum(['user', 'admin']).optional(),
}).refine(data => Object.keys(data).length > 0, {
  message: 'At least one field must be provided',
});
```

### Step 3: Admin Role Verification

The `withAdminContext()` helper automatically verifies admin role:

```typescript
// lib/db/rls-helpers.ts (already implemented)
export async function withAdminContext<T>(
  adminUserId: string,
  callback: (prisma: PrismaClient) => Promise<T>
): Promise<T> {
  // Verify user has 'admin' role
  const adminRole = await prisma.userRole.findFirst({
    where: {
      user_id: adminUserId,
      role: 'admin',
    },
  });

  if (!adminRole) {
    throw new Error('User does not have admin role');
  }

  // Set admin context and execute
  return await prisma.$executeRawUnsafe(
    `SET LOCAL app.current_user_id = '${adminUserId}'`
  ).then(() => callback(prisma));
}
```

## Security Checklist

- [ ] ✅ Authentication verified with `await auth()` from Clerk
- [ ] ✅ Admin role verified by `withAdminContext()` (checks user_roles table)
- [ ] ✅ Input validated with Zod schema
- [ ] ✅ Error handling for non-admin users (403 Forbidden)
- [ ] ✅ Don't expose unnecessary sensitive fields in responses
- [ ] ✅ Proper HTTP status codes (401, 403, 500)
- [ ] ✅ No direct Prisma calls (uses withAdminContext)

## Testing

```typescript
// __tests__/api/admin/users.test.ts
import { POST, GET } from '@/app/api/admin/users/route';
import { auth } from '@clerk/nextjs/server';

jest.mock('@clerk/nextjs/server');

describe('Admin Users API', () => {
  it('returns 401 if not authenticated', async () => {
    (auth as jest.Mock).mockResolvedValue({ userId: null });

    const response = await GET(new Request('http://localhost/api/admin/users'));
    expect(response.status).toBe(401);
  });

  it('returns 403 if not admin', async () => {
    (auth as jest.Mock).mockResolvedValue({ userId: 'user_123' });
    // withAdminContext will throw when checking user_roles table

    const response = await GET(new Request('http://localhost/api/admin/users'));
    expect(response.status).toBe(403);
  });

  it('returns all users for admin', async () => {
    (auth as jest.Mock).mockResolvedValue({ userId: 'admin_456' });
    // Mock admin role in database

    const response = await GET(new Request('http://localhost/api/admin/users?limit=10'));
    expect(response.status).toBe(200);
    const data = await response.json();
    expect(data.data).toBeInstanceOf(Array);
  });
});
```

## Common Mistakes

### ❌ Mistake 1: Using withUserContext for admin operations
```typescript
const data = await withUserContext(userId, async (prisma) => {
  return await prisma.user.findMany();  // Only returns current user's data!
});
```

### ✅ Fix: Use withAdminContext
```typescript
const data = await withAdminContext(userId, async (prisma) => {
  return await prisma.user.findMany();  // Returns ALL users (admin can see all)
});
```

### ❌ Mistake 2: Not handling admin role verification errors
```typescript
const data = await withAdminContext(userId, async (prisma) => {
  // ...
});
// No try/catch - user gets confusing error
```

### ✅ Fix: Handle admin verification errors
```typescript
try {
  const data = await withAdminContext(userId, async (prisma) => {
    // ...
  });
} catch (error) {
  if (error.message?.includes('admin')) {
    return Response.json({ error: 'Forbidden: Admin role required' }, { status: 403 });
  }
  throw error;
}
```

## Related Patterns

- [User Context API](./user-context-api.md) - User-specific operations
- [Webhook Handler](./webhook-handler.md) - System context for webhooks
- [RLS Implementation Guide](../../data/RLS_IMPLEMENTATION_GUIDE.md) - Complete RLS reference
