---
trigger: model_decision
---

<!-- Adaptado para Antigravity -->
---
applyTo: "src/**/*.css, src/**/*.scss, src/styles/**, src/components/**"
---

# Styling

## Rules

- **No inline styles** — never use the `style` prop
- **No CSS-in-JS** (no styled-components, no emotion)
- **No external UI libraries** (no MUI, no Ant Design) unless already present
- Use plain CSS files in `src/styles/` for global styles, or CSS Modules per component

```typescript
// ❌ WRONG — inline style
<div style={{ color: 'red', padding: '16px' }}>Error</div>

// ✅ CORRECT — CSS class
<div className={styles.errorMessage}>Error</div>
```

## Global Styles Location

```
src/styles/
├── index.css       # Global resets, base styles, CSS custom properties
├── _buttons.css    # Button base styles
├── _input.css      # Input base styles
├── _resets.css     # CSS reset
└── _measure.css    # Spacing/sizing utilities
```

Import global styles in `src/main.tsx` — do not re-import in components.

## CSS Custom Properties (Variables)

Define design tokens in `src/styles/index.css` as CSS custom properties:

```css
/* ✅ CORRECT — design tokens */
:root {
  --color-primary: #005fcc;
  --color-error: #d32f2f;
  --spacing-md: 16px;
  --font-size-body: 1rem;
}

/* Component usage */
.errorMessage {
  color: var(--color-error);
  padding: var(--spacing-md);
}
```

## Component-Level Styling

For component-specific styles, use CSS Modules:

```
src/components/AccountCard/
├── AccountCard.tsx
└── AccountCard.module.css
```

```typescript
import styles from './AccountCard.module.css';

const AccountCard = ({ title }: IAccountCard) => (
  <div className={styles.container}>
    <h2 className={styles.title}>{title}</h2>
  </div>
);
```

## Responsive Design

Use mobile-first CSS with breakpoints as custom properties:

```css
/* Mobile first */
.container { padding: var(--spacing-sm); }

/* Tablet and up */
@media (min-width: 768px) {
  .container { padding: var(--spacing-md); }
}
```

## Accessibility in Styles

- Ensure color contrast ratio of at least 4.5:1 for normal text (WCAG AA)
- Never use color alone to convey meaning — pair with icons or text
- Don't remove `outline` on focus without providing an alternative focus indicator

```css
/* ❌ WRONG — removes focus visibility */
* { outline: none; }

/* ✅ CORRECT — custom focus style */
:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```
