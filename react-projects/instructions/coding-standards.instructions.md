---
applyTo: "src/**/*.ts, src/**/*.tsx"
---

# Coding Standards — r365-backoffice

## TypeScript Strict

- ❌ **Nunca `any`** — usar tipos específicos o `unknown`
- ✅ `interface` para shapes de objetos, `type` para unions/intersections
- ✅ Strict mode habilitado en tsconfig.json
- ✅ Usar tipos built-in de React: `React.ReactNode`, `React.ComponentProps`

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

## Imports

- ✅ Path alias `@/` para imports desde `src/`
- ✅ Barrel exports (`index.ts`) en cada carpeta de componente
- ✅ `import type` para tipos (Biome lo requiere)

```typescript
// ✅ CORRECTO
import { BaseButton } from "@/components";
import type { User } from "@/types/user.types";

// ❌ INCORRECTO
import { BaseButton } from "../../components/BaseButton/BaseButton";
import { User } from "@/types/user.types"; // falta 'type'
```

## Formularios

- ✅ Siempre validar con **Zod** antes de enviar
- ✅ Mostrar error por campo, no solo toast general
- ❌ No enviar formularios sin validación client-side

## Seguridad

- ✅ Sanitizar inputs del usuario (prevenir XSS)
- ✅ HTTPS para todas las llamadas API
- ❌ Nunca loguear datos sensibles (tokens, passwords, IDs de usuario)
- ❌ No `console.log` en producción — eliminar antes de commit

## Lo que NUNCA se hace

- ❌ `any` en TypeScript
- ❌ `class`, `constructor`, `this`, inheritance
- ❌ `console.log` / `console.error` en producción
- ❌ Código comentado — eliminar antes de commit
- ❌ Librerías de UI de terceros (MUI, shadcn, etc.)
- ❌ Lógica de negocio en componentes UI
- ❌ Datos sensibles en logs
- ❌ localStorage (usar sessionStorage)
