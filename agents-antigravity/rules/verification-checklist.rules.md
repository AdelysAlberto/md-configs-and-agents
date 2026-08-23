---
description: 'Pre-completion deterministic verification checklist'
applyTo: '**'
---

# Pre-Completion Verification Checklist

Run through this checklist and execute deterministic verification before marking any task as done.

## 1. Code Quality & Invariants
- [ ] Strictly functional TypeScript (no `class`, no `this`, no `React.FC`).
- [ ] No `any` types used.
- [ ] Vertical slicing preserved: domain code in `src/modules/<FeatureName>/`.
- [ ] Result Pattern implemented in services (`{ success: true, data } | { success: false, error }`). Never throw exceptions.
- [ ] Zustand store accessed via atomic selectors or `useShallow` (no full store destructuring).
- [ ] CSS Modules used exclusively (`*.module.css`) with design tokens.
- [ ] All user-facing text wrapped in `t('key')` i18n keys.
- [ ] Exact dependency versions pinned in `package.json` (no `^` or `~`).

---

## 2. Deterministic Verification Gate (Terminal Commands)
Execute in the terminal and ensure 100% pass:

```bash
bun run biome:check && bun run check && bun test
# OR (pnpm)
pnpm fix && pnpm tsc --noEmit && pnpm test
```
