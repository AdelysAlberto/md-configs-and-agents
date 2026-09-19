---
description: Frontend Architecture and React / React Native Clean Standards
applyTo: "**/*.tsx, **/*.ts, **/*.jsx, **/*.js"
---

# Frontend Architecture & React Standards

> Universal standards for React, React Native, and Web frontend applications.

## 1. Core Stack & Framework Invariants

- **React 19+ / Functional TypeScript**: Strictly pure functional components (`export const Component = () => ...`). Prohibit `class`, `this`, and `React.FC`.
- **Global State Management**: Use **Zustand 5+** exclusively. Access store state using atomic selectors with `useShallow` (no full store destructuring).
- **Data Fetching & Caching**: Use **TanStack Query (React Query)**:
  - Encapsulate queries and mutations inside dedicated custom hooks (e.g. `useUsersQuery()`, `useCreateAccountMutation()`).
  - Isolate loading states with dedicated UI components (e.g. `<UserListLoading />` / Skeleton loaders) to prevent layout shifts.
- **Storage Strategy**: Use `sessionStorage` or `cookieStorage` according to session isolation vs cross-tab persistence requirements. Prohibit raw unvalidated localStorage for sensitive data.

## 2. File Length & Modular Decoupling

- **Hard Limit**: Maximum **250 lines of code (LOC)** per file/component/view.
- **Screen Orchestrators**: Target `< 100 LOC` for top-level screen containers.
- Extract state, business logic, and API calls into custom hooks.
- Extract sub-views and form sections into dedicated component modules.

## 3. Component Reusability & Base Design System

- **Base Components First**: Buttons, Inputs, Modals, Cards, and Layouts must be built as reusable base primitives and reused across the entire application.
- If a UI pattern is written more than once, extract it into a reusable base component immediately.

## 4. Styling & CSS Standards

- **Zero Inline Styles**: Prohibit inline CSS (`style={{ ... }}`). Use CSS Modules (`*.module.css`) or design system utility tokens.
- Maintain concentric border radii: `outerRadius = innerRadius + padding`.

## 5. Pure Utilities vs Closures

- Functions calculating dates, converting currency, transforming data, or operating purely on arguments must be **pure functions** placed in `utils/`.
- If a function does not depend on component closure or React state, it MUST live outside the component or in a dedicated utility module.
