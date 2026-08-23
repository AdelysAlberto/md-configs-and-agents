---
applyTo: "Dockerfile, docker-compose*, package.json, tsconfig.json, biome.json, drizzle.config.ts, .github/**"
---

# Config & Setup — Tooling & Environment

## Docker Development (MANDATORY)

All development runs through Docker Compose.

| Item | Details |
|------|---------|
| **Stack Location** | `../dockers/` directory |
| **Services** | r365-core (3010), MongoDB (27017), MariaDB (3306), Keycloak (8080) |
| **Start** | `cd ../dockers && docker-compose up --build` |
| **Logs** | `docker-compose logs -f r365-core` |
| **Rebuild** | `docker-compose build --no-cache r365-core` |
| **Hot Reload** | Automatic in containers |

## TypeScript Configuration

- `strict: true`, `noImplicitAny: true` in `tsconfig.json`
- Path aliases: `@/` maps to `src/`
- Target: ESNext with Node module resolution

## Tooling

| Tool | Purpose | Command |
|------|---------|---------|
| **Biome** | Formatting & linting | `pnpm biome check` |
| **Vitest** + **Supertest** | Unit & integration testing | `pnpm test` |
| **Commitlint** | Conventional Commits enforcement | Automatic via hooks |
| **Husky** | Pre-commit hooks | Automatic |
| **Drizzle Kit** | Database migrations | `pnpm drizzle-kit generate` |

## Package Manager

- **pnpm** — do not use npm or yarn
- Lockfile: `pnpm-lock.yaml` must be committed

## Environment Variables

- All secrets via centralized secret manager (never committed)
- `.env` for local development only (gitignored)
- Docker injects environment from compose files
