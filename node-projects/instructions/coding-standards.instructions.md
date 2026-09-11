---
applyTo: "src/**/*.ts"
---

# Coding Standards — Node.js & Backend Services

## Universal Anti-Pattern Elimination (Zero-Tolerance)

- **Zero Double Fan-Out**: Publishers must never self-echo or receive their own emitted events in Pub/Sub channels. Use sender IDs (`senderId`), `noLocal: true`, or separate ingest vs egress channels.
- **Scoped Subscriptions**: Subscriptions to message buses/WebSockets must be strictly scoped to specific entity IDs (`room:${id}`). Never use broad wildcards (`*`, `#`) for tenant/client streams.
- **Bounded In-Memory State**: Never store state in unbounded in-process `Map`/`Set` collections. Use LRU caches with maximum sizes and TTLs, or delegate to Redis/database.
- **Zero Secrets in URLs / Query Params**: Auth tokens, API keys, and credentials must never travel via query strings. Use `Authorization: Bearer <token>` or `X-API-Key` headers.
- **Mandatory Lifecycle & TTL**: Every ephemeral key in Redis, cache entry, or distributed lock MUST have an explicit TTL. Never create immortal locks or un-expiring sessions.
- **Guarded Business Rules**: Validate and enforce all business constraints (quotas, `maxMembers`, paywall permissions, balance checks) inside domain service logic with transactional atomicity. Never rely on schema definitions or UI checks alone.
- **Idempotent Mutations**: All state-changing operations (creations, joins, payments, webhook processing) must be idempotent and guarded via idempotency keys or unique constraints.
- **Zero Silent Error Swallowing**: Empty catch blocks (`catch (e) {}`) are strictly forbidden. Infrastructure and network errors must be logged with operational context and returned via `Result.fail()`.
- **Zero N+1 Queries**: Never query databases or remote APIs inside loops (`map`, `forEach`, `for`). Use batching (`WHERE id IN (...)`), joins, or DataLoader.
- **Omni-Channel Boundary Validation**: Validate all inbound payloads across WebSockets, Pub/Sub events, Webhooks, and gRPC with Zod/schema validation before executing business logic.

---

## Paradigm: Functional Programming ONLY

- **NO classes** — pure functions only
- **NO constructors** or OOP patterns
- Pure functions with explicit return types
- Immutable data structures (spread `{ ...obj }`, never mutate)
- Function composition over inheritance

```typescript
// ✅ Immutable update
const updateUser = (user: User, updates: Partial<User>): User => ({
  ...user,
  ...updates,
});

// ❌ Mutation
const badUpdate = (user: User, updates: Partial<User>): User => {
  user.email = updates.email; // FORBIDDEN
  return user;
};
```

---

## Type Safety Rules

| Rule | Details |
|------|---------|
| **NO `any`** | Use `unknown` + type guards for dynamic data |
| **Explicit returns** | All functions must declare return types |
| **Prefer `type`** | Use `type` for data shapes; `interface` only for contracts/abstractions |
| **Union types** | Prefer `"active" \| "inactive"` over enums |
| **No `as`** | Avoid type assertions unless safety is proven |
| **Generics** | Use for reusable, type-safe APIs |
| **`Record<string, unknown>`** | For dynamic objects instead of `any` |

---

## Error Handling: Result Pattern (`ResponseFail` / `Result.fail`)

**Rule**: Services MUST use `ResponseFail.X()` or `Result.fail(error)` and return `Result<T>`. Never `throw` unchecked exceptions out of services.

---

## Logging Convention

```typescript
logger.info("[serviceName]: Starting operation", { operationId });
logger.warn("[serviceName]: Validation failed", { reason });
logger.error("[serviceName]: Infrastructure failure", { error, stack: error.stack });
```

---

## Forbidden Patterns

```typescript
// ❌ Double fan-out / Self-echo loops
// ❌ Wildcard subscriptions (* / #) on client/tenant streams
// ❌ Unbounded global in-memory state / maps
// ❌ Secrets in URL query parameters
// ❌ Immortal sessions / locks without TTL
// ❌ Business rules in schema/UI but missing in service logic
// ❌ Non-idempotent write operations / missing deduplication
// ❌ Empty catch blocks / swallowed exceptions
// ❌ N+1 queries in loops
// ❌ Unvalidated WebSocket / Queue payloads
// ❌ God services (multiple unrelated methods)
// ❌ Business logic in controllers
// ❌ any type
// ❌ console.log (use logger)
// ❌ Mutable state / Classes
```
