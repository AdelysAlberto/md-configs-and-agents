---
applyTo: "src/**/*.ts"
---

# Coding Standards — TypeScript & Functional Programming

## Paradigm: Functional Programming ONLY

- **NO classes** — pure functions only (AppError exists only as legacy safety net)
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

```typescript
// ✅ Type guard for unknown data
const isUser = (data: unknown): data is User =>
  typeof data === "object" && data !== null && "id" in data && "email" in data;

// ✅ Union type
type UserStatus = "active" | "inactive" | "pending";

// ❌ FORBIDDEN
const bad = (data: any): any => data;
```

## Error Handling: AppError vs ResponseFail

| **AppError** (Legacy) | **ResponseFail** (Current) |
|----------------------|---------------------------|
| `throw AppError.NotFound("msg")` | `return ResponseFail.NotFound("msg")` |
| ❌ Only for errorHandler safety net | ✅ Use in ALL services |
| `@/utils/appError` | `@/utils/result` |

**Rule**: Services MUST use `ResponseFail.X()` and return `Result<T>`. Never `throw`.

## Logging Convention

```typescript
logger.info("[serviceName]: Starting operation");
logger.warn("[serviceName]: Validation failed");
logger.error("[serviceName]: Critical failure");
logger.info("[serviceName]: ✅ Operation completed successfully");
```

## Documentation

- JSDoc comments for exported functions (brief, one-line)
- All documentation in **English**
- Minimal inline comments — code should be self-explanatory
- Destructure function arguments to clarify intent

## Forbidden Patterns

```typescript
// ❌ God services (multiple unrelated methods)
// ❌ Business logic in controllers
// ❌ try/catch in controllers
// ❌ throw in services (use ResponseFail)
// ❌ any type
// ❌ console.log (use logger)
// ❌ Mutable state
// ❌ Classes
```
