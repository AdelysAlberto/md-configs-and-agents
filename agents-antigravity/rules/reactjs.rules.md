---
description: 'React Clean Code standards, pure functional components, and Biome'
applyTo: '**/*.tsx, **/*.ts'
---

# React & Clean Code Standards

## 1. Core Invariants
- **Pure Functional Components**: Prohibit `class`, `this`, and `React.Component`. Use direct `export const Component = () => ...` or function declarations.
- **Never use `React.FC` or `React.FunctionComponent`**: Type props directly in the argument destructuring `({ title, children }: Props)`.
- **Linter & Formatter**: Biome standard (`biome.json`). Always pass `bun run biome:check` / `pnpm fix`.
- **Styles**: CSS Modules exclusively (`*.module.css`) with design tokens. No inline styles.
- **Internationalization**: All user-facing text must use `t('key')`.

---

## 2. Component Design & Prop Typing

```typescript
// ✅ GOOD: Direct typing, pure functional, CSS module integration
import type { ReactNode } from 'react';
import styles from './Button.module.css';

interface ButtonProps {
  children: ReactNode;
  variant?: 'primary' | 'secondary';
  onClick: () => void;
}

export const Button = ({ children, variant = 'primary', onClick }: ButtonProps) => {
  return (
    <button
      type="button"
      className={`${styles.button} ${styles[`button--${variant}`]}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
};
```

---

## 3. Hooks Hygiene & Anti-Patterns

### ❌ BAD (Unnecessary Effects & Redundant State)
```typescript
// ❌ BAD: Synchronizing state that could be derived directly during render
const [fullName, setFullName] = useState('');
useEffect(() => {
  setFullName(`${firstName} ${lastName}`);
}, [firstName, lastName]);
```

### ✅ GOOD (Derived State)
```typescript
// ✅ GOOD: Compute directly during render
const fullName = `${firstName} ${lastName}`;
```

---

## 4. State & Data Flow
- **Local State**: `useState` / `useReducer` for internal component-only UI state.
- **Global Client State**: Zustand (with atomic selectors or `useShallow`).
- **Async / Server State**: TanStack Query or custom hooks returning Result Pattern objects.
- **Forms**: Controlled components validated with Zod schemas.
