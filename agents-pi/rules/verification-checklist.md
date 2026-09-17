---
description: Deterministic verification gate before task completion
alwaysApply: false
---

# Pre-Completion Verification Checklist

Execute deterministic verification before marking any non-trivial coding task as done:

## 1. Code Quality & Invariants
- [ ] Strictly functional TypeScript (no `class`, no `this`, no `React.FC`).
- [ ] No `any` types used (`unknown` + type guards).
- [ ] Vertical slicing preserved in `src/modules/<FeatureName>/`.
- [ ] Result Pattern implemented in services (`{ success: true, data } | { success: false, error }`).
- [ ] Provider Adapter Pattern (DIP): domain services import domain-agnostic provider interfaces only.
- [ ] React Native screens strictly adhere to `<ScreenLayout>` and < 250 LOC limits.

## 2. Deterministic Verification Commands

```bash
bun run biome:check && bun run check && bun test
# OR (when using pnpm)
pnpm biome:check && pnpm typecheck --noEmit && pnpm test
```
