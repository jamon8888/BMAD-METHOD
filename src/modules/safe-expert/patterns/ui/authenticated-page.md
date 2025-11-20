# Authenticated Page Pattern

**Purpose**: Create Next.js pages requiring Clerk authentication with proper redirects

**When to use**: Dashboard pages, user profile pages, any page requiring authentication

## Implementation

### Step 1: App Router Page with Clerk Auth

```typescript
// app/dashboard/page.tsx
import { auth, currentUser } from '@clerk/nextjs/server';
import { redirect } from 'next/navigation';
import { withUserContext } from '@/lib/db/rls-helpers';
import { prisma } from '@/lib/db/prisma';

export const dynamic = 'force-dynamic'; // REQUIRED for authentication

export default async function DashboardPage() {
  // 1. Verify authentication
  const { userId } = await auth();
  if (!userId) {
    redirect('/sign-in');
  }

  // 2. Get user info from Clerk
  const user = await currentUser();

  // 3. Fetch user-specific data with RLS context
  const stats = await withUserContext(userId, async (prisma) => {
    const paymentCount = await prisma.payment.count({
      where: { user_id: userId },
    });

    const totalAmount = await prisma.payment.aggregate({
      where: {
        user_id: userId,
        status: 'succeeded',
      },
      _sum: {
        amount: true,
      },
    });

    return {
      paymentCount,
      totalAmount: totalAmount._sum.amount || 0,
    };
  });

  // 4. Render authenticated UI
  return (
    <div className="container mx-auto p-6">
      <h1 className="text-3xl font-bold mb-6">
        Welcome, {user?.firstName || 'User'}!
      </h1>

      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        <StatsCard
          title="Total Payments"
          value={stats.paymentCount}
          description="All time"
        />
        <StatsCard
          title="Total Amount"
          value={`$${(stats.totalAmount / 100).toFixed(2)}`}
          description="Successful payments"
        />
      </div>
    </div>
  );
}

function StatsCard({
  title,
  value,
  description,
}: {
  title: string;
  value: string | number;
  description: string;
}) {
  return (
    <div className="bg-white p-6 rounded-lg shadow">
      <h3 className="text-sm font-medium text-gray-500">{title}</h3>
      <p className="text-3xl font-bold mt-2">{value}</p>
      <p className="text-sm text-gray-400 mt-1">{description}</p>
    </div>
  );
}
```

### Step 2: Client Component with Clerk Hooks

```typescript
// app/dashboard/profile/page.tsx
import { auth } from '@clerk/nextjs/server';
import { redirect } from 'next/navigation';
import { ProfileForm } from './profile-form';

export const dynamic = 'force-dynamic';

export default async function ProfilePage() {
  const { userId } = await auth();
  if (!userId) {
    redirect('/sign-in');
  }

  return (
    <div className="container mx-auto p-6">
      <h1 className="text-3xl font-bold mb-6">Profile Settings</h1>
      <ProfileForm />
    </div>
  );
}
```

```typescript
// app/dashboard/profile/profile-form.tsx
'use client';

import { useUser } from '@clerk/nextjs';
import { useState } from 'react';

export function ProfileForm() {
  const { user, isLoaded } = useUser();
  const [firstName, setFirstName] = useState(user?.firstName || '');
  const [lastName, setLastName] = useState(user?.lastName || '');
  const [loading, setLoading] = useState(false);

  if (!isLoaded) {
    return <div>Loading...</div>;
  }

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);

    try {
      await user?.update({
        firstName,
        lastName,
      });

      alert('Profile updated successfully!');
    } catch (error) {
      console.error('Update error:', error);
      alert('Failed to update profile');
    } finally {
      setLoading(false);
    }
  };

  return (
    <form onSubmit={handleSubmit} className="max-w-md space-y-4">
      <div>
        <label htmlFor="firstName" className="block text-sm font-medium mb-1">
          First Name
        </label>
        <input
          id="firstName"
          type="text"
          value={firstName}
          onChange={(e) => setFirstName(e.target.value)}
          className="w-full px-3 py-2 border rounded-md"
          required
        />
      </div>

      <div>
        <label htmlFor="lastName" className="block text-sm font-medium mb-1">
          Last Name
        </label>
        <input
          id="lastName"
          type="text"
          value={lastName}
          onChange={(e) => setLastName(e.target.value)}
          className="w-full px-3 py-2 border rounded-md"
          required
        />
      </div>

      <button
        type="submit"
        disabled={loading}
        className="w-full bg-blue-600 text-white py-2 rounded-md hover:bg-blue-700 disabled:opacity-50"
      >
        {loading ? 'Updating...' : 'Update Profile'}
      </button>
    </form>
  );
}
```

### Step 3: Layout with Auth Context

```typescript
// app/dashboard/layout.tsx
import { auth } from '@clerk/nextjs/server';
import { redirect } from 'next/navigation';
import { DashboardNav } from './dashboard-nav';

export const dynamic = 'force-dynamic';

export default async function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  const { userId } = await auth();
  if (!userId) {
    redirect('/sign-in');
  }

  return (
    <div className="min-h-screen flex">
      <DashboardNav />
      <main className="flex-1">{children}</main>
    </div>
  );
}
```

```typescript
// app/dashboard/dashboard-nav.tsx
'use client';

import { UserButton } from '@clerk/nextjs';
import Link from 'next/link';
import { usePathname } from 'next/navigation';

export function DashboardNav() {
  const pathname = usePathname();

  const links = [
    { href: '/dashboard', label: 'Overview' },
    { href: '/dashboard/payments', label: 'Payments' },
    { href: '/dashboard/profile', label: 'Profile' },
  ];

  return (
    <nav className="w-64 bg-gray-900 text-white p-6">
      <div className="mb-8 flex items-center justify-between">
        <h2 className="text-xl font-bold">Dashboard</h2>
        <UserButton afterSignOutUrl="/" />
      </div>

      <ul className="space-y-2">
        {links.map((link) => (
          <li key={link.href}>
            <Link
              href={link.href}
              className={`block px-4 py-2 rounded ${
                pathname === link.href
                  ? 'bg-blue-600'
                  : 'hover:bg-gray-800'
              }`}
            >
              {link.label}
            </Link>
          </li>
        ))}
      </ul>
    </nav>
  );
}
```

### Step 4: Admin Page with Role Check

```typescript
// app/admin/dashboard/page.tsx
import { auth } from '@clerk/nextjs/server';
import { redirect } from 'next/navigation';
import { withAdminContext } from '@/lib/db/rls-helpers';

export const dynamic = 'force-dynamic'; // REQUIRED for admin pages

export default async function AdminDashboard() {
  const { userId } = await auth();
  if (!userId) {
    redirect('/sign-in');
  }

  try {
    // withAdminContext verifies admin role, throws if not admin
    const stats = await withAdminContext(userId, async (prisma) => {
      const totalUsers = await prisma.user.count();
      const totalPayments = await prisma.payment.count();

      return {
        totalUsers,
        totalPayments,
      };
    });

    return (
      <div className="container mx-auto p-6">
        <h1 className="text-3xl font-bold mb-6">Admin Dashboard</h1>

        <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div className="bg-white p-6 rounded-lg shadow">
            <h3 className="text-sm font-medium text-gray-500">Total Users</h3>
            <p className="text-3xl font-bold mt-2">{stats.totalUsers}</p>
          </div>
          <div className="bg-white p-6 rounded-lg shadow">
            <h3 className="text-sm font-medium text-gray-500">Total Payments</h3>
            <p className="text-3xl font-bold mt-2">{stats.totalPayments}</p>
          </div>
        </div>
      </div>
    );
  } catch (error) {
    // Not admin - redirect to dashboard
    redirect('/dashboard');
  }
}
```

## Accessibility Checklist

- [ ] ✅ Semantic HTML (nav, main, section, etc.)
- [ ] ✅ ARIA labels for buttons and links
- [ ] ✅ Keyboard navigation works
- [ ] ✅ Focus states visible
- [ ] ✅ Color contrast meets WCAG 2.1 AA
- [ ] ✅ Loading states announced to screen readers

## Validation

```bash
# Type check
yarn type-check

# Build (verifies auth works)
yarn build

# Test authentication flow
yarn test:e2e
```

## Common Mistakes

### ❌ Mistake 1: Missing dynamic export
```typescript
// Admin page without force-dynamic
export default async function AdminPage() {
  // RLS context set at BUILD time, not REQUEST time - BROKEN!
}
```

### ✅ Fix: Add dynamic export
```typescript
export const dynamic = 'force-dynamic'; // REQUIRED
export default async function AdminPage() {
  // RLS context set at REQUEST time - WORKS!
}
```

### ❌ Mistake 2: Client component fetching with auth
```typescript
'use client';
export default function DashboardPage() {
  const { userId } = useAuth(); // Client-side auth
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('/api/data'); // No RLS context!
  }, []);
}
```

### ✅ Fix: Server component with RLS
```typescript
// Server component (default in App Router)
export default async function DashboardPage() {
  const { userId } = await auth();
  const data = await withUserContext(userId, async (prisma) => {
    return await prisma.data.findMany({ where: { user_id: userId } });
  });
  // RLS enforced!
}
```

## Related Patterns

- [User Context API](../api/user-context-api.md) - Fetch user data
- [Form with Validation](./form-with-validation.md) - Forms on authenticated pages
- [Data Table](./data-table.md) - Display user data
