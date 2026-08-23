---
trigger: model_decision
---

# Styling

## CSS Modules (MANDATORY)

✅ ALL styles MUST use `.module.css` files  
❌ No Tailwind CSS  
❌ No inline styles (exception: truly dynamic runtime values)  
❌ No styled-components or CSS-in-JS  

```tsx
// ✅ Correct
import styles from "./MyComponent.module.css";
const MyComponent: FC = () => <div className={styles.container}>Content</div>;

// ❌ Wrong
const MyComponent = () => <div style={{ padding: 16 }}>Content</div>;
const MyComponent = () => <div className="flex p-4 text-sm">Content</div>; // no Tailwind
```

## Mobile-First (MANDATORY)

Design order: **320px → 768px → 1280px**

```css
/* Mobile base (no media query needed) */
.container {
  padding: 1rem;
  font-size: 16px; /* prevents iOS zoom on inputs */
}

/* Tablet */
@media (min-width: 768px) {
  .container { padding: 2rem; }
}

/* Desktop */
@media (min-width: 1280px) {
  .container { padding: 3rem; }
}
```

## Touch Targets

| Element | Minimum |
|---|---|
| Buttons | `min-height: 48px` |
| Inputs | `min-height: 48px` |
| Icons (interactive) | `44px × 44px` tap area |
| Toggle switches | `min-height: 44px` |

## Design Tokens (use CSS custom properties)

Use project-level tokens — do NOT hardcode colors or spacing:

```css
/* ✅ Correct */
.button { background: var(--btn-gold-mid); color: var(--text-primary); }

/* ❌ Wrong */
.button { background: #f5a623; color: #ffffff; }
```

Common tokens:
```
--bg-main            background
--surface-3          elevated surface
--input-bg           input background
--input-border       input border
--text-primary       primary text
--text-secondary     secondary text
--text-muted         muted/disabled text
--btn-gold-mid       gold CTA
--action-primary     primary action
--status-error       error state
--font-family        global font
--scrollbar-thumb    scrollbar color
```

## Accessibility in Styles
- Color contrast: WCAG AA minimum (4.5:1 text, 3:1 UI components)
- Focus indicators: visible outline for keyboard navigation
- Motion: respect `prefers-reduced-motion`

```css
@media (prefers-reduced-motion: reduce) {
  .animated { animation: none; transition: none; }
}
```
