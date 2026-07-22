# NZTA Private Portal — Reglas de Antigravity

React 18 + TypeScript 5.7 SPA para el portal privado de NZ Transport Agency. Usa Vite 5, React Router 6, Zustand 4, TanStack Query 5, Zod 3, ESLint 9 y la librería `@sice/frontend-components`.

---

## Reglas Críticas (aplican a todos los archivos)

0. **Responde siempre en español.**

1. **La Verdad sobre el Acuerdo** — Identifica problemas, explica el por qué y da la solución correcta. Nunca aceptes malas prácticas. → `.agents/rules/coding-standards.rules.md`

2. **Solo programación funcional** — Prohibido usar `class`, `constructor`, `this`, `extends`. Usa funciones, hooks y Zustand. → `.agents/rules/coding-standards.rules.md`

3. **Cero `any`** — Usa tipos específicos o `unknown`. Evita `React.FC` — prefiere declaración directa de funciones. → `.agents/rules/coding-standards.rules.md`

4. **Sin comentarios `//`** — Exclusivamente JSDoc, en inglés. → `.agents/rules/coding-standards.rules.md`

5. **Librería UI `@sice/frontend-components` primero** — Verifica la librería siempre antes de construir UI nativo. Wrappers del proyecto en `src/baseComponents/`. → `.agents/rules/components.rules.md`

6. **TanStack Query para datos** — Todo servicio lleva un hook `useQuery`/`useMutation`. Cero llamadas HTTP desde los componentes. → `.agents/rules/services-hooks.rules.md`

7. **HTTP vía `src/infrastructure/http/`** — Usa `privateRequest.ts` + `publicRequest.ts`. Constructores de URL en `src/services/api/`. → `.agents/rules/services-hooks.rules.md`

8. **Cero estilos inline** — Solo CSS plano o CSS Modules. Nada de CSS-in-JS. → `.agents/rules/styling.rules.md`

9. **i18n para todo texto** — Modifica `en.json` y `es.json` simultáneamente. → `.agents/rules/i18n.rules.md`

10. **Solo Biome y TypeScript** — Usa `pnpm lint` (Biome) y `pnpm typecheck` antes de commits. Cada vez que termines una tarea, DEBES ejecutar el analizador de TypeScript y Biome para asegurar calidad. → `.agents/rules/config-setup.rules.md`

11. **Commits** — Formato: `#<BRANCH_ID> <type>(<scope>): <description>`. Usa `git branch | grep '\*'` para el ID. → `.agents/rules/commits.rules.md`

12. **Variables de entorno vía `src/utils/envs.ts`** — Nunca `import.meta.env` directamente en el código. → `.agents/rules/architecture.rules.md`

---

## Archivos de Instrucciones Modulares

| Archivo | applyTo | Dominio |
|---|---|---|
| `.agents/rules/coding-standards.rules.md` | `src/**` | TypeScript, FP, security, JSDoc, ESLint fixes |
| `.agents/rules/components.rules.md` | `src/baseComponents/**, src/pages/**` | Librería UI, base components, DRY, composición |
| `.agents/rules/services-hooks.rules.md` | `src/services/**, src/hooks/**` | Patrón API, TanStack Query, adapters |
| `.agents/rules/state-management.rules.md` | `src/states/**, src/hooks/**` | Stores Zustand, useAuthStore, manejo de errores |
| `.agents/rules/i18n.rules.md` | `src/**` | Claves de traducción, en/es, interpolación |
| `.agents/rules/architecture.rules.md` | `src/**` | Estructura, nombres, fronteras de módulos |
| `.agents/rules/styling.rules.md` | `src/**/*.css, src/styles/**` | CSS Modules, variables, sin estilos en línea |
| `.agents/rules/config-setup.rules.md` | `*.json, tsconfig*, vite.config.ts` | Versiones, scripts ESLint, variables env |
| `.agents/rules/testing.rules.md` | `src/**/__tests__/**, src/**/*.test.*` | Vitest, RTL, mocks |
| `.agents/rules/commits.rules.md` | `**` | Commits Convencionales con ID de rama |
| `.agents/rules/verification-checklist.rules.md` | `**` | Checklist antes de finalizar |
| `.agents/rules/reactjs.rules.md` | `**/*.tsx, **/*.ts, **/*.css` | Patrones de React 18 adaptados |

---

## Referencia Rápida del Proyecto

| Elemento | Valor |
|---|---|
| Librería UI | `import { Button, Table, ... } from '@sice/frontend-components'` |
| Componentes base del proyecto | `import { X } from 'src/baseComponents'` |
| Variables de entorno | `import envs from 'src/utils/envs'` |
| Estado de Auth | `import { useAuthStore } from 'src/states/global/useAuth.store'` |
| Hook de traducción | `import { useTranslation } from 'react-i18next'` |
| Peticiones HTTP privadas | `src/infrastructure/http/privateRequest.ts` |
| Peticiones HTTP públicas | `src/infrastructure/http/publicRequest.ts` |
| Constructores URL API | `src/services/api/` |
| Traducciones | `src/infrastructure/lang/en/`, `src/infrastructure/lang/es/` |

## Comandos de Verificación

```bash
pnpm lint     # Biome check & auto-fix
pnpm typecheck # Analizador de TypeScript
pnpm test     # Suite de Vitest
git branch | grep '\*'   # ID de la rama para el commit
```

### **Antes de Completar una Tarea**
1. ✅ Ejecutar comandos de verificación listados arriba
2. ✅ Corregir errores y warnings detectados
3. ✅ Revisar archivos cambiados una vez más
4. ✅ Asegurar cumplimiento del checklist (`.agents/rules/verification-checklist.rules.md`)
5. ✅ Verificar traducciones i18n
6. ✅ Confirmar que no hay logeo de datos sensibles
7. ✅ Hacer commit descriptivo en inglés siguiendo el estándar

> **Mantra de Antigravity**: Prioriza código limpio y de alto rendimiento. Usa estándares modernos (ES2022+), pero favorece la simplicidad y estabilidad. Nada de `any` ni `class`. Optimiza, documenta en JSDoc y no apruebes malas prácticas.
