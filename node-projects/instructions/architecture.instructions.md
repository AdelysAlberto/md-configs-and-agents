---
applyTo: "src/**"
---

# Architecture — Clean Architecture & Screaming Design

## Clean Architecture Layers

| Layer | Responsibility | Rules |
|-------|---------------|-------|
| **Controllers** | Thin HTTP adapters | NO business logic, NO try/catch |
| **Services** | Pure business logic | Result Pattern (`ResponseOk`/`ResponseFail`), NO throw |
| **Repositories** | Data access abstraction | NO business logic, provider abstraction |
| **Providers** | External service integrations | Isolated in `shared/provider/` |
| **Schemas** | Input validation (Zod) | DTOs and request validation |
| **Mappers** | Data transformation | External ↔ internal format |

## Project Structure

```
src/
├── modules/
│   └── {domain}/              # Domain-specific logic
│       ├── routes/            # HTTP route definitions
│       ├── controllers/       # Thin HTTP adapters
│       ├── services/          # Use-cases and business logic
│       ├── schemas/           # DTOs and Zod validations
│       ├── repositories/      # Data access layer
│       ├── mappers/           # Transform external ↔ internal data
│       └── utils/             # Domain-specific utilities
├── shared/
│   ├── middlewares/           # Common middlewares (auth, error, validation)
│   ├── provider/              # External service clients
│   ├── config/                # Environment and configuration
│   └── types/                 # Global types, aliases, enums
└── main.ts                    # Application entry point
```

## File Naming Convention

```
src/modules/{domain}/
├── controllers/{domain}.controller.ts
├── services/{domain}.service.ts
├── repositories/{domain}.repository.ts
├── schemas/{domain}.schema.ts
└── routes/{domain}.routes.ts
```

## Design Principles

- **Screaming Architecture**: folder names reflect domain intent, not technical layers
- Each top-level folder = a **domain concept**
- Cross-cutting logic lives in `shared/`
- Use `tsconfig.json` path aliases (`@/` prefix)
- Dependencies point **inward** (toward domain)
- Business logic **independent** of frameworks

## Data Flow

```
Request → Route → Middleware → Controller → Service → Repository → Provider
                                    ↓
Response ← Controller ← Service ← Repository ← Provider
```

## Development Workflow — New Features

1. **Define Types** → types/schemas first
2. **Create Repository** → data access functions
3. **Implement Service** → business logic with Result Pattern
4. **Create Controller** → thin HTTP adapter with `sendResult`
5. **Define Routes** → `router.post("/endpoint", validate(schema), controller)`

## SOLID Principles (Summary)

- **SRP**: Each function has ONE responsibility — don't mix validate + activate
- **OCP**: Extensible through composition (e.g., `ValidationRule[]`), not modification
- **LSP**: Repository abstractions are substitutable (DB ↔ cache)
- **ISP**: Segregated types (`UserReader` vs `UserWriter`)
- **DIP**: Services depend on repository abstractions, not implementations
