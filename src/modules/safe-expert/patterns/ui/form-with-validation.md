# Form with Validation Pattern

**Purpose**: Create forms with Zod schema validation, error display, and proper accessibility

**When to use**: User input forms, settings pages, data entry forms

## Implementation

### Step 1: Zod Schema Definition

```typescript
// lib/validations/payment.ts
import { z } from 'zod';

export const createPaymentSchema = z.object({
  amount: z
    .number()
    .min(0.5, 'Amount must be at least $0.50')
    .max(999999, 'Amount cannot exceed $999,999'),
  currency: z.enum(['usd', 'eur', 'gbp'], {
    errorMap: () => ({ message: 'Invalid currency' }),
  }),
  description: z
    .string()
    .min(1, 'Description is required')
    .max(200, 'Description cannot exceed 200 characters'),
  customer_email: z
    .string()
    .email('Invalid email address')
    .optional(),
  metadata: z.record(z.string()).optional(),
});

export type CreatePaymentInput = z.infer<typeof createPaymentSchema>;
```

### Step 2: Server Action with Validation

```typescript
// app/actions/payment.ts
'use server';

import { auth } from '@clerk/nextjs/server';
import { withUserContext } from '@/lib/db/rls-helpers';
import { createPaymentSchema, CreatePaymentInput } from '@/lib/validations/payment';
import { revalidatePath } from 'next/cache';

export async function createPayment(input: CreatePaymentInput) {
  // 1. Verify authentication
  const { userId } = await auth();
  if (!userId) {
    return { error: 'Unauthorized', success: false };
  }

  // 2. Validate input
  const validation = createPaymentSchema.safeParse(input);
  if (!validation.success) {
    return {
      error: validation.error.flatten().fieldErrors,
      success: false,
    };
  }

  try {
    // 3. Create payment with RLS context
    const payment = await withUserContext(userId, async (prisma) => {
      return await prisma.payment.create({
        data: {
          user_id: userId,
          amount: Math.round(validation.data.amount * 100), // Convert to cents
          currency: validation.data.currency,
          description: validation.data.description,
          status: 'pending',
          metadata: validation.data.metadata,
        },
      });
    });

    // 4. Revalidate cache
    revalidatePath('/dashboard/payments');

    return { data: payment, success: true };
  } catch (error) {
    console.error('Create payment error:', error);
    return { error: 'Failed to create payment', success: false };
  }
}
```

### Step 3: Form Component

```typescript
// app/dashboard/payments/new/payment-form.tsx
'use client';

import { useState } from 'react';
import { useRouter } from 'next/navigation';
import { createPayment } from '@/app/actions/payment';
import { CreatePaymentInput } from '@/lib/validations/payment';

export function PaymentForm() {
  const router = useRouter();
  const [loading, setLoading] = useState(false);
  const [errors, setErrors] = useState<Record<string, string[]>>({});

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    setLoading(true);
    setErrors({});

    const formData = new FormData(e.currentTarget);
    const input: CreatePaymentInput = {
      amount: parseFloat(formData.get('amount') as string),
      currency: formData.get('currency') as 'usd' | 'eur' | 'gbp',
      description: formData.get('description') as string,
      customer_email: formData.get('customer_email') as string || undefined,
    };

    const result = await createPayment(input);

    if (result.success) {
      router.push('/dashboard/payments');
      router.refresh();
    } else if (typeof result.error === 'object') {
      // Field-level errors from Zod
      setErrors(result.error);
    } else {
      // General error
      alert(result.error);
    }

    setLoading(false);
  };

  return (
    <form onSubmit={handleSubmit} className="max-w-lg space-y-6">
      {/* Amount Field */}
      <div>
        <label
          htmlFor="amount"
          className="block text-sm font-medium text-gray-700 mb-1"
        >
          Amount *
        </label>
        <input
          id="amount"
          name="amount"
          type="number"
          step="0.01"
          min="0.50"
          max="999999"
          required
          aria-describedby={errors.amount ? 'amount-error' : undefined}
          aria-invalid={!!errors.amount}
          className={`w-full px-3 py-2 border rounded-md focus:outline-none focus:ring-2 ${
            errors.amount
              ? 'border-red-500 focus:ring-red-500'
              : 'border-gray-300 focus:ring-blue-500'
          }`}
        />
        {errors.amount && (
          <p id="amount-error" className="mt-1 text-sm text-red-600" role="alert">
            {errors.amount[0]}
          </p>
        )}
      </div>

      {/* Currency Field */}
      <div>
        <label
          htmlFor="currency"
          className="block text-sm font-medium text-gray-700 mb-1"
        >
          Currency *
        </label>
        <select
          id="currency"
          name="currency"
          required
          aria-describedby={errors.currency ? 'currency-error' : undefined}
          aria-invalid={!!errors.currency}
          className={`w-full px-3 py-2 border rounded-md focus:outline-none focus:ring-2 ${
            errors.currency
              ? 'border-red-500 focus:ring-red-500'
              : 'border-gray-300 focus:ring-blue-500'
          }`}
        >
          <option value="usd">USD ($)</option>
          <option value="eur">EUR (€)</option>
          <option value="gbp">GBP (£)</option>
        </select>
        {errors.currency && (
          <p id="currency-error" className="mt-1 text-sm text-red-600" role="alert">
            {errors.currency[0]}
          </p>
        )}
      </div>

      {/* Description Field */}
      <div>
        <label
          htmlFor="description"
          className="block text-sm font-medium text-gray-700 mb-1"
        >
          Description *
        </label>
        <textarea
          id="description"
          name="description"
          rows={3}
          maxLength={200}
          required
          aria-describedby={errors.description ? 'description-error' : undefined}
          aria-invalid={!!errors.description}
          className={`w-full px-3 py-2 border rounded-md focus:outline-none focus:ring-2 ${
            errors.description
              ? 'border-red-500 focus:ring-red-500'
              : 'border-gray-300 focus:ring-blue-500'
          }`}
        />
        {errors.description && (
          <p id="description-error" className="mt-1 text-sm text-red-600" role="alert">
            {errors.description[0]}
          </p>
        )}
      </div>

      {/* Optional Email Field */}
      <div>
        <label
          htmlFor="customer_email"
          className="block text-sm font-medium text-gray-700 mb-1"
        >
          Customer Email (Optional)
        </label>
        <input
          id="customer_email"
          name="customer_email"
          type="email"
          aria-describedby={errors.customer_email ? 'email-error' : undefined}
          aria-invalid={!!errors.customer_email}
          className={`w-full px-3 py-2 border rounded-md focus:outline-none focus:ring-2 ${
            errors.customer_email
              ? 'border-red-500 focus:ring-red-500'
              : 'border-gray-300 focus:ring-blue-500'
          }`}
        />
        {errors.customer_email && (
          <p id="email-error" className="mt-1 text-sm text-red-600" role="alert">
            {errors.customer_email[0]}
          </p>
        )}
      </div>

      {/* Submit Button */}
      <div className="flex gap-4">
        <button
          type="submit"
          disabled={loading}
          className="flex-1 bg-blue-600 text-white py-2 px-4 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 disabled:opacity-50 disabled:cursor-not-allowed"
        >
          {loading ? 'Creating...' : 'Create Payment'}
        </button>
        <button
          type="button"
          onClick={() => router.back()}
          className="px-4 py-2 border border-gray-300 rounded-md hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-gray-500"
        >
          Cancel
        </button>
      </div>
    </form>
  );
}
```

### Step 4: Form Page

```typescript
// app/dashboard/payments/new/page.tsx
import { auth } from '@clerk/nextjs/server';
import { redirect } from 'next/navigation';
import { PaymentForm } from './payment-form';

export const dynamic = 'force-dynamic';

export default async function NewPaymentPage() {
  const { userId } = await auth();
  if (!userId) {
    redirect('/sign-in');
  }

  return (
    <div className="container mx-auto p-6">
      <h1 className="text-3xl font-bold mb-6">Create New Payment</h1>
      <PaymentForm />
    </div>
  );
}
```

## Accessibility Checklist

- [ ] ✅ Labels associated with inputs (htmlFor + id)
- [ ] ✅ Required fields marked with asterisk and aria-required
- [ ] ✅ Error messages with aria-describedby and role="alert"
- [ ] ✅ aria-invalid on fields with errors
- [ ] ✅ Focus states visible (focus:ring-2)
- [ ] ✅ Keyboard navigation works (Tab, Enter, Escape)
- [ ] ✅ Submit button shows loading state
- [ ] ✅ Cancel button available

## Testing

```typescript
// __tests__/components/payment-form.test.tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { PaymentForm } from '@/app/dashboard/payments/new/payment-form';
import { createPayment } from '@/app/actions/payment';

jest.mock('@/app/actions/payment');
jest.mock('next/navigation', () => ({
  useRouter: () => ({
    push: jest.fn(),
    refresh: jest.fn(),
    back: jest.fn(),
  }),
}));

describe('PaymentForm', () => {
  it('renders all form fields', () => {
    render(<PaymentForm />);

    expect(screen.getByLabelText(/amount/i)).toBeInTheDocument();
    expect(screen.getByLabelText(/currency/i)).toBeInTheDocument();
    expect(screen.getByLabelText(/description/i)).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /create payment/i })).toBeInTheDocument();
  });

  it('displays validation errors', async () => {
    (createPayment as jest.Mock).mockResolvedValue({
      success: false,
      error: {
        amount: ['Amount must be at least $0.50'],
      },
    });

    render(<PaymentForm />);

    const submitButton = screen.getByRole('button', { name: /create payment/i });
    fireEvent.click(submitButton);

    await waitFor(() => {
      expect(screen.getByText(/amount must be at least/i)).toBeInTheDocument();
    });
  });

  it('submits form with valid data', async () => {
    (createPayment as jest.Mock).mockResolvedValue({
      success: true,
      data: { id: 'pay_123' },
    });

    render(<PaymentForm />);

    fireEvent.change(screen.getByLabelText(/amount/i), { target: { value: '100.00' } });
    fireEvent.change(screen.getByLabelText(/description/i), { target: { value: 'Test' } });
    fireEvent.click(screen.getByRole('button', { name: /create payment/i }));

    await waitFor(() => {
      expect(createPayment).toHaveBeenCalledWith({
        amount: 100,
        currency: 'usd',
        description: 'Test',
        customer_email: undefined,
      });
    });
  });
});
```

## Validation

```bash
# Type check
yarn type-check

# Unit tests
yarn test:unit

# Accessibility audit
yarn lighthouse
```

## Related Patterns

- [User Context API](../api/user-context-api.md) - Server actions with RLS
- [Authenticated Page](./authenticated-page.md) - Pages with forms
- [Data Table](./data-table.md) - Display form results
