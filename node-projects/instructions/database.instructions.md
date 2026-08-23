---
applyTo: "drizzle/**, src/**/repositories/**, drizzle.config.ts, src/**/schemas/**"
---

# Database — Schema Registry & Migrations

## Shared Database Registry — MANDATORY

> **CRITICAL**: `r365-wallet` and `r365-core` share the same PostgreSQL database.
> A shared schema registry lives at `/Docu Database/DATABASE_SCHEMA_REGISTRY.md`.

| Rule | Details |
|------|---------|
| **When to update** | Every PR that adds/modifies/removes tables, columns, or indexes |
| **What to update** | Table definition in your service section + Change Log entry |
| **Ownership** | Only modify YOUR service's tables |
| **Cross-service refs** | Add implicit FKs to "Cross-Service References" section |
| **Validation** | Check registry for name collisions before creating tables |

## Migration Workflow

1. Create migration SQL file in `drizzle/` (or Drizzle schema change)
2. Update `DATABASE_SCHEMA_REGISTRY.md` with exact column definitions
3. Add Change Log entry: date, service name, description, migration filename
4. **Both changes MUST be in the same PR**

## Drizzle ORM Conventions

- Schema definitions in service's schema files
- Use Drizzle Kit for migration generation
- Type-safe queries with Drizzle query builder
- Reference: `drizzle.config.ts` for connection config

## Repository Integration

Repositories abstract Drizzle queries behind clean function interfaces:

```typescript
export async function findUserById(id: string) {
  const result = await db.select().from(usersTable).where(eq(usersTable.id, id));
  return result[0] ?? null;
}
```

→ See `repositories.instructions.md` for full patterns.
