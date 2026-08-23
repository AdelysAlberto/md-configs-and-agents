---
trigger: model_decision
---

# Components

## Base Components (MANDATORY — use before any third-party lib)

**CRITICAL**: Always check if a Base component exists before creating a new one or using AntD.

### Available Base Components

| Component | Location | Status |
|---|---|---|
| `BaseButton` | `src/components/Button` | ✅ Ready |
| `BaseSelect` | `src/components/BaseSelect` | ✅ Ready (portal-based, no AntD) |
| `BaseInput` | `src/components/BaseInput` | 🔨 To create |
| `BaseModal` | `src/components/BaseModal` | ✅ Ready |
| `BaseCheckbox` | `src/components/BaseCheckbox` | 🔨 To create |
| `BaseRadio` | `src/components/BaseRadio` | 🔨 To create |
| `BaseSwitch` | `src/components/BaseSwitch` | 🔨 To create |
| `BaseTabs` | `src/components/BaseTabs` | 🔨 To create |
| `BaseCard` | `src/components/BaseCard` | 🔨 To create |
| `BaseAlert` | `src/components/BaseAlert` | 🔨 To create |
| `BaseBadge` | `src/components/BaseBadge` | 🔨 To create |
| `BaseSpinner` | `src/components/BaseSpinner` | 🔨 To create |
| `BaseSkeleton` | `src/components/BaseSkeleton` | 🔨 To create |
| `BasePagination` | `src/components/BasePagination` | 🔨 To create |

Toast notifications use `useToastStore` (Zustand) — no AntD `notification`/`message`.

### Imports via barrel
```ts
import { BaseButton, BaseSelect } from "@/components";
// NOT: import BaseButton from "@/components/Button/BaseButton"
```

## ANTD → Base Component Migration Map

| AntD | Replacement | Notes |
|---|---|---|
| `<Button>` | `<BaseButton>` | variants: `primary`, `secondary`, `tertiary`, `danger` |
| `<Select>` | `<BaseSelect>` | portal-based, no overflow issues |
| `<Input>` | `<BaseInput>` | TO CREATE |
| `<Modal>` | `<BaseModal>` | Import from `@/components/BaseModal` |
| `notification.*` | `useToastStore` | TO CREATE |
| `message.*` | `useToastStore` | TO CREATE |
| `<Form>` | Custom with Base components | |
| `<Checkbox>` | `<BaseCheckbox>` | TO CREATE |

**Migration steps:**
1. Identify AntD component
2. Check Base component exists → use it; else create it
3. Replace AntD with Base component
4. Remove AntD import
5. Apply CSS Module styling

## Creating New Base Components

```tsx
// src/components/BaseInput/BaseInput.tsx
import type { FC, InputHTMLAttributes } from "react";
import styles from "./BaseInput.module.css";

interface BaseInputProps extends Omit<InputHTMLAttributes<HTMLInputElement>, "size"> {
  label?: string;
  error?: string;
  size?: "sm" | "md" | "lg";
  fullWidth?: boolean;
}

const BaseInput: FC<BaseInputProps> = ({
  label, error, size = "md", fullWidth = true, ...inputProps
}) => { /* implementation */ };

export default BaseInput;
```

```css
/* BaseInput.module.css — mobile-first */
.input {
  width: 100%;
  min-height: 48px; /* touch-friendly */
  font-size: 16px;  /* prevents iOS zoom */
}
@media (min-width: 768px) {
  .input { min-height: 44px; }
}
```

Add to barrel: `src/components/index.js`
```ts
export { default as BaseInput } from "./BaseInput";
export type { BaseInputProps } from "./BaseInput";
```

## BaseButton Reference

```tsx
<BaseButton
  variant="primary"   // "primary" | "secondary" | "tertiary" | "danger"
  size="medium"       // "small" | "medium" | "large"
  isLoading={false}
  disabled={false}
  fullWidth={false}
  onClick={handleClick}
>
  Submit
</BaseButton>
```

> ⚠️ `BaseButton` uses `children` — NOT `title` prop (old docs were wrong).
