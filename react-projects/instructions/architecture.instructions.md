---
applyTo: "src/**"
---

# Arquitectura — r365-backoffice

## Principios Arquitectónicos

1. **Vertical Slicing** — Organizar por feature/dominio, NO por capa técnica
2. **Screaming Architecture** — La estructura "grita" lo que hace la app, no los frameworks
3. **Functional Programming Only** — ❌ No classes, constructors, `this`, inheritance
4. **Service Layer Architecture** (obligatorio):
```
UI Component → Custom Hook (TanStack Query) → Adapter → Service → HTTP client
```

## Capas y responsabilidades

| Capa | Responsabilidad | Ubicación |
|---|---|---|
| **UI** | Presentación pura, sin lógica de negocio | `src/pages/*/`, `src/components/` |
| **Hook** | `useQuery` / `useMutation`, maneja loading/error | `src/hooks/`, `src/pages/*/use*.ts` |
| **Adapter** | Transforma respuesta API → tipo UI | `src/adapters/` |
| **Service** | HTTP fetch calls | `src/services/` |
| **HTTP client** | Wrapper fetch con interceptors JWT | `src/providers/http/` |

## Estructura del Proyecto

```
src/
├── index.html              # HTML shell
├── index.tsx               # Bun.serve entry — SPA fallback /*
├── frontend.tsx            # React root (ReactDOM.createRoot)
├── App.tsx                 # createBrowserRouter + Providers
├── index.css               # CSS vars (design system) + global reset
├── constants/              # Config, routes, colors, feature constants
├── types/                  # Shared TypeScript types
├── store/                  # Zustand stores (authStore, uiStore)
├── components/             # BASE COMPONENTS reutilizables (Base*)
├── layouts/                # AuthLayout, DashboardLayout (Sidebar + Header)
├── navigation/             # PrivateRoute, PublicRoute
├── providers/              # QueryProvider, http/
├── pages/                  # Features (vertical slicing)
├── adapters/               # API → UI transformers
├── services/               # HTTP calls
├── hooks/                  # Hooks reutilizables globales
└── mocks/                  # Mock data
```

## Routing

- Todas las rutas definidas en `src/constants/routes.ts` como objeto `PATHS`
- Rutas privadas → `<PrivateRoute>` (requiere `isAuthenticated`)
- Rutas públicas → `<PublicRoute>` (redirige si ya autenticado)
- Router: `createBrowserRouter` de React Router 7

## Layouts

- **AuthLayout** — Login, forgot-password
- **DashboardLayout** — Sidebar + Header + `<Outlet />`

## Reglas

- ✅ Cada feature en su carpeta bajo `pages/`
- ✅ Componentes específicos de feature dentro de `pages/Feature/components/`
- ✅ Hooks específicos de feature dentro de `pages/Feature/` como `use*.ts`
- ✅ Componentes compartidos solo en `src/components/`
- ❌ No mezclar concerns entre features
- ❌ No poner lógica de negocio en componentes UI
