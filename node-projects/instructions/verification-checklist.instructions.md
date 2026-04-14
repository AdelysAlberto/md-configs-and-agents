---
applyTo: "**"
---

# Verification Checklist — Post-Task

Run through this checklist after every code change.

## Architecture

- [ ] Dependencies point inward (toward domain)
- [ ] Business logic ONLY in services
- [ ] Data access ONLY in repositories
- [ ] Controllers are thin (no business logic, no try/catch)

## Result Pattern

- [ ] Services return `Result<T>` — `ResponseOk(data)` / `ResponseFail.X("code")`
- [ ] NO `throw` in services (except try/catch for external providers)
- [ ] Controllers use `sendResult()` or manual destructuring for paginated
- [ ] Service-to-service failures propagated with `return result`

## API Response

- [ ] `data` contains content directly (array or object, never wrapped)
- [ ] `pagination` is sibling of `data` (never nested inside)
- [ ] No `status` field in HTTP body
- [ ] Error responses: `{ "error": "ErrorCode" }`

## Type Safety

- [ ] No `any` types
- [ ] Explicit return types on all functions
- [ ] `unknown` + type guards for dynamic data

## Functional Programming

- [ ] No classes
- [ ] No mutable state
- [ ] Pure functions (side effects only in repositories/providers)

## Domain Boundaries

- [ ] No cross-domain repository imports
- [ ] Inter-domain communication uses services only

## Database (if applicable)

- [ ] Migration file created
- [ ] `DATABASE_SCHEMA_REGISTRY.md` updated
- [ ] Change Log entry added
- [ ] No name collisions with other service's tables

## Quality

- [ ] Logger used (not console.log)
- [ ] Documentation in English
- [ ] JSDoc on exported functions
