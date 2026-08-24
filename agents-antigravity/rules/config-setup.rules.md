---
trigger: model_decision
description: 'Configuration, setup, dependencies, Biome and linter standards'
applyTo: '*.json, *.ts, tsconfig*.json, vite.config.ts, eslint.config.js, package.json, biome.json'
---

# Config & Setup

## Tech Stack Versions & Scaffolding Rule

- **Pinned Exact Versions**: In `package.json`, wildcards (`^`, `~`) are strictly prohibited. Always pin exact versions (e.g. `"react": "19.0.0"`).
- **Always Install Latest Stable Versions (`@latest`)**: When scaffolding or creating a new project, NEVER hardcode outdated or static version numbers into `package.json`. Always use `@latest` or standard CLI generators (e.g. `pnpm create astro@latest`, `npm create vite@latest`).
- **Dynamic Dependency Resolution**: Run `pnpm add <package>@latest` or `pnpm add -D <package>@latest` to resolve current stable packages at runtime.

| Tool | Version Strategy |
|---|---|
| React / React DOM | `@latest` (v19+) |
| TypeScript | `@latest` (v5.x+) |
| Vite / Astro | `@latest` |
| State & Query (Zustand, React Query) | `@latest` |
| Validation & Testing (Zod, Vitest) | `@latest` |
| Node.js | `>=20.0.0` (LTS) |
| Package Manager | `pnpm` (`@latest`) |

## Linter & Formatter (Biome)

This project uses **Biome** (`@biomejs/biome@latest`) as the primary linter and formatter.

When initializing a project, ALWAYS generate `biome.json` using this default template:

```json
{
  "$schema": "https://biomejs.dev/schemas/2.5.9/schema.json",
  "files": {
    "includes": ["src/**/*", "!**/*.svg", "!dist/**/*", "!node_modules/**/*"]
  },
  "css": {
    "parser": {
      "cssModules": true
    }
  },
  "formatter": {
    "attributePosition": "multiline",
    "enabled": true,
    "indentStyle": "space",
    "indentWidth": 2,
    "lineWidth": 120
  },
  "javascript": {
    "formatter": {
      "arrowParentheses": "asNeeded",
      "bracketSpacing": false,
      "jsxQuoteStyle": "double",
      "quoteStyle": "double",
      "semicolons": "always",
      "trailingCommas": "es5"
    },
    "globals": [
      "describe", "it", "test", "expect", "beforeEach", "afterEach",
      "beforeAll", "afterAll", "jest", "vi", "vitest", "process", "meta"
    ]
  },
  "json": {
    "formatter": {
      "trailingCommas": "none"
    }
  },
  "linter": {
    "enabled": true,
    "rules": {
      "a11y": {
        "noLabelWithoutControl": "off",
        "noStaticElementInteractions": "off",
        "useFocusableInteractive": "off",
        "useKeyWithClickEvents": "off",
        "useSemanticElements": "off",
        "useValidAnchor": "off",
        "noSvgWithoutTitle": "off"
      },
      "complexity": {
        "noForEach": "off",
        "noImportantStyles": "off"
      },
      "correctness": {
        "noUndeclaredVariables": "error",
        "noUnusedImports": "error",
        "noUnusedVariables": "warn",
        "useExhaustiveDependencies": "off",
        "useImportExtensions": "off",
        "useUniqueElementIds": "off"
      },
      "security": {
        "noDangerouslySetInnerHtml": "off"
      },
      "style": {
        "noDescendingSpecificity": "off",
        "noInferrableTypes": "off",
        "useImportType": "error"
      }
    }
  },
  "vcs": {
    "clientKind": "git",
    "enabled": true,
    "useIgnoreFile": true
  }
}
```

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
