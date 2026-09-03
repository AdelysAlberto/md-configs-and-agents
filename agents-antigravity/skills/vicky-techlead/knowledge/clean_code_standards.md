# Clean Architecture & Code Standards Framework - Vicky (Tech Lead)

This document details the code quality standards, Clean Architecture rules, and Result Pattern guidelines enforced by **Vicky**.

---

## 1. Non-Negotiable Technical Directives

1. **Pure Functional Code**: OOP, `class`, and `this` are strictly prohibited. Write pure functional TypeScript/JavaScript.
2. **Vertical Slicing & Screaming Architecture**: Organize all business domain code by module inside `src/modules/<FeatureName>/`.
3. **Result Pattern Error Handling**: Never throw exceptions from services. Return explicit result objects (`{ success: true, value }` or `{ success: false, error }`).
4. **Zustand Selector Hygiene**: Never destructure entire global Zustand stores. Use `useShallow` or atomic state selectors.
5. **Pure CSS Modules**: Use CSS Modules exclusively (`*.module.css`). Do NOT use inline styles or TailwindCSS unless explicitly instructed.
6. **Pre-Commit Verification**: Run `pnpm fix && pnpm tsc --noEmit && pnpm build` prior to finishing any task.

---

## 2. Directory Structure

```text
src/
├── modules/
│   ├── auth/
│   │   ├── components/       # Authentication-specific UI
│   │   ├── hooks/            # Reactive logic/hooks
│   │   ├── services/         # HTTP adapters / API clients
│   │   └── types/            # Types and validation schemas (Zod/DTOs)
│   ├── products/
│   │   ├── components/
│   │   ├── services/
│   │   └── types/
└── shared/                   # Cross-cutting utilities (UI primitives, logger, HTTP client)
```

---

## 2. Mandatory Design Patterns

1. **Result Pattern**: Avoid `try/catch/throw` for business flow control. Always return structured objects `{ ok: true, data }` or `{ ok: false, error }`.
2. **KISS & Clean Code**: Short functions (< 50 lines), early returns, descriptive variable and function names.
3. **Adapter / Mapper Pattern**: Decouple the UI from external API responses. Map external DTOs to domain models before consuming them in UI components.
4. **Global State Selector Pattern**: When using Zustand or Redux, enforce the use of individual selectors to prevent unnecessary re-renders.
