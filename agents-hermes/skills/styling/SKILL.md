---
name: styling
description: CSS Modules, BEM, design tokens, responsive layout and transition standards. Use when working on files matching: src/**/*.css, src/**/*.scss, src/styles/**, src/components/**.
metadata:
  hermes:
    tags: [team-pinky, coding-standards]
    category: engineering
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

## Interactive UI Components & Transitions (Dropdowns, Navbars, Modals)

When building interactive UI components (mobile menus, dropdowns, dialogs, hamburger menus):

1. **BEM Naming Standard**: Follow `.block__element--modifier` strictly (e.g. `.v-navbar__hamburger--active`, `.v-navbar__mobile-menu--open`).
2. **GPU-Accelerated Smooth Transitions**:
   - Always animate `opacity` and `transform` (`translateY`, `scale`).
   - **Never animate `height: auto` or toggle abruptly with `display: none` without transition states**. Use `opacity: 0; pointer-events: none; transform: translateY(-10px); transition: all 0.25s ease-in-out;` and toggle to `opacity: 1; pointer-events: auto; transform: translateY(0);`.
3. **Backdrop and Glassmorphism**: Use design tokens (`var(--backdrop-blur)`) with `-webkit-backdrop-filter` fallback and semi-transparent RGBA/HSLA backgrounds.
4. **Accessible States**: Sync visual modifier classes with ARIA attributes (`aria-expanded="true"`, `aria-hidden="false"`, `aria-label`).
5. **Mobile-First Breakpoints**: Ensure responsive elements have explicit media query definitions for desktop vs mobile layouts (e.g. hiding desktop nav and displaying hamburger under `@media (max-width: 860px)`).

```css
/* ✅ CORRECT — Smooth Dropdown / Mobile Menu pattern */
.menu {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  opacity: 0;
  transform: translateY(-8px);
  pointer-events: none;
  transition: opacity 0.25s ease, transform 0.25s ease;
  backdrop-filter: var(--backdrop-blur);
  -webkit-backdrop-filter: var(--backdrop-blur);
}

.menu--open {
  opacity: 1;
  transform: translateY(0);
  pointer-events: auto;
}
```
