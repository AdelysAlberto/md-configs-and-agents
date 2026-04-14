---
applyTo: "src/store/**, src/hooks/**, src/providers/**"
---

# State Management — r365-backoffice

## Estrategia de estado

| Tipo | Herramienta | Ejemplo |
|---|---|---|
| **Local** | `useState` | Visibilidad de modal, input temporal |
| **Global** | Zustand store | Auth, sidebar, preferencias UI |
| **Server** | TanStack Query | Listas de usuarios, transacciones, KYC |

## Zustand Stores

Los stores son objetos planos con funciones — ❌ No clases, no `this`.

### AuthStore (`src/store/authStore.ts`)

```typescript
interface AuthState {
  user: AdminUser | null;
  accessToken: string | null;
  isAuthenticated: boolean;
  login: (token: string, user: AdminUser) => void;
  logout: () => void;
}
```

- Token guardado en **sessionStorage** — ❌ NO localStorage
- Leer token fuera de React: `useAuthStore.getState().accessToken`

### UIStore (`src/store/uiStore.ts`)

```typescript
interface UIState {
  sidebarOpen: boolean;
  toggleSidebar: () => void;
}
```

## TanStack Query (Server State)

- Provider: `src/providers/QueryProvider.tsx`
- Patrón: ver → `services-hooks.instructions.md`
- `staleTime: 30_000` como default sensato
- Query keys: `['dominio', filtros]`

## Reglas

- ✅ Zustand para estado que persiste entre navegaciones (auth, UI)
- ✅ TanStack Query para TODO dato que viene del servidor
- ✅ `useState` para estado efímero de un solo componente
- ✅ sessionStorage para persistencia de sesión
- ❌ No localStorage — usar sessionStorage
- ❌ No Redux, no Context API para estado global
- ❌ No duplicar estado del servidor en Zustand — usar TanStack Query

## Ejemplo correcto vs incorrecto

```typescript
// ✅ CORRECTO — Server state en TanStack Query
const { data: users } = useUsers(filters);

// ❌ INCORRECTO — Server state en Zustand
const useUsersStore = create((set) => ({
  users: [],
  fetchUsers: async () => {
    const res = await getUsersService();
    set({ users: res.data });
  },
}));
```
