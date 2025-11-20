# E2E User Flow Test Pattern

**Purpose**: Test complete user journeys end-to-end with authentication and RLS isolation

**When to use**: Critical user flows, authentication flows, multi-step processes

## Implementation

### Step 1: Playwright Setup

```typescript
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './__tests__/e2e',
  fullyParallel: false, // Run sequentially to avoid test database conflicts
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : 1,
  reporter: 'html',
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },

  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
  ],

  webServer: {
    command: 'npm run dev',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
  },
});
```

### Step 2: Authentication Helper

```typescript
// __tests__/e2e/helpers/auth.ts
import { Page } from '@playwright/test';

export async function signIn(page: Page, email: string, password: string) {
  await page.goto('/sign-in');

  // Clerk sign-in form
  await page.fill('input[name="identifier"]', email);
  await page.click('button[type="submit"]');

  await page.fill('input[name="password"]', password);
  await page.click('button[type="submit"]');

  // Wait for redirect to dashboard
  await page.waitForURL('/dashboard');
}

export async function signOut(page: Page) {
  await page.click('[data-testid="user-menu"]');
  await page.click('text=Sign out');
  await page.waitForURL('/');
}

export async function createTestUser(email: string, password: string) {
  // Use Clerk API to create test user
  const response = await fetch('https://api.clerk.dev/v1/users', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${process.env.CLERK_SECRET_KEY}`,
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      email_address: [email],
      password,
    }),
  });

  return await response.json();
}
```

### Step 3: Payment Flow E2E Test

```typescript
// __tests__/e2e/payment-flow.test.ts
import { test, expect } from '@playwright/test';
import { signIn, signOut, createTestUser } from './helpers/auth';
import { testPrisma, cleanupTestDatabase } from '../setup/test-db';

test.describe('Payment Flow - E2E', () => {
  let testUser: any;
  const testEmail = `test-${Date.now()}@example.com`;
  const testPassword = 'TestPassword123!';

  test.beforeAll(async () => {
    // Create test user in Clerk
    testUser = await createTestUser(testEmail, testPassword);
  });

  test.beforeEach(async () => {
    await cleanupTestDatabase();
  });

  test.afterAll(async () => {
    await testPrisma.$disconnect();
  });

  test('complete payment flow from dashboard to success', async ({ page }) => {
    // Step 1: Sign in
    await signIn(page, testEmail, testPassword);

    // Step 2: Navigate to payments page
    await page.click('text=Payments');
    await expect(page).toHaveURL('/dashboard/payments');

    // Verify empty state
    await expect(page.locator('text=No payments yet')).toBeVisible();

    // Step 3: Click "Make Payment" button
    await page.click('button:has-text("Make Payment")');
    await expect(page).toHaveURL('/dashboard/payments/new');

    // Step 4: Fill payment form
    await page.fill('input[name="amount"]', '100.00');
    await page.fill('input[name="description"]', 'Test payment');

    // Step 5: Submit payment
    await page.click('button[type="submit"]:has-text("Pay Now")');

    // Step 6: Wait for Stripe checkout (in test mode, auto-succeeds)
    await page.waitForURL('/dashboard/payments/success*');

    // Step 7: Verify success message
    await expect(page.locator('text=Payment successful')).toBeVisible();

    // Step 8: Navigate back to payments list
    await page.click('a:has-text("View Payments")');
    await expect(page).toHaveURL('/dashboard/payments');

    // Step 9: Verify payment appears in list
    const paymentRow = page.locator('[data-testid="payment-row"]').first();
    await expect(paymentRow).toBeVisible();
    await expect(paymentRow.locator('text=succeeded')).toBeVisible();
    await expect(paymentRow.locator('text=$100.00')).toBeVisible();

    // Step 10: Verify RLS isolation - create payment for another user
    const otherUserId = 'user_other_123';
    await testPrisma.payment.create({
      data: {
        id: 'pay_other',
        user_id: otherUserId,
        amount: 9999,
        status: 'succeeded',
        description: 'Other user payment',
      },
    });

    // Refresh page - should NOT see other user's payment
    await page.reload();
    const payments = await page.locator('[data-testid="payment-row"]').count();
    expect(payments).toBe(1); // Only current user's payment visible

    // Verify other user's payment is not visible
    await expect(page.locator('text=Other user payment')).not.toBeVisible();
  });

  test('user cannot access another users payment details', async ({ page }) => {
    // Setup: Create payment for another user
    const otherUserId = 'user_other_456';
    const otherPayment = await testPrisma.payment.create({
      data: {
        id: 'pay_other_secret',
        user_id: otherUserId,
        amount: 5000,
        status: 'succeeded',
        description: 'Secret payment',
      },
    });

    // Sign in as test user
    await signIn(page, testEmail, testPassword);

    // Try to directly navigate to other user's payment
    await page.goto(`/dashboard/payments/${otherPayment.id}`);

    // Should see 404 or unauthorized message (RLS blocks access)
    await expect(page.locator('text=Payment not found')).toBeVisible();
  });

  test('admin can view all payments', async ({ page }) => {
    // Create admin user
    const adminEmail = `admin-${Date.now()}@example.com`;
    const adminPassword = 'AdminPassword123!';
    const adminUser = await createTestUser(adminEmail, adminPassword);

    // Grant admin role in database
    await testPrisma.userRole.create({
      data: {
        user_id: adminUser.id,
        role: 'admin',
      },
    });

    // Create payments for multiple users
    await testPrisma.payment.createMany({
      data: [
        { id: 'pay_1', user_id: testUser.id, amount: 1000, status: 'succeeded' },
        { id: 'pay_2', user_id: 'user_other_1', amount: 2000, status: 'succeeded' },
        { id: 'pay_3', user_id: 'user_other_2', amount: 3000, status: 'succeeded' },
      ],
    });

    // Sign in as admin
    await signIn(page, adminEmail, adminPassword);

    // Navigate to admin payments page
    await page.goto('/admin/payments');

    // Verify admin sees ALL payments
    const paymentRows = await page.locator('[data-testid="payment-row"]').count();
    expect(paymentRows).toBe(3); // Admin sees all 3 payments
  });
});
```

### Step 4: Multi-Step Form E2E Test

```typescript
// __tests__/e2e/onboarding-flow.test.ts
import { test, expect } from '@playwright/test';
import { signIn } from './helpers/auth';

test.describe('Onboarding Flow - E2E', () => {
  test('complete onboarding from signup to dashboard', async ({ page }) => {
    const email = `onboard-${Date.now()}@example.com`;

    // Step 1: Sign up
    await page.goto('/sign-up');
    await page.fill('input[name="emailAddress"]', email);
    await page.fill('input[name="password"]', 'TestPassword123!');
    await page.click('button[type="submit"]');

    // Step 2: Verify email (in test mode, auto-verifies)
    await page.waitForURL('/onboarding');

    // Step 3: Complete profile
    await page.fill('input[name="firstName"]', 'Test');
    await page.fill('input[name="lastName"]', 'User');
    await page.click('button:has-text("Next")');

    // Step 4: Select plan
    await page.click('[data-testid="plan-pro"]');
    await page.click('button:has-text("Next")');

    // Step 5: Complete setup
    await page.click('button:has-text("Finish Setup")');

    // Step 6: Verify redirect to dashboard
    await page.waitForURL('/dashboard');
    await expect(page.locator('text=Welcome, Test!')).toBeVisible();
  });
});
```

## Validation

```bash
# Run E2E tests
npx playwright test

# Run specific test file
npx playwright test payment-flow.test.ts

# Run with UI
npx playwright test --ui

# Expected output:
# Running 3 tests using 1 worker
#
# ✓ complete payment flow from dashboard to success (5.2s)
# ✓ user cannot access another users payment details (2.1s)
# ✓ admin can view all payments (3.8s)
#
# 3 passed (11s)
```

## Related Patterns

- [API Integration Test](./api-integration-test.md) - Test API endpoints
- [User Context API](../api/user-context-api.md) - API pattern being tested
- [Authenticated Page](../ui/authenticated-page.md) - UI pattern being tested
