# Antigravity Instructions

This is the **index** for project coding standards. Detailed rules live in `.agents/rules/`.

## Modular Instructions Index

| File | Scope | Topics |
|---|---|---|
| [coding-standards](rules/coding-standards.md) | `src/**` | TypeScript, ES2022+, functional programming, Biome, commits |
| [architecture](rules/architecture.md) | `src/**` | Vertical slicing, service layer, DRY, HTTP client |
| [components](rules/components.md) | `src/components/**` | Base components library, ANTD → Base map, creating components |
| [styling](rules/styling.md) | `*.module.css, src/styles/**` | CSS Modules, design tokens, mobile-first breakpoints |
| [services-hooks](rules/services-hooks.md) | `src/services/**, src/hooks/**, src/adapters/**` | Result pattern, React Query, adapters |
| [state-management](rules/state-management.md) | `src/store/**, src/hooks/**` | Zustand selectors, React Query, CookieStore, react-jwt |
| [i18n](rules/i18n.md) | `src/**` | react-i18next, translation keys, no hardcoded strings |
| [ux-design](rules/ux-design.md) | `src/pages/**, src/components/**` | Mobile-first, touch targets, ARIA, WCAG AA |
| [migration](rules/migration.md) | `src/**/*.{ts,tsx,js,jsx}` | JSX→TSX, ANTD→Base, delete obsolete files |
| [verification-checklist](rules/verification-checklist.md) | `**` | Post-task checklist, build/lint commands |

## External Reference Documents

| Document | Purpose |
|---|---|
| [result-pattern-services.md](rules/result-pattern-services.md) | Full Result Pattern guide with examples |
| [TECHNICAL_REFACTORING_RULES.md](rules/technical-refactoring-rules.md) | Refactoring patterns and performance optimization |
| [DEPENDENCIES_UPDATE_STRATEGY.md](rules/dependencies-update-strategy.md) | Dependency update strategy (Phase 8+) |
| [refactoring-instructions.md](rules/refactoring-loops.md) | JS/TS loop structure and refactoring patterns |

## Stack Quick Reference

| Technology | Version | Notes |
|---|---|---|
| React | 19+ | |
| TypeScript | 7+ | `strict: false`, `checkJs: false` |
| Vite + SWC | latest | `vite.config.js`, handles TS/TSX natively |
| @tanstack/react-query | latest | Server state, `select` for transforms |
| Zustand | latest | Global state — selector pattern MANDATORY |
| react-i18next | latest | All user-facing text |
| Biome | latest | Lint + format (`pnpm fix`) |
| CSS Modules | — | Exclusive styling solution |
| pnpm | — | Package manager |

## Critical Non-Negotiables (quick reference)

1. **Functional only** — no `class`, no `this`, no OOP
2. **CSS Modules only** — no Tailwind, no inline styles
3. **No AntD** — replace with Base components from `src/components`
4. **TypeScript for all new/touched files** — `.jsx` → `.tsx`
5. **Result Pattern** — services never `throw`, return `TResponseRequest<T>`
6. **Zustand selectors** — `useStore(s => s.field)` not destructure
7. **No hardcoded strings** — always use i18n `t()` keys
8. **Mobile-first** — 320px base, then 768px+, then 1280px+
9. **No `console.log`** — remove before committing
10. **Post-task checklist** — run `pnpm fix && pnpm tsc --noEmit && pnpm build`

## ANTD → Base Component Map (current status)

| AntD | Base Component | Status |
|---|---|---|
| `<Button>` | `<BaseButton>` · `src/components/Button` | ✅ Ready |
| `<Select>` | `<BaseSelect>` · `src/components/BaseSelect` | ✅ Ready |
| `<Input>` | `<BaseInput>` · `src/components/BaseInput` | 🔨 To create |
| `<Modal>` | `<BaseModal>` · `src/components/BaseModal` | ✅ Ready |
| `notification` | `useToastStore` · `src/store/useToast.store` | 🔨 To create |
| `message` | `useToastStore` · `src/store/useToast.store` | 🔨 To create |
| `<Form>` | Custom form with Base components | — |

### **Before Completing Task**
1. ✅ Run all verification commands listed above
2. ✅ Fix all errors and warnings found
3. ✅ Review changed files one more time
4. ✅ Ensure ALL checklist items are completed
5. ✅ Verify i18n translations are properly created
6. ✅ Confirm no sensitive data is logged
7. ✅ Commit changes with descriptive message in English following Conventional Commits

# Response Style (prose only — does NOT apply to code generation or rule compliance)

- Skip filler phrases ("I understand", "Let me know if...", "Here is the solution")
- After completing file operations, confirm in 1 line max
- Use bullet points for multiple notes
- No style rule overrides architecture rules, result pattern, or instruction files
- Code quality and rule compliance are always full priority

---

> **CRITICAL**: Do not mark a task as complete until ALL verification items pass. This is non-negotiable and mandatory for every single task.

> **Mantra**: Prioritize high-performance and clean code. Use modern standards (ES2022+), but favor simplicity and stability over novelty. Strictly avoid deprecated or legacy patterns. For every technical decision, choose the path that minimizes cognitive complexity and optimizes resource usage, ensuring code remains maintainable, robust, and efficient.