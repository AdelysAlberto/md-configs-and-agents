# Copilot Instructions

> CSR · React 19 · React Router 7 · Bun · TypeScript strict · TailwindCSS 4
>

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
13. **UI Literals / i18n** — All user-visible text MUST be defined in `src/locales/en/`. Never hardcode UI strings in JSX. Check if literal exists first; create it if not. Language: **English only**. → `i18n.instructions.md`

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
| [`ux.instructions.md`](.github/instructions/ux.instructions.md) | `src/pages/**`, `src/layouts/**` | Patrones UX, estados, accesibilidad |
| [`verification-checklist.instructions.md`](.github/instructions/verification-checklist.instructions.md) | `**` | Checklist post-tarea, comandos |
| [`i18n.instructions.md`](.github/instructions/i18n.instructions.md) | `src/**` | UI literals, i18n setup, locales structure |
| [`project-context.instructions.md`](.github/instructions/project-context.instructions.md) | `**` | Contexto de negocio, dominios, glosario, flujos |

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


### **Before Completing Task**
1. ✅ Run all verification commands listed above
2. ✅ Fix all errors and warnings found
3. ✅ Review changed files one more time
4. ✅ Ensure ALL checklist items are completed
5. ✅ Verify i18n translations are properly created
6. ✅ Confirm no sensitive data is logged
7. ✅ Commit changes with descriptive message in English following Conventional Commits

# Response Style (prose only — does NOT apply to code generation or rule compliance)

- Skip filler phrases ("I understand", "Let me know if...", "Here is the solution")
- After completing file operations, confirm in 1 line max
- Use bullet points for multiple notes
- No style rule overrides architecture rules, result pattern, or instruction files
- Code quality and rule compliance are always full priority

---

> **CRITICAL**: Do not mark a task as complete until ALL verification items pass. This is non-negotiable and mandatory for every single task.

> **Mantra**: Prioritize high-performance and clean code. Use modern standards (ES2022+), but favor simplicity and stability over novelty. Strictly avoid deprecated or legacy patterns. For every technical decision, choose the path that minimizes cognitive complexity and optimizes resource usage, ensuring code remains maintainable, robust, and efficient.
