---
trigger: model_decision
description: 'Runtime decision'
applyTo: '**/*.tsx, **/*.ts, **/*.js, **/*.jsx'
---

# JavaScript & TypeScript Rules

These rules apply when working with JavaScript or TypeScript.

## ⚡ 4. Non-Negotiable Directives (Quick Reference)

1. **Pure Functional Code**: Prohibit `class`, `this`, and OOP. Write pure functional TypeScript/JavaScript.
2. **Vertical Slicing**: Group all business domain code by module inside `src/modules/<FeatureName>/`.
3. **Result Pattern**: Never throw exceptions from services. Return explicit result objects (`{ success, value/error }`).
4. **Zustand Selector Hygiene**: Never destructure entire global Zustand stores. Use `useShallow` or atomic selectors.
5. **Styles**: Use CSS Modules exclusively (`*.module.css`) with BEM and design tokens. No inline styles or TailwindCSS unless explicitly instructed.
6. **Internationalization**: All user-facing text must use `t('key')` keys.
7. **Pre-Commit Verification**: Always run `pnpm fix && pnpm tsc --noEmit && pnpm build` (or `bun run biome:check && bun run check && bun run build`) before completing any technical task.
8. **Simplification First**: Fixes should make the system simpler, not more complex. Prefer removing or consolidating code over adding a new layer, flag, or special case. If a fix grows the system's surface area, look for the version that shrinks it.
9. **Pinned Exact Versions (Zero Caret `^` / `~` Strictly Prohibited)**: In all `package.json` files, wildcards like `^` or `~` are strictly forbidden. Always pin exact, deterministic versions (e.g., `"maplibre-gl": "6.5.0"`).
10. **Mandatory Latest Stable Investigation**: Before installing or updating any package, actively query npm/bun registries to use the latest stable GA release available.
11. **Unified Biome 2.5.x Standard**: All modules must include their official `biome.json` config and execute Biome for linting and formatting.

## Domain Rule Routing

Load only the rules relevant to the current task.

| Trigger | Rule |
| --- | --- |
| React Components / Hooks / TSX | `rules/reactjs.rules.md` |
| CSS Modules / Styling | `rules/styling.rules.md` |
| UI Components / Design System | `rules/ui-library.rules.md` |
| Zustand / State | `rules/state-management.rules.md` |
| Services / API / Fetching | `rules/services-hooks.rules.md` |
| Architecture / Directory Structure | `rules/architecture.rules.md` |
| i18n / Localization | `rules/i18n.rules.md` |
| Config / package.json / Dependencies | `rules/config-setup.rules.md` |
| Unit / Integration Tests | `rules/testing.rules.md` |
| Git / Commits | `rules/commits.rules.md` |
| Backend / API Workflow | `rules/backend_workflow_standards.md` |
| Completion / Verification | `rules/verification-checklist.rules.md` |
