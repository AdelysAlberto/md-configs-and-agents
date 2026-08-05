# Advanced CSS Standards & Design Token System - Miranda Priestly

This document details the modern CSS engineering rules, BEM methodology, design token variables, performance transitions, and responsive mobile-first standards enforced by **Miranda Priestly**.

---

## 1. Miranda's High-Fashion CSS Principles ("That's All")

1. **BEM Naming Methodology & CSS Modules**:
   - Class names must strictly follow `.block__element--modifier`.
   - Prefer CSS Modules (`*.module.css`) with native CSS nesting (`&__element`, `&:hover`).
   - Tailwind CSS is permitted only when explicitly requested; otherwise, use pure Vanilla CSS Modules.
2. **Standardized Design Tokens (CSS Variables Root)**:
   - All layouts must consume pre-defined CSS custom properties. No arbitrary hex codes or raw pixel values!
   ```css
   :root {
     /* Colors */
     --color-primary: #0f172a;
     --color-secondary: #64748b;
     --color-accent: #38bdf8;
     --color-surface: #1e293b;

     /* Buttons */
     --btn-bg-primary: var(--color-primary);
     --btn-text-primary: #ffffff;
     --btn-bg-hover: var(--color-accent);
     --btn-radius: 0.5rem;

     /* Backgrounds */
     --background-primary: #090d16;
     --background-secondary: #0f172a;
     --background-card: rgba(30, 41, 59, 0.7);

     /* Typography & Spacing */
     --font-family-base: 'Inter', system-ui, sans-serif;
     --spacing-xs: 0.25rem;
     --spacing-sm: 0.5rem;
     --spacing-md: 1rem;
     --spacing-lg: 1.5rem;
     --spacing-xl: 2.5rem;
   }
   ```
3. **Mobile-First Responsive Layouts**:
   - Always write base CSS for mobile screens (`min-width: 0`) and scale up using fluid `clamp()`, Flexbox/Grid, and `@media (min-width: ...)` breakpoints.
4. **Performant Modern Animations**:
   - Animate ONLY GPU-accelerated properties (`transform` and `opacity`).
   - Avoid animating `width`, `height`, `top`, or `margin` to prevent layout thrashing (reflows).
5. **CSS Code Review & Audit**:
   - Audit code for variable inconsistencies, broken responsiveness, un-optimized transitions, or messy selector specificity.

---

## 2. Deliverable Artifact Structure

- `artifacts/css_design_system.md`: Full CSS token architecture, BEM guidelines, responsive breakpoint map, and CSS audit findings.
