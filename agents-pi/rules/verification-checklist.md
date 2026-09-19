---
description: Deterministic verification gate before task completion
alwaysApply: false
---

# Pre-Completion Verification Checklist

Execute deterministic verification before marking any non-trivial coding task as done:

---

## 1. Code Quality & Architectural Invariants

- [ ] **Pure Functional Paradigm**: Zero `class`, zero `this`, zero `React.FC`.
- [ ] **Type Safety**: No `any` types used (`unknown` + type guards or Zod/Valibot schemas).
- [ ] **Screaming Architecture & Vertical Slicing**: Code located under domain modules `src/modules/<FeatureName>/`.
- [ ] **Result Pattern**: Services return `{ success: true, data } | { success: false, error }` without throwing raw exceptions.
- [ ] **Provider Adapter Pattern (DIP)**: Domain logic depends on agnostic interfaces in `src/providers/`, not vendor SDKs.
- [ ] **File Length Hard Limit**: No file exceeds **250 LOC**; screens under **100 LOC**.
- [ ] **No Inline Styles**: Zero `style={{ ... }}` in React/Web components (use CSS Modules with design tokens).
- [ ] **Data Fetching Hook + Loading**: Service calls are wrapped in custom hooks with TanStack Query and include a dedicated Loading component.
- [ ] **Pure Utilities**: Calculation and transformation logic without closures extracted to `utils/`.
- [ ] **Backend Service-Repository Segregation**: Domain logic in Service, raw queries in Repository.
- [ ] **Bruno API Collections**: Every new/modified backend route has a corresponding `.bru` file in `bruno/`.
- [ ] **Zero Deprecated APIs**: No deprecated methods, props, or dependencies are introduced.

---

## 2. Deterministic Verification Commands

Run in the project directory and ensure 100% pass:

```bash
bun run biome:check && bun run check && bun test
# OR (when using pnpm)
pnpm biome:check && pnpm typecheck --noEmit && pnpm test
```
