---
description: Frontend architecture invariants for React, TanStack Query, Zustand, and component decoupling
globs:
  - "**/*.tsx"
  - "**/*.jsx"
scope:
  - "tool:edit(*.tsx)"
  - "tool:write(*.tsx)"
condition:
  - "style=\\{\\{"
---

# Frontend Architecture Invariants (React & Web)

Mandatory engineering standards for all React frontend development.

---

## 1. Core Technology Stack

- **Framework**: React (Pure functional components, zero `class`, zero `React.FC`).
- **Data Fetching & Server State**: TanStack Query (`@tanstack/react-query`).
- **Global & Client State**: Zustand (v5+ with atomic selectors and `useShallow`).
- **Linter & Formatter**: Biome (`biome.json`).
- **Styling**: CSS Modules (`*.module.css`) with design tokens / CSS variables. **Zero inline styles** (`style={{ ... }}` is strictly prohibited).
- **Client Storage**: Evaluate between `sessionStorage` (isolated, tab-scoped session security) and `CookieStorage` (cross-tab / SSR-compatible) according to the project's security and session lifecycle requirements.

---

## 2. Component Decoupling & Line Limits

- **Maximum 250 LOC Hard Limit**: No React component file may exceed **250 lines of code**.
- **Views & Screens Under 100 LOC**: Page and screen components must strictly remain under **100 LOC** by decomposing layout sections and presentation into subcomponents.
- **Base Component Reuse**:
  - Centralize foundational UI elements (`Button`, `Input`, `Modal`, `Layout`, `Card`, `Badge`) in `src/shared/components/` or `src/components/ui/`.
  - Never reimplement ad-hoc styling for core controls. If a visual pattern or component structure appears more than once, extract it into a reusable base component.

---

## 3. Data Fetching, Services & Loading States

- **Mandatory Custom Hook Wrapper**:
  - Direct calls to API services from inside UI components are prohibited.
  - Every domain service call MUST be encapsulated within a custom hook that manages TanStack Query (`useQuery` / `useMutation`).
  - Example:
    ```tsx
    // src/modules/User/hooks/useUserData.ts
    export function useUserData(userId: string) {
      return useQuery({
        queryKey: ['user', userId],
        queryFn: () => userService.getUserById(userId),
      });
    }
    ```
- **Mandatory Loading Components**:
  - UI components consuming query hooks must explicitly handle loading states with dedicated Loading / Skeleton components. Never leave the user with blank or hanging screens.

---

## 4. Hook Hygiene & Hook Extraction

- **Multiple `useEffect` Rule**:
  - If a component accumulates multiple `useEffect` hooks, complex derived logic, or event listeners, you MUST extract that orchestration into a dedicated custom hook (e.g. `useUserProfile()`).
- **Never use `useEffect` for Derived State**:
  - Compute values directly in the render body.
  - Never trigger secondary state updates from an effect when synchronous computation is possible.

---

## 5. Pure Utilities vs. Closures

- Any function that transforms dates, formats currencies, manipulates strings, or performs mathematical calculations without depending on React state, component props, or closure scope **MUST be extracted to a pure function in `utils/`**.
- Keep UI components focused strictly on rendering and user interaction.
