---
applyTo: "package.json, tsconfig.json, biome.json, build.ts, bunfig.toml, src/index.tsx, src/index.html, src/frontend.tsx, src/constants/config.ts"
---

# Config & Setup — r365-backoffice

## Stack & Versiones actuales

| Tecnología | Versión | Uso |
|---|---|---|
| **Bun** | latest | Runtime, bundler, servidor, package manager |
| **React** | 19.2+ | UI framework |
| **TypeScript** | strict (ESNext) | Lenguaje |
| **React Router** | 7.13+ | Routing (createBrowserRouter) |
| **Zustand** | 5.0+ | Estado global |
| **TanStack Query** | 5.90+ | Server state, caching |
| **Zod** | 4.3+ | Validación |
| **TailwindCSS** | 4.2+ | Utilidades CSS |
| **Biome** | 2.4+ | Linting + formatting |

## Runtime: Bun (NO Node.js)

- ✅ `bun install` — NO npm/yarn/pnpm
- ✅ `bun run <script>` — NO npm run
- ✅ `bun --watch` para dev
- ✅ Bun carga `.env` automáticamente — NO usar dotenv
- ✅ Variables de entorno: prefijo `BUN_PUBLIC_` para client-side

## Scripts disponibles

```json
{
  "dev": "bun --watch --port 3000 src/index.tsx",
  "start": "NODE_ENV=production bun src/index.tsx",
  "build": "bun run build.ts",
  "fix": "biome check --fix ./src",
  "check": "biome check ./src"
}
```

## Entry points

| Archivo | Rol |
|---|---|
| `src/index.tsx` | Bun.serve — SPA server con fallback `/*` |
| `src/index.html` | HTML shell |
| `src/frontend.tsx` | React root (`ReactDOM.createRoot`) |
| `src/App.tsx` | Router + Providers |

## TypeScript Config

- `strict: true`, `target: ESNext`, `module: Preserve`
- Path alias: `@/*` → `./src/*`
- `verbatimModuleSyntax: true` — requiere `import type`
- `noUncheckedIndexedAccess: true`

## Biome Config

- Indent: 2 spaces, line width: 120
- Quotes: double, semicolons: always
- `noExplicitAny: error`, `useImportType: error`
- Tailwind directives habilitadas en CSS parser

## Environment variables (`src/constants/config.ts`)

```typescript
config.apiBaseUrl       // API principal
config.walletApiUrl     // API wallet
config.onboardingApiUrl // API onboarding
config.isProd / config.isDev
config.sessionKey       // "r365_bo_session"
```
