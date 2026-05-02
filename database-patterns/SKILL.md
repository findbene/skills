---
name: database-patterns
description: "Database design patterns for SQL and NoSQL covering schema design, normalization, indexing strategies, and data modeling best practices. Use this skill any time database schemas need to be designed, data models need to be chosen, normalization decisions need to be made, or database patterns need to be applied. Trigger immediately on: \"database design\", \"schema design\", \"normalization\", \"data model\", \"ERD\", \"one-to-many\", \"many-to-many\", \"NoSQL\", \"document model\", \"relational design\", \"foreign key\", \"table design\", \"database pattern\", \"data modeling\". If someone says \"how should I structure my database?\" or \"design a schema for this\" this skill MUST trigger."
---

# Database Patterns

Best practices for database design and optimization.

## Schema Design

### Naming Conventions
```sql
-- Tables: plural, snake_case
users, order_items, user_profiles

-- Columns: snake_case
first_name, created_at, is_active

-- Primary keys: id
-- Foreign keys: {table}_id
user_id, order_id
```

### Essential Columns
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  -- ... business columns
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  deleted_at TIMESTAMPTZ  -- Soft delete
);
```

## Indexing Strategy

Index foreign keys, frequently queried columns, and columns used in WHERE clauses. Composite index column order matters — put the most selective column first.

```sql
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_orders_user_status ON orders(user_id, status);
CREATE INDEX idx_active_users ON users(email) WHERE deleted_at IS NULL;
```

## Query Optimization

### Avoid N+1
```typescript
// BAD: N+1 queries
const users = await db.users.findMany();
for (const user of users) {
  const posts = await db.posts.findMany({ where: { userId: user.id } });
}

// GOOD: Single query with join
const users = await db.users.findMany({ include: { posts: true } });
```

### Use EXPLAIN
```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@example.com';
```

### Pagination
Cursor-based pagination performs better than offset for large datasets because it doesn't need to skip rows:
```sql
-- Cursor pagination
SELECT * FROM users WHERE id > 'last_seen_id' ORDER BY id LIMIT 20;
```

## Common Patterns

### Soft Deletes
Set `deleted_at` timestamp instead of deleting rows. Filter with `WHERE deleted_at IS NULL`. Preserves data for audit/recovery.

### Audit Log
```sql
CREATE TABLE audit_logs (
  id UUID PRIMARY KEY,
  table_name VARCHAR(255),
  record_id UUID,
  action VARCHAR(50),
  old_data JSONB,
  new_data JSONB,
  user_id UUID,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### Optimistic Locking
Add a `version` column. Update with `WHERE version = N` — if 0 rows affected, a conflict occurred.

## Migrations Best Practices

1. Add columns as nullable first
2. Backfill data
3. Then add NOT NULL constraint

Avoid in production: DROP COLUMN without backup, RENAME COLUMN (breaks queries), changing column type without migration.

## Transactions
```typescript
await db.$transaction(async (tx) => {
  const user = await tx.users.create({ data: userData });
  await tx.accounts.create({ data: { userId: user.id } });
});
```
