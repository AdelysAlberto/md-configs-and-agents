# Copilot Instructions — Clean Architecture & Functional Programming

## Stack

- **Runtime**: Node.js + Express (TypeScript, strict mode)
- **ORM**: Drizzle ORM (PostgreSQL)
- **Validation**: Zod
- **Auth**: Keycloak
- **Linting**: Biome
- **Testing**: Vitest + Supertest
- **Package Manager**: pnpm
- **Containers**: Docker Compose (mandatory for development)

---

## Critical Rules

1. **Functional Programming Only** — NO classes, NO constructors, pure functions, immutable data → `coding-standards.instructions.md`
2. **Result Pattern** — Services return `Result<T>` via `ResponseOk()`/`ResponseFail.X()`. NEVER `throw` → `services.instructions.md`
3. **Thin Controllers** — NO business logic, NO try/catch. Use `sendResult()` → `controllers.instructions.md`
4. **NO `any` type** — Use `unknown` + type guards. Explicit return types always → `coding-standards.instructions.md`
5. **API Response Format** — `data` is direct content (array/object), `pagination` is sibling, NO `status` in body → `api-response.instructions.md`
6. **Domain Boundaries (DDD)** — Never import repositories across domains. Use service-to-service → `ddd.instructions.md`
7. **Screaming Architecture** — Folder names = domain concepts. `src/modules/{domain}/` structure → `architecture.instructions.md`
8. **Database Registry** — Update `DATABASE_SCHEMA_REGISTRY.md` with every migration PR → `database.instructions.md`
9. **AppError is Legacy** — Only exists as errorHandler safety net. Services use `ResponseFail.X()` exclusively → `services.instructions.md`
10. **Container-First** — All development via Docker Compose → `config-setup.instructions.md`
11. **Business Context** — Domain models, API surface, security requirements → `project-context.instructions.md`
12. **Post-Task Verification** — Run checklist after every change → `verification-checklist.instructions.md`

---

## Modular Instructions

| File | Scope | applyTo |
|------|-------|---------|
| [`architecture.instructions.md`](./instructions/architecture.instructions.md) | Project structure, layers, SOLID, data flow | `src/**` |
| [`coding-standards.instructions.md`](./instructions/coding-standards.instructions.md) | TypeScript, FP, type safety, forbidden patterns | `src/**/*.ts` |
| [`controllers.instructions.md`](./instructions/controllers.instructions.md) | Controller patterns, sendResult, validation | `**/controllers/**, **/routes/**` |
| [`services.instructions.md`](./instructions/services.instructions.md) | Result Pattern, error mapping, composition | `**/services/**` |
| [`repositories.instructions.md`](./instructions/repositories.instructions.md) | Data access, provider abstraction | `**/repositories/**` |
| [`api-response.instructions.md`](./instructions/api-response.instructions.md) | Response format, pagination, error format | `**/controllers/**, **/interfaces/**` |
| [`ddd.instructions.md`](./instructions/ddd.instructions.md) | Domain boundaries, inter-domain communication | `src/modules/**` |
| [`database.instructions.md`](./instructions/database.instructions.md) | Schema registry, migrations, Drizzle | `drizzle/**, **/repositories/**` |
| [`config-setup.instructions.md`](./instructions/config-setup.instructions.md) | Docker, tooling, environment | `Dockerfile, package.json, tsconfig.json` |
| [`project-context.instructions.md`](./instructions/project-context.instructions.md) | Business domain, models, APIs, security | `src/**` |
| [`verification-checklist.instructions.md`](./instructions/verification-checklist.instructions.md) | Post-task checklist | `**` |

## Reference Documents

| Document | Purpose |
|----------|---------|
| [`.github/project-context.md`](./project-context.md) | Full business requirements |
| [`.github/result-pattern-services.md`](./result-pattern-services.md) | Result Pattern deep-dive |
| [`docs/http-provider.md`](../docs/http-provider.md) | HTTP client patterns |

## Quick Lookup — Imports

```typescript
// Result Pattern
import { ResponseOk, ResponseFail } from "@/utils/result";
import { sendResult } from "@/utils/result";
import type { Result } from "@/utils/result";

// API Response helpers
import { createPaginationMeta, createSuccessResponse } from "@/interfaces/api-response";

// HTTP status codes
import { HttpStatus } from "@/interfaces";

// Logger
import { logger } from "@/provider/logger";
```

## Verification Commands

```bash
pnpm biome check           # Lint & format
pnpm test                   # Run tests
pnpm drizzle-kit generate   # Generate migrations
```

---

> **Mantra**: *Keep controllers thin, services smart, repositories isolated, types explicit, and folder names screaming the business intent.*
