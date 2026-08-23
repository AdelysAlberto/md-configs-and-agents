<!-- Adaptado para Opencode -->
---
applyTo: "src/**"
---
# Architecture

## Project Structure

```
src/
├── assets/            # Static assets (images, icons, fonts)
├── baseComponents/    # Project-specific wrappers around @sice/frontend-components
├── config/            # App-wide configuration and theme
├── constants/         # TypeScript union types / enums
├── enums/             # TypeScript union types / enums
├── hooks/             # Custom React hooks (data fetching, UI logic)
├── infrastructure/    # Cross-cutting concerns
│   ├── http/              # Axios instances (privateRequest, publicRequest)
│   ├── lang/              # i18n setup + translation files (en/, es/)
│   ├── layout/            # App layout components
│   ├── secure/            # Auth guards, permission checks
│   └── store/             # localStorage / sessionStorage helpers
├── interfaces/        # TypeScript interfaces (IEntity naming convention)
├── pages/             # Page-level components (route entry points)
├── router/            # React Router config (private.router, public.router)
├── services/          # API URL builders
│   └── api/               # Endpoint definitions per domain
├── states/            # Zustand global state stores (per domain)
│   └── global/            # Auth store, theme state
├── styles/            # Global CSS files
├── types/             # TypeScript type aliases
├── utils/             # Pure utility functions
│   └── envs.ts            # Environment variable access (ONLY source of env vars)
└── vite-env.d.ts
```

## Naming Conventions

| Entity | Convention | Example |
|---|---|---|
| Components | PascalCase | `AccountCard.tsx` |
| Hooks | camelCase + `use` prefix | `useGetAccount.hook.ts` |
| Stores | camelCase + `use` prefix | `useAuth.store.ts` |
| Services/APIs | camelCase | `accounts.api.ts` |
| Interfaces | `I` prefix + PascalCase | `IAccount`, `IButton` |
| Types | PascalCase with `T` prefix | `TAccountStatus` |
| CSS Modules | camelCase | `accountCard.module.css` |

## Page Structure

Pages in `src/pages/` are route entry points only — they should NOT contain business logic:

```typescript
// src/pages/private/AccountSummary/AccountSummary.tsx
const AccountSummaryPage = () => {
  const { data, isLoading } = useGetAccount({ accountId });

  if (isLoading) return <BaseLoading />;

  return <AccountSummaryLayout account={data} />;
};
```

Business logic → hooks. Layout → components. Data → services + hooks.

## Routing (React Router 6)

- Routes defined in `src/router/router.tsx`
- Protected routes use guards in `src/router/ProtectedRouted.tsx`
- Use lazy loading for page-level code splitting:

```typescript
// src/router/private.router.tsx
const AccountPage = lazy(() => import('./pages/private/Account/AccountPage'));
```

## Environment Variables

**ONLY** access env vars through `src/utils/envs.ts` — never use `import.meta.env` directly:

```typescript
// ❌ WRONG
const url = import.meta.env.VITE_API_URL;

// ✅ CORRECT
import envs from 'src/utils/envs';
const url = envs.API.API_URL;
```

## Barrel Files

Each major directory should have an `index.ts` barrel for clean imports:

```typescript
// src/components/index.ts — already exists, use it
import { BaseButton, BaseInput } from 'src/components';
```

## Module Boundaries

- `pages/` → can import from any `src/` directory
- `baseComponents/` → must NOT import from `pages/`
- `hooks/` → can import from `services/`, `states/`, `utils/`, `interfaces/`
- `services/` → can only import from `utils/envs.ts` and `interfaces/`
- `store/` → can import from `interfaces/` and `utils/`