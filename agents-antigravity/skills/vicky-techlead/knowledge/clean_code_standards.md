# Clean Architecture & Code Standards Framework - Vicky (Tech Lead)

This document details the code quality standards, Clean Architecture rules, and Result Pattern guidelines enforced by **Vicky**.

---

## 1. Non-Negotiable Technical Directives

1. **Pure Functional Code**: OOP, `class`, and `this` are strictly prohibited. Write pure functional TypeScript/JavaScript.
2. **Vertical Slicing & Screaming Architecture**: Organize all business domain code by module inside `src/modules/<FeatureName>/`.
3. **Result Pattern Error Handling**: Never throw exceptions from services. Return explicit result objects (`{ success: true, value }` or `{ success: false, error }`).
4. **Zustand Selector Hygiene**: Never destructure entire global Zustand stores. Use `useShallow` or atomic state selectors.
5. **Pure CSS Modules**: Use CSS Modules exclusively (`*.module.css`). Do NOT use inline styles or TailwindCSS unless explicitly instructed.
6. **Pre-Commit Verification**: Run `pnpm fix && pnpm tsc --noEmit && pnpm build` prior to finishing any task.

---

## 2. Directory Structure

```text
src/
├── modules/
│   ├── auth/
│   │   ├── components/       # Authentication-specific UI
│   │   ├── hooks/            # Reactive logic/hooks
│   │   ├── services/         # HTTP adapters / API clients
│   │   └── types/            # Types and validation schemas (Zod/DTOs)
│   ├── products/
│   │   ├── components/
│   │   ├── services/
│   │   └── types/
└── shared/                   # Cross-cutting utilities (UI primitives, logger, HTTP client)
```

---

## 2. Patrones de Diseño Obligatorios

1. **Result Pattern**: Evitar `try/catch/throw` para el control de flujo de negocio. Retornar siempre objetos estructurados `{ ok: true, data }` o `{ ok: false, error }`.
2. **KISS & Clean Code**: Funciones cortas (< 50 líneas), early returns, nombres descriptivos de variables y funciones.
3. **Adapter / Mapper Pattern**: Desacoplar la UI de las respuestas de API externas. Mapear DTOs externos a modelos de dominio antes de consumirlos en componentes UI.
4. **Selector Pattern en Estado Global**: Al usar Zustand o Redux, exigir el uso de selectores individuales para evitar re-renders innecesarios.
