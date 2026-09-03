<!-- Adapted for OpenCode -->
---

applyTo: "src/**"
---

# Configuration & Setup

## Project Setup Guidelines

### Vite Configuration

- Use `pnpm` as the package manager (never npm or yarn).
- Pin exact versions in `package.json` (no `^` or `~` wildcards).
- Biome 2.5.x must be configured and used for all linting/formatting.

### Biome Configuration

Create a `biome.json` at the project root with:

```json
{
  "$schema": "https://biomejs.dev/schemas/2024/bioome.schema.json",
  "organize.imports": {
    "enabled": true,
    "order": "alphabetical",
    "groups": ["builtin", "external", "relative", "parent", "sibling", "index"]
  },
  "formatter": {
    "enabled": true,
    "indentStyle": "space",
    "indentSize": 2
  },
  "linter": {
    "enabled": true,
    "rules": {
      "recommended": true,
      "aqua": { "enabled": false },
      "nursery": { "enabled": false },
      "correctness": { "enabled": true },
      "style": { "enabled": true }
    }
  }
}
```

### TypeScript Configuration

- `tsconfig.json` must have `strict: true`, `noUncheckedIndexSignature: true`, and `isolatedModules: true`.
- Never use `// @ts-ignore` comments. Fix type errors properly.
- Environment variables must be accessed through `src/utils/envs.ts` only.

### npm Scripts (package.json)

Required scripts:

- `pnpm fix` — runs Biome fix for formatting
- `pnpm tsc --noEmit` — type checks
- `pnpm build` — builds the project
- `pnpm test` — runs unit/integration tests

### Environment Variables

- Never use `import.meta.env` directly.
- Always access through `import envs from 'src/utils/envs'`:

  ```typescript
  const url = envs.API.API_URL;
  ```

- `.env` files must be listed in `.env.example` (never commit real secrets).
