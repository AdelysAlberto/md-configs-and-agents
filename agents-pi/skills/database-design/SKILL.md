---
name: database-design
description: Database architecture, relational schemas (PostgreSQL), Drizzle ORM, physical migrations, indexing strategies (B-Tree, GIN), Redis caching, and ACID transactions. Use when modeling schemas or optimizing queries.
license: MIT
compatibility: opencode
metadata:
  domain: database
  technologies: postgresql,drizzle,redis
---

# Database Design, ORM & Query Performance

Master guide for relational schema design, query optimization, indexing, migration management, and distributed caching.

## 1. Core Database Invariants

1. **Strict Normalization & Referential Integrity**: Design schemas in 3NF with explicit foreign keys, `ON DELETE` constraints, and composite unique keys.
2. **Deterministic Migrations**: Every schema change must have a physical SQL migration file and journal entry. Never alter production schemas imperatively without migration files.
3. **Targeted Indexing**: Analyze execution plans (`EXPLAIN ANALYZE`). Define B-Tree indexes for equality/range lookups, GIN for JSONB/arrays, and partial indexes for hot subsets.
4. **ACID Transactions**: Wrap multi-table state mutations in atomic transactions. Explicitly prevent race conditions and dirty reads.
5. **No N+1 Queries & No `SELECT *`**: Always project only required fields and use explicit joins or pre-warmed relation loaders.
6. **Strict Prohibition of Production Deletion & Destructive Operations**: It is strictly forbidden to execute `DELETE`, `DROP TABLE`, `DROP DATABASE`, `TRUNCATE` statements, or any destructive modifications on the production database without prior and explicit user authorization.

---

## 2. Schema Modeling with Drizzle ORM (PostgreSQL)

```typescript
import { pgTable, uuid, varchar, timestamp, index } from 'drizzle-orm/pg-core';

export const usersTable = pgTable(
  'users',
  {
    id: uuid('id').defaultRandom().primaryKey(),
    email: varchar('email', { length: 255 }).notNull().unique(),
    fullName: varchar('full_name', { length: 128 }).notNull(),
    createdAt: timestamp('created_at', { withTimezone: true }).defaultNow().notNull(),
    updatedAt: timestamp('updated_at', { withTimezone: true }).defaultNow().notNull(),
  },
  (table) => [
    index('idx_users_email').on(table.email),
    index('idx_users_created_at').on(table.createdAt),
  ]
);
```

---

## 3. Migration & Journal Lifecycle

When adding or modifying tables:
1. Generate migration: `bun run drizzle-kit generate`
2. Inspect the resulting `.sql` file in `drizzle/` and verify the entry in `drizzle/meta/_journal.json`.
3. Apply migrations programmatically on application bootstrap:

```typescript
import { migrate } from 'drizzle-orm/node-postgres/migrator';
import { db } from './client';

export const runMigrations = async () => {
  await migrate(db, { migrationsFolder: './drizzle' });
};
```

---

## 4. Indexing & Query Optimization

| Pattern | Recommended Index | Example |
| :--- | :--- | :--- |
| **Exact match / Range** | Standard B-Tree | `CREATE INDEX idx_orders_user ON orders(user_id);` |
| **Filtered Subsets** | Partial Index | `CREATE INDEX idx_active_subs ON subs(user_id) WHERE status = 'active';` |
| **Text search / Tags** | GIN Index | `CREATE INDEX idx_posts_tags ON posts USING GIN (tags);` |
| **JSONB Fields** | GIN on JSONB path | `CREATE INDEX idx_audit_meta ON audit USING GIN (metadata jsonb_path_ops);` |

### Query Inspection Rule
Always test expensive queries using:
```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT u.id, o.total
FROM users u
JOIN orders o ON o.user_id = u.id
WHERE o.created_at >= NOW() - INTERVAL '30 days';
```

---

## 5. Caching & Redis Integration

- **Cache-Aside Pattern**: Look in cache -> if miss, query DB -> populate cache with TTL.
- **Explicit Expiration**: Every cached entry MUST have a sensible TTL (`EX 300` / `EX 3600`).
- **Cache Invalidation**: Invalidate or update the cache immediately upon successful DB write transactions.
