<!-- Adapted for Opencode -->
---
applyTo: "src/**"
---
# Verification Checklist

## Pre-Commit Verification

Always run the following before completing any technical task:

```bash
pnpm fix              # Biome formatting fix
pnpm tsc --noEmit     # Type check (zero errors)
pnpm test             # Unit + integration tests with coverage
```

## Post-Task Verification Checklist

After completing any code implementation or technical task, and before delivering results to the user:

1. **Invoke Specialist Sub-Agent**:
   - Delegate code audit to **Vicky TechLead** (`vicky-techlead` / `/standards`), **Dr. House** (`house-testing` / `/testing`), and **Inspector Gadget** (`gadget-auditor` / `/audit`).

2. **Audit Criteria**:
   - Analyze modified code diff verifying Clean Architecture, Result Pattern, unit test coverage, zero unused endpoints, and best practices.
   - Verify zero regressions by checking type compilation (`pnpm tsc --noEmit`), linter (`pnpm fix`), and unit tests (`pnpm test`).

3. **Results Delivery**:
   - Only after sub-agent approval, summarize findings in `walkthrough.md` and complete the task.

## Code Quality Non-Negotiables

- **Pure Functional Code**: Prohibit `class`, `this`, and OOP. Write pure functional TypeScript/JavaScript.
- **Vertical Slicing**: Group all business domain code by module inside `src/modules/<FeatureName>/`.
- **Result Pattern**: Never throw exceptions from services. Return explicit result objects (`{ success, value/error }`).
- **Zustand Selector Hygiene**: Never destructure entire global Zustand stores. Use `useShallow` or atomic selectors.
- **Styles**: Use CSS Modules exclusively (`*.module.css`) with BEM and design tokens. No inline styles or TailwindCSS unless explicitly instructed.
- **Internationalization**: All user-facing text must use `t('key')` keys.
- **Pinned Exact Versions**: In all `package.json` files, wildcards like `^` or `~` are strictly forbidden. Always pin exact, deterministic versions.
- **Mandatory Latest Stable Investigation**: Before installing or updating any package, actively query npm/bun registries to use the latest stable GA release available.
- **Unified Biome 2.5.x Standard**: All modules must include their official `biome.json` config and execute Biome for linting and formatting.
- **Simplification First**: Fixes should make the system simpler, not more complex. Prefer removing or consolidating code over adding a new layer, flag, or special case.