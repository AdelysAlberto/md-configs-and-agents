---
description: Backend architecture invariants for Bun, Fastify/Express, Service-Repository pattern, and Bruno API testing
globs:
  - "src/**/*.ts"
  - "src/**/*.js"
scope:
  - "tool:edit(*.ts)"
  - "tool:write(*.ts)"
condition:
  - "console\\.log\\("
---

# Backend Architecture Invariants (Node & Bun)

Mandatory engineering standards for all backend services, APIs, and microservices.

---

## 1. Core Technology Stack

- **Runtime**: Bun (or Node.js with strict TypeScript when project dictates).
- **Framework**: Fastify (preferred for high-throughput performance & schema validation) or Express.
- **Logging**: Structured Pino logger. `console.log` is strictly prohibited. Sanitize sensitive data (passwords, tokens, credentials).
- **API Testing & Documentation**: Bruno (`.bru` executable collections).
- **Linter & Formatter**: Biome (`biome.json`).

---

## 2. Layered Architecture: Controller, Service & Repository

Strictly enforce the separation of concerns across architectural layers:

```text
HTTP Request
     │
     ▼
[ Controller ]   -> Validates request schema, extracts params/body, invokes Service, sets HTTP status code
     │
     ▼
[ Service ]      -> Encapsulates pure business logic, domain rules, and orchestration (returns Result)
     │
     ▼
[ Repository ]   -> Encapsulates raw database queries, ORM calls (Drizzle, Prisma), and external storage
```

### Architectural Layer Invariants:
1. **Controllers**:
   - Only handle HTTP transport, authentication context, and route parameters.
   - Do NOT contain business logic or database queries.
2. **Services**:
   - Contain domain business logic.
   - Return strongly typed Result shapes: `{ success: true, data: T } | { success: false, error: AppError }`.
   - **Never throw raw exceptions** across service boundaries.
   - Do NOT execute direct SQL queries or ORM models directly; delegate to Repositories.
3. **Repositories**:
   - The single place where database interactions, SQL queries, or ORM calls reside.
   - Decouple domain services from the concrete database engine or ORM syntax.

---

## 3. Mandatory Bruno API Collections (`.bru`)

- For every new or modified route, an executable Bruno file (`.bru`) must be created or updated under the `bruno/` directory:
  - `bruno/Public/<Module>/<endpoint>.bru` for unauthenticated routes.
  - `bruno/Private/<Module>/<endpoint>.bru` for authenticated routes.
- Every `.bru` file must include valid payload examples, HTTP method, URL, and expected assertions.

---

## 4. Unified Response Schema

All HTTP responses must return a deterministic JSON envelope:
- **Success (`200` / `201`)**:
  ```json
  {
    "message": "Operation completed successfully",
    "data": { ... }
  }
  ```
- **Error (`4xx` / `5xx`)**:
  ```json
  {
    "error": "VALIDATION_ERROR",
    "message": "Descriptive, actionable error explanation"
  }
  ```
