
# Styling (CSS Modules & Design Tokens)

## CSS Module Guidelines

All component styling must use **CSS Modules** (`*.module.css`). Never use global CSS or inline styles in components.

### Naming Convention

- Component file: `Button.tsx`
- Styles file: `Button.module.css`
- Class names within CSS Module follow BEM: `button__text--primary`

### Design Tokens (CSS Variables)

All repeated values must use CSS custom properties defined at the root level:

```css
:root {
  --color-primary: #0a0a2e;
  --color-primary-hover: #1a1a4e;
  --color-surface: #f0f0f0;
  --spacing-1: 4px;
  --spacing-2: 8px;
  --spacing-3: 16px;
  --spacing-4: 24px;
  --font-size-xs: 12px;
  --font-size-sm: 14px;
  --font-size-md: 16px;
  --font-size-lg: 20px;
  --font-weight-normal: 400;
  --font-weight-bold: 700;
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
}
```

### Token Usage

```css
/* ✅ CORRECT — using tokens */
.button {
  fill: var(--color-primary);
  margin: var(--spacing-2);
  padding: var(--spacing-3);
  border-radius: var(--radius-md);
  font-size: var(--font-size-md);
  font-weight: var(--font-weight-bold);
}

/* ❌ WRONG — magic values */
.button {
  margin: 16px;
  padding: 12px;
  border-radius: 8px;
  font-size: 16px;
}
```

### Responsive Breakpoints

Use logical breakpoints based on content:

```css
@media (min-width: 640px) { /* sm */ }
@media (min-width: 768px) { /* md */ }
@media (min-width: 1024px) { /* lg */ }
@media (min-width: 1280px) { /* xl */ }
```

### Animations

- Use `transform` and `opacity` only for animations.
- Never use `top`, `left`, `width`, `height` for animations (triggers layout thrashing).
- Use `prefers-reduced-motion` for reduced motion accessibility.

```css
/* ✅ CORRECT */
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

/* ❌ WRONG */
@keyframes slideLeft {
  from { left: 0; }
  to { left: 100px; }
}
```