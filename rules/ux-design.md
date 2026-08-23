---
trigger: model_decision
---

# UX & Accessibility

## Mobile-First (NON-NEGOTIABLE)

Design order: **320px → 768px → 1280px+**

1. Start with 320px — make it perfect
2. Tablet (768px-1024px) — adapt and enhance
3. Desktop (1280px+) — full experience

Testing required:
- Chrome DevTools: iPhone SE, iPhone 14 Pro, Galaxy S20
- Test landscape AND portrait
- Test touch interactions
- Slow 3G throttling for perceived performance

## Touch Targets

| Element | Minimum |
|---|---|
| Buttons | `48×48px` |
| Inputs | `48px height` |
| Icons (interactive) | `44×44px` tap area |
| Links in lists | `44px tap area` |

```css
/* Touch-friendly default */
.button { min-height: 48px; min-width: 48px; }
.input  { min-height: 48px; }
```

## Accessibility (WCAG AA minimum)

### Semantic HTML
```html
<!-- ❌ Wrong -->
<div onClick={handleNav}>Menu</div>

<!-- ✅ Correct -->
<button type="button" onClick={handleNav}>Menu</button>
<nav aria-label="Main navigation">...</nav>
<main>...</main>
```

### ARIA attributes
```tsx
// Interactive components
<button
  aria-label="Close dialog"
  aria-pressed={isOpen}
  aria-disabled={disabled}
>

// Form fields
<input
  aria-required="true"
  aria-invalid={!!error}
  aria-describedby={error ? "field-error" : undefined}
/>
<p id="field-error" role="alert">{error}</p>

// Custom select/combobox
<div role="combobox" aria-expanded={isOpen} aria-haspopup="listbox">
<ul role="listbox" aria-label="Options">
<li role="option" aria-selected={selected}>
```

### Keyboard Navigation
All interactive elements MUST be keyboard-accessible:
- `Tab` / `Shift+Tab` — focus navigation
- `Enter` / `Space` — activate buttons, toggle
- `Escape` — close modals/dropdowns
- Arrow keys — navigate lists, sliders

### Color Contrast
- Text: 4.5:1 minimum (WCAG AA)
- UI components/borders: 3:1 minimum
- Use CSS custom property tokens — never hardcode colors

### iOS input zoom prevention
```css
/* Prevents iOS from zooming in on input focus */
input, select, textarea { font-size: 16px; }
```

## Performance in UI
- Memoize expensive calculations: `useMemo`
- Memoize callbacks passed to children: `useCallback`
- Memoize components re-rendering unnecessarily: `React.memo`
- Prefer portal-based dropdowns/modals (avoid overflow clipping + z-index issues)
- Use `loading` / skeleton states for async content — no layout shifts
