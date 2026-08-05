<!-- Adaptado para Antigravity -->
---
applyTo: "*.json, *.ts, tsconfig*.json, vite.config.ts, eslint.config.js, package.json"
---

# Config & Setup

## Tech Stack Versions

| Tool | Version |
|---|---|
| React | 18.2.0 |
| TypeScript | 5.7.2 |
| Vite | 5.0.10 |
| React Router | 6.26.1 |
| Zustand | 4.5.4 |
| TanStack Query | 5.79.0 |
| Zod | 3.24.1 |
| ESLint | 9.18.0 |
| Vitest | 3.2.0 |
| Axios | 1.9.0 |
| i18next | 23.7.11 |
| Node.js | 20+ |
| Package manager | pnpm |

## Linter & Formatter (Biome)

This project uses **Biome** as the primary linter and formatter.

Config file: `biome.json`

### Key Scripts

```bash
pnpm lint      # biome check --write (safe auto-fixes + formatting)
pnpm typecheck # tsc --noEmit -p tsconfig.src.json
```

### Auto-fix These Violations Always

1. **Unused imports** — remove them
2. **Unused variables** — remove or prefix with `_`
3. **Node.js import protocol** — add `node:` prefix when required by rules

```typescript
// ❌ WRONG
import { unused, UsedType } from './types';

// ✅ CORRECT
import { UsedType } from './types';
```

### Run Before Every Commit

```bash
pnpm typecheck && pnpm lint
```

## TypeScript Config

`tsconfig.json` has `strict: true` — always maintain this. Key settings:
- `target: ES2022`
- `moduleResolution: bundler`

## Vite Config

- Config in `vite.config.ts`
- Build triggered with `cross-env VITE_PROJECT=NZTA vite build`

## Environment Variables

- Runtime config is loaded from `public/env-config.{env}.js` files
- Access via `src/utils/envs.ts` — never use `import.meta.env` directly
- Environment files: `dev`, `qa`, `test1`, `ci`

## Husky Pre-commit

Pre-commit hooks enforce Biome checks and Typechecks. Do not bypass with `--no-verify` unless absolutely necessary and approved.

## Vitest Config

- Config in `vitest.config.ts`
- Test setup in `src/setupTests.ts`
- Tests live in `src/**/__tests__/` or alongside components as `*.test.tsx`
