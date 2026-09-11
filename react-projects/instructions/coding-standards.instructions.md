---
applyTo: "src/**/*.ts, src/**/*.tsx"
---

# Coding Standards — React & TypeScript Frontends

## Universal Anti-Pattern Elimination (Zero-Tolerance)

- **Zero Secrets in URLs / Query Params**: Tokens, API keys, and credentials must never travel via query params. Use standard `Authorization` headers or secure cookies.
- **Deterministic Lifecycle & Cleanup**: All `useEffect` subscriptions, WebSocket listeners, intervals (`setInterval`), and event listeners MUST return an explicit cleanup function to unregister on unmount.
- **Zero Silent Error Swallowing**: Never leave empty catch blocks (`catch (e) {}`). All async failures must be captured in error boundaries or explicit error state.
- **Zero N+1 Fetch Loops**: Never fetch items individually in a `.map()` or loop inside components. Fetch aggregated data or batch endpoints.
- **Scoped Subscriptions**: Client WebSocket or event subscriptions must be strictly scoped to the active entity/view (`room:${id}`), never wildcards (`*`).
- **Guarded Invariants & Business Rules**: Never rely solely on disabled buttons or cosmetic UI validation; validate inputs with Zod schemas before submission and handle backend constraint rejections gracefully.
- **Bounded State & Memory**: Avoid accumulating unbounded state in stores or local state arrays without pagination/virtualization.

---

## TypeScript Strict

- ❌ **Nunca `any`** — usar tipos específicos o `unknown` con type guards
- ✅ `interface` para shapes de objetos, `type` para unions/intersections
- ✅ Strict mode habilitado en tsconfig.json
- ✅ No `React.FC` — tipar props directamente en la firma de la función

---

## Functional Programming Only

```typescript
// ✅ CORRECTO
const calculateTotal = (items: Item[]): number =>
  items.reduce((sum, item) => sum + item.price, 0);

// ❌ INCORRECTO
class Calculator {
  constructor(private items: Item[]) {}
  getTotal() { return this.items.reduce((s, i) => s + i.price, 0); }
}
```

---

## Naming Conventions

| Elemento | Convención | Ejemplo |
|---|---|---|
| Componentes | `PascalCase` | `BaseButton`, `TransactionFilters` |
| Hooks | `camelCase` + `use` | `useLogin`, `useTransactions` |
| Stores | `camelCase` + `Store` | `authStore`, `uiStore` |
| Types | sufijo `.types.ts` | `auth.types.ts`, `user.types.ts` |
| Services | sufijo `.service.ts` | `auth.service.ts` |
| Adapters | sufijo `.adapter.ts` | `users.adapter.ts` |
| Constantes valor | `UPPER_SNAKE_CASE` | `MAX_RETRIES`, `API_TIMEOUT` |
| Constantes objeto | `PascalCase` / `camelCase` | `PATHS`, `config` |

---

## Imports

- ✅ Path alias `@/` para imports desde `src/`
- ✅ Barrel exports (`index.ts`) en cada carpeta de componente
- ✅ `import type` para tipos (Biome / TS requirement)

---

## Formularios & Validación

- ✅ Siempre validar con **Zod** antes de enviar
- ✅ Mostrar error por campo, no solo toast general
- ✅ **Zod v4** — usar validators standalone: `z.email()`, `z.url()`, `z.uuid()` — ❌ NO `z.string().email()`
- ❌ No enviar formularios sin validación client-side

---

## Seguridad

- ✅ Sanitizar inputs del usuario (prevenir XSS)
- ✅ HTTPS para todas las llamadas API
- ❌ Nunca loguear ni exponer datos sensibles (tokens, passwords, PII)
- ❌ No `console.log` en producción — eliminar antes de commit
- ❌ No usar `localStorage` para tokens de autenticación sensibles (usar `sessionStorage` o HttpOnly cookies)

---

## Lo que NUNCA se hace

- ❌ `any` en TypeScript
- ❌ `class`, `constructor`, `this`, inheritance
- ❌ `catch (e) {}` vacío
- ❌ Tokens o secrets en URL query params
- ❌ `console.log` / `console.error` en producción
- ❌ Código comentado — eliminar antes de commit
- ❌ Librerías de UI de terceros desalineadas (MUI, shadcn, etc.)
- ❌ Lógica de negocio en componentes UI
