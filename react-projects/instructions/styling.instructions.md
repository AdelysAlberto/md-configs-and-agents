---
applyTo: "src/**/*.css, src/constants/colors.ts, src/components/**, src/pages/**"
---

# Styling & Design System — r365-backoffice

## Enfoque: TailwindCSS 4 + CSS Variables

Este proyecto usa **TailwindCSS 4+** como sistema de utilidades CSS.
Todas las variables están definidas en `src/index.css`.

## Regla crítica: Siempre usar variables CSS

❌ **NUNCA** colores hardcodeados en componentes:
```tsx
// ❌ INCORRECTO
<div className="bg-[#0F362A]">
<div style={{ color: '#EF4444' }}>
```

✅ **SIEMPRE** variables CSS o clases Tailwind mapeadas:
```tsx
// ✅ CORRECTO
<div className="bg-[var(--color-primary)]">
<div className="text-[var(--color-danger)]">
```

## Paleta de colores (CSS Variables)

```css
/* Brand */
--color-primary: #0f362a      --color-secondary: #f37100
--color-accent: #3b82f6        --color-accent-light: #ebf2ff

/* Backgrounds */
--color-bg: #f5f7fa            --color-bg-card: #ffffff
--color-surface: #ffffff       --color-surface-2: #f0f3f8
--color-sidebar-bg: #0f362a

/* Text */
--color-text: #1a1d26          --color-text-secondary: #6b7280
--color-text-tertiary: #9ca3af --color-text-inverse: #ffffff

/* Status */
--color-success: #10b981       --color-danger: #ef4444
--color-warning: #f59e0b       --color-info: #3b82f6
/* Cada status tiene variantes -bg y -text */

/* Grayscale */
--color-gray-1: #ffffff  --color-gray-2: #f5f7fa  --color-gray-3: #e5e7eb
--color-gray-4: #d1d5db  --color-gray-5: #9ca3af  --color-gray-6: #1a1d26

/* Borders & Shadows */
--color-border: #e5e7eb        --color-divider: #f0f3f8
--shadow-sm / --shadow-md / --shadow-lg

/* Radius */
--radius-sm: 4px  --radius-md: 8px  --radius-lg: 12px  --radius-xl: 16px

/* Layout */
--sidebar-width: 240px         --sidebar-collapsed-width: 64px
--header-height: 64px

/* Transitions */
--transition-fast: 150ms ease  --transition-normal: 250ms ease
```

## Design tokens JS — `src/constants/colors.ts`

Synced con `r365-mobile/app-core/constants/Colors.ts`.
Usar para lógica condicional JS, NO para estilos directos.

## Reglas de estilo

- ✅ Usar clases de Tailwind para layout, spacing, typography
- ✅ Usar `var(--color-*)` para colores del design system
- ✅ Responsive: mobile-first con breakpoints de Tailwind
- ❌ No `!important` salvo casos excepcionales documentados
- ❌ No CSS Modules (este proyecto usa Tailwind, no CSS Modules)
- ❌ No CSS-in-JS (styled-components, emotion, etc.)
- ❌ No inline `style` attributes salvo valores dinámicos calculados
