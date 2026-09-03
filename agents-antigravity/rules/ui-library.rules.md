---
trigger: file_context
description: 'Strict standard for the Design System & Reusable React Frontend Component Library (DRY, CSS Variables, Folder-based Structure)'
applyTo: src/**/*.tsx, src/components/**/*
---

# Design System & Reusable Component Library Rules (React)

## 1. DRY Principle & Single Source of Truth
- **Zero Duplication of Interactive Elements**: It is strictly prohibited to inline buttons, input fields, labels, modals, toasts, or dropdowns directly in pages or module views.
- **Centralized Library**: Every reusable visual element MUST belong to `src/components/` and be consumed from there. If a component's appearance or behavior is modified (e.g., `Button`), the change must be reflected across 100% of the application by modifying that single source.

---

## 2. Mandatory Component Folder Structure

Each library component MUST reside within its own dedicated folder in `src/components/<ComponentName>/`:

```text
src/components/<ComponentName>/
├── <ComponentName>.tsx        # Pure functional React component code
├── <ComponentName>.types.ts   # Strict Props and TypeScript types definition
└── index.ts                   # Clean barrel export
```

### Example `index.ts` Export:
```typescript
export * from "./Button";
export * from "./Button.types";
```

---

## 3. Mandatory Global CSS Variables Invariant (`var(--...)`)

- **Hardcoding Colors in Code or Inline Classes is Prohibited**: All colors (backgrounds, borders, text, accents, and states) MUST use global CSS variables or theme tokens (`var(--color-...)` or centrally configured variants in `index.css`).
- If the brand changes a primary or secondary hue, the change MUST be made exclusively in the variable definition in `index.css` without needing to modify component code.

### Standard Design System Tokens:
- `--color-primary`: Institutional or brand primary color
- `--color-secondary`: Secondary or accent color
- `--color-danger`: Critical state, errors, or deletions
- `--color-success`: Successful or confirmation state
- `--color-bg-main`: Application main background
- `--color-bg-card`: Surface and elevated cards
- `--color-border-card`: Separation and delimiting borders

---

## 4. Mandatory Base Component Catalog

1. **`Button`**:
   - Required variants: `primary`, `secondary`, `tertiary`, `danger`, `ghost`, `outline`.
   - Support for left/right icon and loading state (`loading`).
   - Respect standard design system `border-radius`.
2. **`Input`**:
   - Text, password, search, email fields.
   - Support for `label`, `error`, `helperText`, and `icon`.
3. **`Dropdown` / `Select`**:
   - Unified dropdown selector for filters and forms.
4. **`Modal`**:
   - `InformationModal`: Notice, credits, or details modal.
   - `ConfirmationModal`: Interactive confirmation modal (accept/cancel).
5. **`Toast`**:
   - `ToastProvider` & `useToast()` hook for floating notifications with `success`, `error`, `warning`, `info` variants.
6. **`Image`**:
   - Image component with lazy loading, loading skeleton, and error fallback.
7. **`Badge`**:
   - Status labels and metric indicators.
8. **`Card`**:
   - Elevated surface container with adaptive border.

---

## 5. Simplicity & Pure Functional Code
- Classes or object-oriented patterns (`class`, `this`) are prohibited.
- Components must be simple, minimalist, and free of over-engineering.
