# API Integration Test Pattern

**Purpose**: Test API endpoints with RLS context helpers and verify user isolation

**When to use**: Integration tests for API routes, RLS verification, database operations

## Implementation

### Step 1: Setup Test Database

```typescript
// __tests__/setup/test-db.ts
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient({
  datasources: {
    db: {
      url: process.env.TEST_DATABASE_URL,
    },
  },
});

export async function setupTestDatabase() {
  // Run migrations
  await prisma.$executeRawUnsafe('DROP SCHEMA public CASCADE');
  await prisma.$executeRawUnsafe('CREATE SCHEMA public');
  // Run: npx prisma migrate deploy
}

export async function cleanupTestDatabase() {
  const tables = await prisma.$queryRaw`
    SELECT tablename FROM pg_tables WHERE schemaname='public'
  `;

  for (const { tablename } of tables) {
    if (tablename !== '_prisma_migrations') {
      await prisma.$executeRawUnsafe(`TRUNCATE TABLE "${tablename}" CASCADE`);
    }
  }
}

export { prisma as testPrisma };
```

### Step 2: API Integration Test with RLS

```typescript
// __tests__/api/payments.test.ts
import { GET, POST } from '@/app/api/payments/route';
import { auth } from '@clerk/nextjs/server';
import { testPrisma, cleanupTestDatabase } from '../setup/test-db';

jest.mock('@clerk/nextjs/server');

describe('Payments API - RLS Integration', () => {
  beforeEach(async () => {
    await cleanupTestDatabase();
  });

  afterAll(async () => {
    await testPrisma.$disconnect();
  });

  describe('GET /api/payments', () => {
    it('returns 401 if not authenticated', async () => {
      (auth as jest.Mock).mockResolvedValue({ userId: null });

      const request = new Request('http://localhost/api/payments');
      const response = await GET(request);

      expect(response.status).toBe(401);
      const data = await response.json();
      expect(data.error).toBe('Unauthorized');
    });

    it('returns only current user payments (RLS isolation)', async () => {
      // Setup: Create payments for multiple users
      const user1Id = 'user_123';
      const user2Id = 'user_456';

      await testPrisma.payment.createMany({
        data: [
          { id: 'pay_1', user_id: user1Id, amount: 1000, status: 'succeeded' },
          { id: 'pay_2', user_id: user1Id, amount: 2000, status: 'succeeded' },
          { id: 'pay_3', user_id: user2Id, amount: 3000, status: 'succeeded' },
        ],
      });

      // Act: Request as user1
      (auth as jest.Mock).mockResolvedValue({ userId: user1Id });

      const request = new Request('http://localhost/api/payments');
      const response = await GET(request);

      // Assert: Only user1's payments returned
      expect(response.status).toBe(200);
      const data = await response.json();
      expect(data.data).toHaveLength(2);
      expect(data.data.every(p => p.user_id === user1Id)).toBe(true);
      expect(data.data.find(p => p.id === 'pay_3')).toBeUndefined(); // user2's payment not visible
    });

    it('respects query parameters', async () => {
      const userId = 'user_123';

      await testPrisma.payment.createMany({
        data: [
          { id: 'pay_1', user_id: userId, amount: 1000, status: 'succeeded' },
          { id: 'pay_2', user_id: userId, amount: 2000, status: 'pending' },
          { id: 'pay_3', user_id: userId, amount: 3000, status: 'succeeded' },
        ],
      });

      (auth as jest.Mock).mockResolvedValue({ userId });

      const request = new Request('http://localhost/api/payments?limit=2');
      const response = await GET(request);

      expect(response.status).toBe(200);
      const data = await response.json();
      expect(data.data).toHaveLength(2);
    });
  });

  describe('POST /api/payments', () => {
    it('creates payment with user_id from auth', async () => {
      const userId = 'user_123';
      (auth as jest.Mock).mockResolvedValue({ userId });

      const requestBody = {
        amount: 5000,
        currency: 'usd',
        description: 'Test payment',
      };

      const request = new Request('http://localhost/api/payments', {
        method: 'POST',
        body: JSON.stringify(requestBody),
      });

      const response = await POST(request);

      expect(response.status).toBe(201);
      const data = await response.json();
      expect(data.data.user_id).toBe(userId);
      expect(data.data.amount).toBe(5000);

      // Verify in database
      const payment = await testPrisma.payment.findUnique({
        where: { id: data.data.id },
      });
      expect(payment.user_id).toBe(userId);
    });

    it('validates input with Zod schema', async () => {
      const userId = 'user_123';
      (auth as jest.Mock).mockResolvedValue({ userId });

      const invalidBody = {
        amount: -100, // Invalid: negative amount
        currency: 'invalid',
        description: '',
      };

      const request = new Request('http://localhost/api/payments', {
        method: 'POST',
        body: JSON.stringify(invalidBody),
      });

      const response = await POST(request);

      expect(response.status).toBe(400);
      const data = await response.json();
      expect(data.error).toContain('validation');
    });
  });

  describe('RLS Policy Verification', () => {
    it('prevents user from accessing another users payment directly', async () => {
      // Setup: Create payment for user2
      const user1Id = 'user_123';
      const user2Id = 'user_456';

      const payment = await testPrisma.payment.create({
        data: {
          id: 'pay_other',
          user_id: user2Id,
          amount: 9999,
          status: 'succeeded',
        },
      });

      // Act: Try to access as user1 (should fail due to RLS)
      (auth as jest.Mock).mockResolvedValue({ userId: user1Id });

      // Simulate direct query (this would be in your API route using withUserContext)
      const result = await testPrisma.$executeRawUnsafe(`
        SET LOCAL app.current_user_id = '${user1Id}';
        SELECT * FROM "Payment" WHERE id = '${payment.id}';
      `);

      // Assert: Query returns no rows due to RLS
      expect(result).toEqual([]); // RLS blocks access
    });
  });
});
```

### Step 3: Admin Context Integration Test

```typescript
// __tests__/api/admin/users.test.ts
import { GET } from '@/app/api/admin/users/route';
import { auth } from '@clerk/nextjs/server';
import { testPrisma, cleanupTestDatabase } from '../../setup/test-db';

jest.mock('@clerk/nextjs/server');

describe('Admin Users API - RLS Integration', () => {
  beforeEach(async () => {
    await cleanupTestDatabase();
  });

  afterAll(async () => {
    await testPrisma.$disconnect();
  });

  it('returns 403 if user is not admin', async () => {
    const userId = 'user_123';
    (auth as jest.Mock).mockResolvedValue({ userId });

    // User exists but no admin role
    await testPrisma.user.create({
      data: { id: userId, email: 'user@example.com' },
    });

    const request = new Request('http://localhost/api/admin/users');
    const response = await GET(request);

    expect(response.status).toBe(403);
    const data = await response.json();
    expect(data.error).toContain('Admin role required');
  });

  it('returns all users for admin', async () => {
    const adminId = 'admin_123';
    (auth as jest.Mock).mockResolvedValue({ userId: adminId });

    // Create admin user with role
    await testPrisma.user.create({
      data: { id: adminId, email: 'admin@example.com' },
    });

    await testPrisma.userRole.create({
      data: {
        user_id: adminId,
        role: 'admin',
      },
    });

    // Create other users
    await testPrisma.user.createMany({
      data: [
        { id: 'user_1', email: 'user1@example.com' },
        { id: 'user_2', email: 'user2@example.com' },
      ],
    });

    const request = new Request('http://localhost/api/admin/users');
    const response = await GET(request);

    expect(response.status).toBe(200);
    const data = await response.json();
    expect(data.data).toHaveLength(3); // Admin sees all users
  });
});
```

## Test Configuration

### jest.config.js

```javascript
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  setupFilesAfterEnv: ['<rootDir>/__tests__/setup/test-db.ts'],
  testMatch: ['**/__tests__/**/*.test.ts'],
  collectCoverageFrom: [
    'app/**/*.{ts,tsx}',
    'lib/**/*.{ts,tsx}',
    '!**/*.d.ts',
    '!**/node_modules/**',
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
};
```

### package.json scripts

```json
{
  "scripts": {
    "test:integration": "NODE_ENV=test jest --runInBand",
    "test:integration:watch": "NODE_ENV=test jest --watch --runInBand",
    "test:integration:coverage": "NODE_ENV=test jest --coverage --runInBand"
  }
}
```

## Validation

```bash
# Run integration tests
yarn test:integration

# Run with coverage
yarn test:integration:coverage

# Expected output:
# PASS  __tests__/api/payments.test.ts
#   Payments API - RLS Integration
#     GET /api/payments
#       ✓ returns 401 if not authenticated (45ms)
#       ✓ returns only current user payments (RLS isolation) (89ms)
#       ✓ respects query parameters (67ms)
#     POST /api/payments
#       ✓ creates payment with user_id from auth (112ms)
#       ✓ validates input with Zod schema (34ms)
#     RLS Policy Verification
#       ✓ prevents user from accessing another users payment directly (78ms)
#
# Test Suites: 1 passed, 1 total
# Tests:       6 passed, 6 total
# Coverage:    85.3% (meets 80% threshold)
```

## Related Patterns

- [User Context API](../api/user-context-api.md) - API pattern being tested
- [E2E User Flow](./e2e-user-flow.md) - End-to-end testing
- [RLS Implementation Guide](../../data/RLS_IMPLEMENTATION_GUIDE.md) - RLS reference
