<!-- Adapted for Opencode -->
---
applyTo: "src/components/**"
---
# Components & React

## Component Guidelines

- **Functional Components Only**: All components must be pure function declarations. No class components, no `React.FC`.
- **PascalCase Naming**: Component file names must use PascalCase (e.g., `AccountCard.tsx`).
- **Barrel Exports**: Each component directory should export its default export and named exports via `index.ts`.
- **Prop Interfaces**: Define prop interfaces using TypeScript `type` or `interface` at the top of each file.
- **Default Props**: Use destructuring defaults rather than `defaultProps`.

## Component Structure

```typescript
import { useState, useEffect } from "react";

interface ButtonProps {
  variant?: "primary" | "secondary";
  size?: "sm" | "md" | "lg";
  onClick: () => void;
  children: React.ReactNode;
}

const Button = ({ variant = "primary", size = "md", onClick, children }: ButtonProps) => {
  return (
    <button
      className={`
        btn btn--${variant} btn--${size}
      `}
      onClick={onClick}
    >
      {children}
    </button>
  );
};

export { Button };
export type { ButtonProps };
```

## Component Composition

- Prefer composition over inheritance.
- Use render props or renderless components for complex logic.
- Keep components focused on a single responsibility.

## Styling

- Use CSS Modules (`*.module.css`) for component-scoped styles.
- Never use global CSS or inline styles in components.
- Follow BEM convention within CSS Modules for component internals.