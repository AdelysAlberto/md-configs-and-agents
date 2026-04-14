# Copilot Instructions

> CSR · React 19 · React Router 7 · Bun · TypeScript strict · TailwindCSS 4
>
> Para contexto de negocio, dominios, glosario y flujos → [`project-context.instructions.md`](.github/instructions/project-context.instructions.md)

---

## Stack Tecnológico

| Tecnología | Versión | Uso |
|---|---|---|
| Bun | latest | Runtime, bundler, servidor, package manager |
| React | 19.2+ | UI framework |
| TypeScript | strict (ESNext) | Lenguaje |
| React Router | 7.13+ | Routing (`createBrowserRouter`) |
| Zustand | 5.0+ | Estado global (auth, UI) |
| TanStack Query | 5.90+ | Server state, caching, fetching |
| Zod | 4.3+ | Validación de formularios |
| TailwindCSS | 4.2+ | Utilidades CSS |
| Biome | 2.4+ | Linting + formatting |

---

## Reglas Críticas Globales

1. **Sin librerías UI de terceros** — No MUI, shadcn, Ant Design. Solo `Base*` custom → `components.instructions.md`
2. **Functional Programming Only** — No classes, constructors, `this`, inheritance → `coding-standards.instructions.md`
3. **Vertical Slicing** — Organizar por feature/dominio, no por capa técnica → `architecture.instructions.md`
4. **Service Layer** — `UI → Hook (TanStack Query) → Adapter → Service → httpRequest` → `services-hooks.instructions.md`
5. **Sin colores hardcodeados** — Solo `var(--color-*)` del design system → `styling.instructions.md`
6. **sessionStorage** — ❌ Nunca localStorage → `state-management.instructions.md`
7. **Sin `any`** — Tipos estrictos siempre → `coding-standards.instructions.md`
8. **Sin `console.log`** — Eliminar antes de commit → `coding-standards.instructions.md`
9. **`encodeURIComponent`** en path params de URLs → `services-hooks.instructions.md`
10. **Validar con Zod** antes de enviar formularios → `coding-standards.instructions.md`
11. **Cero errores** — Verificar con `get_errors` antes de entregar → `verification-checklist.instructions.md`
12. **Bun, no Node.js** — `bun install`, `bun run`, no npm/yarn/pnpm → `config-setup.instructions.md`

---

## Archivos Modulares de Instrucciones

| Archivo | applyTo | Contenido |
|---|---|---|∫
| [`architecture.instructions.md`](.github/instructions/architecture.instructions.md) | `src/**` | Estructura, capas, routing, layouts |
| [`components.instructions.md`](.github/instructions/components.instructions.md) | `src/components/**`, `src/pages/**/components/**` | Base components, props, diseño |
| [`styling.instructions.md`](.github/instructions/styling.instructions.md) | `src/**/*.css`, `src/constants/colors.ts`, `src/components/**`, `src/pages/**` | Design system, colores, Tailwind |
| [`services-hooks.instructions.md`](.github/instructions/services-hooks.instructions.md) | `src/services/**`, `src/hooks/**`, `src/adapters/**`, `src/providers/http/**` | HTTP client, services, hooks, adapters |
| [`state-management.instructions.md`](.github/instructions/state-management.instructions.md) | `src/store/**`, `src/hooks/**`, `src/providers/**` | Zustand, TanStack Query, sessionStorage |
| [`coding-standards.instructions.md`](.github/instructions/coding-standards.instructions.md) | `src/**/*.ts`, `src/**/*.tsx` | TypeScript, naming, imports, seguridad |
| [`config-setup.instructions.md`](.github/instructions/config-setup.instructions.md) | `package.json`, `tsconfig.json`, `biome.json`, `build.ts` | Bun, Biome, TS config, scripts |
| [`ux-design.instructions.md`](.github/instructions/ux-design.instructions.md) | `src/pages/**`, `src/layouts/**` | Patrones UX, estados, accesibilidad |
| [`verification-checklist.instructions.md`](.github/instructions/verification-checklist.instructions.md) | `**` | Checklist post-tarea, comandos |
| [`project-context.instructions.md`](.github/instructions/project-context.instructions.md) | `**` | Contexto de negocio, dominios, glosario, flujos |

---

## Archivos de Referencia en `.github/`

| Archivo | Propósito |
|---|---|
| `CLAUDE.md` | Reglas específicas para Bun runtime |
| `project-init-prompt.md` | Template genérico de inicialización (NO aplica a este proyecto) |
| `copilot-instructions-backup.md` | Backup del archivo original monolítico |

---

## Lookup Rápido

| Necesito... | Ubicación |
|---|---|
| Rutas de la app | `src/constants/routes.ts` → `PATHS` |
| Config / URLs de API | `src/constants/config.ts` |
| Colores del design system | `src/index.css` (CSS vars) |
| Colores JS tokens | `src/constants/colors.ts` |
| Auth store | `src/store/authStore.ts` |
| HTTP client | `src/providers/http/request.http.ts` |
| Base components | `src/components/` (barrel: `index.ts`) |

---

## Comandos de Verificación

```bash
bun run check          # Biome linting
bun run fix            # Biome auto-fix
bun run dev            # Dev server (port 3000)
bun run build          # Build producción
```

---

## Referencias

| Documento | Link |
|---|---|
| Backoffice Overview | [../Docu/Backoffice/00-BACKOFFICE-OVERVIEW.md](../../Docu/Backoffice/00-BACKOFFICE-OVERVIEW.md) |
| Development Plan | [../Docu/Backoffice/01-DEVELOPMENT-PLAN.md](../../Docu/Backoffice/01-DEVELOPMENT-PLAN.md) |
| Project Overview | [../../Docu/00-PROJECT-OVERVIEW.md](../../Docu/00-PROJECT-OVERVIEW.md) |
| Auth Module | [../../Docu/01-AUTH-REGISTRO.md](../../Docu/01-AUTH-REGISTRO.md) |
| Security | [../../Docu/11-SEGURIDAD.md](../../Docu/11-SEGURIDAD.md) |
