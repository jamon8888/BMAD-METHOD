# Data Table Pattern

**Purpose**: Display user data in tables with sorting, filtering, and pagination

**When to use**: Lists of payments, users, orders, any tabular data

## Implementation

### Step 1: Server Component with Data Fetching

```typescript
// app/dashboard/payments/page.tsx
import { auth } from '@clerk/nextjs/server';
import { redirect } from 'next/navigation';
import { withUserContext } from '@/lib/db/rls-helpers';
import { PaymentsTable } from './payments-table';

export const dynamic = 'force-dynamic';

interface SearchParams {
  page?: string;
  limit?: string;
  sort?: string;
  order?: 'asc' | 'desc';
  status?: string;
}

export default async function PaymentsPage({
  searchParams,
}: {
  searchParams: SearchParams;
}) {
  const { userId } = await auth();
  if (!userId) {
    redirect('/sign-in');
  }

  // Parse query parameters
  const page = parseInt(searchParams.page || '1');
  const limit = parseInt(searchParams.limit || '10');
  const sort = searchParams.sort || 'created_at';
  const order = searchParams.order || 'desc';
  const status = searchParams.status;

  // Fetch data with RLS context
  const { payments, total } = await withUserContext(userId, async (prisma) => {
    const where = {
      user_id: userId,
      ...(status && { status }),
    };

    const [payments, total] = await Promise.all([
      prisma.payment.findMany({
        where,
        take: limit,
        skip: (page - 1) * limit,
        orderBy: { [sort]: order },
      }),
      prisma.payment.count({ where }),
    ]);

    return { payments, total };
  });

  return (
    <div className="container mx-auto p-6">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-3xl font-bold">Payments</h1>
        <a
          href="/dashboard/payments/new"
          className="bg-blue-600 text-white px-4 py-2 rounded-md hover:bg-blue-700"
        >
          New Payment
        </a>
      </div>

      <PaymentsTable
        payments={payments}
        total={total}
        page={page}
        limit={limit}
        sort={sort}
        order={order}
      />
    </div>
  );
}
```

### Step 2: Client Table Component

```typescript
// app/dashboard/payments/payments-table.tsx
'use client';

import { useRouter, useSearchParams } from 'next/navigation';
import { Payment } from '@prisma/client';

interface PaymentsTableProps {
  payments: Payment[];
  total: number;
  page: number;
  limit: number;
  sort: string;
  order: 'asc' | 'desc';
}

export function PaymentsTable({
  payments,
  total,
  page,
  limit,
  sort,
  order,
}: PaymentsTableProps) {
  const router = useRouter();
  const searchParams = useSearchParams();

  const updateQuery = (key: string, value: string) => {
    const params = new URLSearchParams(searchParams);
    params.set(key, value);
    router.push(`?${params.toString()}`);
  };

  const handleSort = (column: string) => {
    const newOrder = sort === column && order === 'asc' ? 'desc' : 'asc';
    updateQuery('sort', column);
    updateQuery('order', newOrder);
  };

  const totalPages = Math.ceil(total / limit);

  if (payments.length === 0) {
    return (
      <div className="text-center py-12">
        <p className="text-gray-500 mb-4">No payments yet</p>
        <a
          href="/dashboard/payments/new"
          className="text-blue-600 hover:underline"
        >
          Create your first payment
        </a>
      </div>
    );
  }

  return (
    <div className="space-y-4">
      {/* Filters */}
      <div className="flex gap-4">
        <select
          className="px-3 py-2 border rounded-md"
          onChange={(e) => updateQuery('status', e.target.value)}
          defaultValue={searchParams.get('status') || ''}
        >
          <option value="">All Statuses</option>
          <option value="pending">Pending</option>
          <option value="succeeded">Succeeded</option>
          <option value="failed">Failed</option>
        </select>

        <select
          className="px-3 py-2 border rounded-md"
          onChange={(e) => updateQuery('limit', e.target.value)}
          defaultValue={limit.toString()}
        >
          <option value="10">10 per page</option>
          <option value="25">25 per page</option>
          <option value="50">50 per page</option>
          <option value="100">100 per page</option>
        </select>
      </div>

      {/* Table */}
      <div className="overflow-x-auto">
        <table className="w-full border-collapse bg-white shadow rounded-lg">
          <thead className="bg-gray-50">
            <tr>
              <SortableHeader
                label="ID"
                column="id"
                currentSort={sort}
                currentOrder={order}
                onSort={handleSort}
              />
              <SortableHeader
                label="Amount"
                column="amount"
                currentSort={sort}
                currentOrder={order}
                onSort={handleSort}
              />
              <SortableHeader
                label="Status"
                column="status"
                currentSort={sort}
                currentOrder={order}
                onSort={handleSort}
              />
              <SortableHeader
                label="Description"
                column="description"
                currentSort={sort}
                currentOrder={order}
                onSort={handleSort}
              />
              <SortableHeader
                label="Created"
                column="created_at"
                currentSort={sort}
                currentOrder={order}
                onSort={handleSort}
              />
              <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">
                Actions
              </th>
            </tr>
          </thead>
          <tbody className="divide-y divide-gray-200">
            {payments.map((payment) => (
              <tr
                key={payment.id}
                data-testid="payment-row"
                className="hover:bg-gray-50"
              >
                <td className="px-6 py-4 text-sm text-gray-900">
                  {payment.id.substring(0, 8)}...
                </td>
                <td className="px-6 py-4 text-sm text-gray-900">
                  ${(payment.amount / 100).toFixed(2)}
                </td>
                <td className="px-6 py-4">
                  <StatusBadge status={payment.status} />
                </td>
                <td className="px-6 py-4 text-sm text-gray-900">
                  {payment.description}
                </td>
                <td className="px-6 py-4 text-sm text-gray-500">
                  {new Date(payment.created_at).toLocaleDateString()}
                </td>
                <td className="px-6 py-4">
                  <a
                    href={`/dashboard/payments/${payment.id}`}
                    className="text-blue-600 hover:underline text-sm"
                  >
                    View
                  </a>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>

      {/* Pagination */}
      <div className="flex items-center justify-between">
        <p className="text-sm text-gray-700">
          Showing <span className="font-medium">{(page - 1) * limit + 1}</span> to{' '}
          <span className="font-medium">
            {Math.min(page * limit, total)}
          </span>{' '}
          of <span className="font-medium">{total}</span> results
        </p>

        <div className="flex gap-2">
          <button
            onClick={() => updateQuery('page', (page - 1).toString())}
            disabled={page === 1}
            className="px-3 py-1 border rounded-md disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-50"
          >
            Previous
          </button>
          {[...Array(totalPages)].map((_, i) => (
            <button
              key={i + 1}
              onClick={() => updateQuery('page', (i + 1).toString())}
              className={`px-3 py-1 border rounded-md ${
                page === i + 1
                  ? 'bg-blue-600 text-white'
                  : 'hover:bg-gray-50'
              }`}
            >
              {i + 1}
            </button>
          ))}
          <button
            onClick={() => updateQuery('page', (page + 1).toString())}
            disabled={page === totalPages}
            className="px-3 py-1 border rounded-md disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-50"
          >
            Next
          </button>
        </div>
      </div>
    </div>
  );
}

function SortableHeader({
  label,
  column,
  currentSort,
  currentOrder,
  onSort,
}: {
  label: string;
  column: string;
  currentSort: string;
  currentOrder: 'asc' | 'desc';
  onSort: (column: string) => void;
}) {
  const isSorted = currentSort === column;

  return (
    <th
      className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase cursor-pointer hover:bg-gray-100"
      onClick={() => onSort(column)}
    >
      <div className="flex items-center gap-1">
        {label}
        {isSorted && (
          <span className="text-blue-600">
            {currentOrder === 'asc' ? '↑' : '↓'}
          </span>
        )}
      </div>
    </th>
  );
}

function StatusBadge({ status }: { status: string }) {
  const colors = {
    pending: 'bg-yellow-100 text-yellow-800',
    succeeded: 'bg-green-100 text-green-800',
    failed: 'bg-red-100 text-red-800',
  };

  return (
    <span
      className={`px-2 py-1 text-xs font-medium rounded-full ${
        colors[status] || 'bg-gray-100 text-gray-800'
      }`}
    >
      {status}
    </span>
  );
}
```

## Accessibility Checklist

- [ ] ✅ Table has proper semantic HTML (table, thead, tbody, tr, th, td)
- [ ] ✅ Column headers use th elements
- [ ] ✅ Sortable columns have cursor:pointer and hover state
- [ ] ✅ Keyboard navigation works (Tab through rows)
- [ ] ✅ Empty state provides helpful message
- [ ] ✅ Pagination buttons disabled when not applicable
- [ ] ✅ Status badges have sufficient color contrast

## Testing

```typescript
// __tests__/components/payments-table.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { PaymentsTable } from '@/app/dashboard/payments/payments-table';

jest.mock('next/navigation', () => ({
  useRouter: () => ({ push: jest.fn() }),
  useSearchParams: () => new URLSearchParams(),
}));

describe('PaymentsTable', () => {
  const mockPayments = [
    {
      id: 'pay_1',
      user_id: 'user_123',
      amount: 10000,
      status: 'succeeded',
      description: 'Test payment',
      created_at: new Date('2024-01-01'),
    },
  ];

  it('renders table with payments', () => {
    render(
      <PaymentsTable
        payments={mockPayments}
        total={1}
        page={1}
        limit={10}
        sort="created_at"
        order="desc"
      />
    );

    expect(screen.getByText('$100.00')).toBeInTheDocument();
    expect(screen.getByText('Test payment')).toBeInTheDocument();
  });

  it('shows empty state when no payments', () => {
    render(
      <PaymentsTable
        payments={[]}
        total={0}
        page={1}
        limit={10}
        sort="created_at"
        order="desc"
      />
    );

    expect(screen.getByText(/no payments yet/i)).toBeInTheDocument();
  });
});
```

## Validation

```bash
# Type check
yarn type-check

# Unit tests
yarn test:unit

# Lighthouse accessibility
yarn lighthouse
```

## Related Patterns

- [User Context API](../api/user-context-api.md) - Fetch table data
- [Authenticated Page](./authenticated-page.md) - Page with table
- [Form with Validation](./form-with-validation.md) - Create/edit forms
