---
applyTo: "**/repositories/**"
---

# Repositories — Data Access Layer

## Core Rules

- Repositories handle **data access only** — NO business logic
- Abstract provider implementation details
- Pure functions with clear input/output types
- Return raw data; let services interpret it

## Repository Patterns

### Find by Criteria

```typescript
export async function findEntityByCriteria(criteria: string) {
  const results = await provider.findBy({ criteria });
  return results.length > 0 ? results[0] : null;
}
```

### Create Entity

```typescript
export async function createEntity(data: CreateEntityInput) {
  return provider.create({
    field1: data.field1,
    field2: data.field2,
    attributes: {
      customField1: [data.customField1],
      customField2: ["defaultValue"],
    },
  });
}
```

### Update Entity

```typescript
export async function updateEntity(id: string, data: Record<string, unknown>) {
  return await provider.update(id, data);
}
```

### List with Pagination

```typescript
export async function getEntities(userId: string, limit: number, offset: number) {
  return await db
    .select()
    .from(entitiesTable)
    .where(eq(entitiesTable.userId, userId))
    .limit(limit)
    .offset(offset);
}

export async function countEntities(userId: string) {
  const result = await db
    .select({ count: count() })
    .from(entitiesTable)
    .where(eq(entitiesTable.userId, userId));
  return result[0]?.count ?? 0;
}
```

## ❌ Forbidden in Repositories

```typescript
// ❌ Business logic
if (user.isActive && user.balance > 0) { /* ... */ }

// ❌ Error handling decisions
throw AppError.NotFound("UserNotFound");

// ❌ Importing repositories from other domains
import { UserRepository } from "@/modules/user/repositories/user.repository";
```
