<!-- Adaptado para Antigravity -->
---
applyTo: "**"
---

# Verification Checklist

Run through this after completing any task.

## Code Quality

- [ ] No `any` types — use specific types or `unknown`
- [ ] No `class`, `constructor`, `this`, `extends` (functional only)
- [ ] No `React.FC` — use direct function declarations
- [ ] No inline `//` comments — JSDoc only, in English
- [ ] No `var` — use `const` / `let`
- [ ] No inline `style` props in JSX
- [ ] No hardcoded UI text — all text through i18n keys

## Components

- [ ] Used `@sice/frontend-components` where applicable
- [ ] Project-specific base components from `src/baseComponents/` for wrappers
- [ ] Imported from barrel file: `import { X } from 'src/baseComponents'`
- [ ] All new components have a typed `interface I<Name>` for props
- [ ] New base components exported from `src/baseComponents/index.ts`

## Services & Data

- [ ] All data fetching uses `useQuery` / `useMutation`
- [ ] Every new service has a corresponding custom hook
- [ ] URL construction uses `src/services/api/` + `envs.ts`
- [ ] No `import.meta.env` used directly — only `src/utils/envs.ts`

## State

- [ ] Server state → TanStack Query (not Zustand)
- [ ] UI/client state → Zustand store in `src/states/`
- [ ] Auth state read from `useAuthStore`, not from localStorage

## i18n

- [ ] New text keys added to **both** `en/` and `es/` translation files
- [ ] Keys follow `{feature}.{section}.{element}` pattern
- [ ] Dynamic values use interpolation, not concatenation

## Functional & Testing

- ✅ Are there any console errors or warnings when running the code?
- ✅ **VERIFICACIÓN POST-CÓDIGO**: Después de escribir código, SIEMPRE revisa (usando `pnpm lint`, `pnpm typecheck` o examinando el archivo) que no haya errores de sintaxis, variables sin definir, o importaciones erróneas.
- ✅ Did you write tests for new components or logic?

## Linting (Biome) & Commits

- [ ] `pnpm lint` run — Biome check & auto-fix
- [ ] No Biome errors or warnings remaining
- [ ] Commit message follows `#<BRANCH_ID> <type>(<scope>): <desc>` format

## Accessibility

- [ ] Interactive elements have accessible labels (ARIA or visible text)
- [ ] Images have alt text
- [ ] Color is not the only means of conveying information
- [ ] Focus indicators are visible (not `outline: none` without alternative)

## Quick Commands

```bash
pnpm typecheck # TypeScript analyzer
pnpm lint     # Biome check & auto-fix
pnpm test     # Run Vitest suite
git branch | grep '\*'   # Get branch ID for commit
```
