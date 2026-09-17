---
name: css-architecture
description: CSS Modules architecture, BEM methodology, design tokens (CSS variables), mobile-first responsive design, and GPU-accelerated animations. Use when styling components or structuring CSS.
license: MIT
compatibility: opencode
metadata:
  domain: styling
  methodology: bem
---

# CSS Architecture & Styling Compliance

Comprehensive guide for clean, maintainable, performant CSS architecture using **CSS Modules**, **BEM methodology**, and **Design Tokens**.

## 1. Core Styling Invariants

1. **CSS Modules Exclusively**: All component styling must use CSS Modules (`*.module.css`). Never use global CSS for components or inline styles.
2. **Zero Hardcoded Magic Values**: Always consume CSS variables / Design Tokens for colors, spacing, typography, and borders.
3. **Strict BEM Naming**: Follow `.block__element--modifier` within CSS modules to guarantee predictable specificity.
4. **Mobile-First Layouts**: Base styles target mobile screens (`min-width: 0`). Use `min-width` media queries to progressively enhance larger screens.
5. **GPU-Accelerated Animations**: Only animate `transform` and `opacity`. Never animate layout properties (`top`, `left`, `width`, `height`, `margin`, `padding`).

---

## 2. File Organization & BEM Naming

- Component file: `ComponentName.tsx`
- Styles file: `ComponentName.module.css`

```css
/* Block */
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: none;
  border-radius: var(--radius-md);
  padding: var(--spacing-2) var(--spacing-4);
  background-color: var(--color-primary);
  color: var(--color-surface);
  transition: transform var(--transition-fast), opacity var(--transition-fast);
}

/* Element */
.button__icon {
  margin-right: var(--spacing-2);
  width: var(--font-size-md);
  height: var(--font-size-md);
}

/* Modifier */
.button--outline {
  background-color: transparent;
  border: 1px solid var(--color-primary);
  color: var(--color-primary);
}

.button--disabled {
  opacity: 0.5;
  pointer-events: none;
}
```

In JSX:
```tsx
import styles from './Button.module.css';

export const Button = ({ outline, disabled, children }) => {
  const className = [
    styles.button,
    outline && styles['button--outline'],
    disabled && styles['button--disabled'],
  ].filter(Boolean).join(' ');

  return <button className={className}>{children}</button>;
};
```

---

## 3. Design Tokens (CSS Custom Properties)

Define tokens centrally in `:root` or design token theme files:

```css
:root {
  /* Colors */
  --color-primary: #0a0a2e;
  --color-primary-hover: #1a1a4e;
  --color-surface: #ffffff;
  --color-background: #f8f9fa;
  --color-text: #1f2937;
  --color-text-muted: #6b7280;

  /* Spacing Scale (8pt grid) */
  --spacing-1: 4px;
  --spacing-2: 8px;
  --spacing-3: 16px;
  --spacing-4: 24px;
  --spacing-5: 32px;
  --spacing-6: 48px;

  /* Typography */
  --font-size-xs: 12px;
  --font-size-sm: 14px;
  --font-size-md: 16px;
  --font-size-lg: 20px;
  --font-size-xl: 24px;
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-bold: 700;

  /* Borders & Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 16px;
  --radius-full: 9999px;

  /* Transitions */
  --transition-fast: 150ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-normal: 250ms cubic-bezier(0.4, 0, 0.2, 1);
}
```

---

## 4. Responsive Breakpoints

Adopt content-aware, mobile-first breakpoints:

```css
/* Mobile base styles first */
.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--spacing-3);
}

/* Tablet (md) */
@media (min-width: 768px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Desktop (lg) */
@media (min-width: 1024px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

---

## 5. Animation Hygiene & Reduced Motion

```css
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(4px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animatedElement {
  animation: fadeIn var(--transition-normal) forwards;
}

@media (prefers-reduced-motion: reduce) {
  .animatedElement {
    animation: none;
    transition: none;
  }
}
```
