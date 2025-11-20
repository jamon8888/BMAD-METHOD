# RLS Migration Pattern

**Purpose**: Create Prisma database migration with Row Level Security (RLS) policies to protect user data

**When to use**: Any new table storing user-specific data, adding sensitive columns, user-owned content

## Implementation

### Step 1: Define Prisma Schema

```prisma
model YOUR_TABLE {
  id         String   @id @default(cuid())
  user_id    String   // REQUIRED for RLS
  // ...your fields
  created_at DateTime @default(now())
  updated_at DateTime @updatedAt

  @@index([user_id])  // CRITICAL for RLS performance
}
```

### Step 2: Generate Migration

```bash
npx prisma migrate dev --name add_YOUR_TABLE_with_rls
```

### Step 3: Add RLS Policies to Migration

Edit the generated migration file (in `prisma/migrations/`):

```sql
-- Enable RLS on the table
ALTER TABLE "YOUR_TABLE" ENABLE ROW LEVEL SECURITY;

-- Policy 1: User Isolation (MANDATORY)
CREATE POLICY "user_isolation_YOUR_TABLE"
ON "YOUR_TABLE"
FOR ALL
TO wtfb_app_user
USING (
  user_id = current_setting('app.current_user_id', true)
);

-- Policy 2: Admin Override (if needed)
CREATE POLICY "admin_access_YOUR_TABLE"
ON "YOUR_TABLE"
FOR ALL
TO wtfb_app_user
USING (
  EXISTS (
    SELECT 1 FROM user_roles
    WHERE user_id = current_setting('app.current_user_id', true)
    AND role = 'admin'
  )
);

-- Policy 3: System Access (for webhooks/background jobs)
CREATE POLICY "system_access_YOUR_TABLE"
ON "YOUR_TABLE"
FOR ALL
TO wtfb_app_user
USING (
  current_setting('app.system_context', true) = 'true'
);
```

### Step 4: Customization Guide

Replace placeholders:
- `YOUR_TABLE` → Your table name (e.g., `payments`, `subscriptions`)
- Add table-specific conditions if needed
- Adjust policy names for clarity

## Security Checklist

- [ ] ✅ Table has `user_id` column
- [ ] ✅ Index on `user_id` for performance
- [ ] ✅ RLS enabled on table
- [ ] ✅ User isolation policy created
- [ ] ✅ Policies use `current_setting('app.current_user_id', true)`
- [ ] ✅ Migration tested in development

## Validation

```bash
# Check RLS is enabled
psql -U wtfb_user -d wtfb_dev \
  -c "SELECT tablename, rowsecurity FROM pg_tables WHERE tablename = 'YOUR_TABLE';"

# Expected output: rowsecurity = t (true)

# Test isolation
psql -U wtfb_app_user -d wtfb_dev \
  -c "SET app.current_user_id = 'user_123'; SELECT * FROM YOUR_TABLE;"

# Should only return rows where user_id = 'user_123'
```

## Critical Notes

⚠️ **Database Users**:
- `wtfb_user` - Runs migrations, OWNS tables
- `wtfb_app_user` - Application user, RLS ENFORCED

⚠️ **Never bypass RLS**:
- Direct Prisma calls will fail (linter catches this)
- MUST use `withUserContext()`, `withAdminContext()`, or `withSystemContext()`

## Related Patterns

- [User Context API](../api/user-context-api.md) - How to use RLS from API routes
- [Prisma Transaction](./prisma-transaction.md) - Transactions with RLS
- [RLS Implementation Guide](../../data/RLS_IMPLEMENTATION_GUIDE.md) - Complete reference
