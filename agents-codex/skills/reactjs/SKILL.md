---
name: reactjs
description: React Clean Code standards, pure functional components, modular routing, TanStack Query, ErrorBoundary, and Biome. Use when working on files matching: **/*.tsx, **/*.ts, **/*.js, **/*.jsx.
---

# React & Clean Code Standards

## 1. Core Invariants

- **Pure Functional Components**: Prohibit `class`, `this`, and `React.Component` (except for `ErrorBoundary`). Use direct `export const Component = () => ...` declarations.
- **Never use `React.FC` or `React.FunctionComponent`**: Type props directly in the argument destructuring `({ title, children }: Props)`.
- **Linter & Formatter**: Biome standard (`biome.json`). Always pass `bun run biome:check` / `pnpm fix`.
- **Styles**: CSS Modules exclusively (`*.module.css`) with design tokens. No inline styles.
- **Internationalization**: All user-facing text must use `t('key')`.

---

## 2. Component Design & Prop Typing

```typescript
// ✅ GOOD: Direct typing, pure functional, CSS module integration
import type { ReactNode } from 'react';
import styles from './Button.module.css';

interface ButtonProps {
  children: ReactNode;
  variant?: 'primary' | 'secondary';
  onClick: () => void;
}

export const Button = ({ children, variant = 'primary', onClick }: ButtonProps) => {
  return (
    <button
      type="button"
      className={`${styles.button} ${styles[`button--${variant}`]}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
};
```

---

## 3. Modular Navigation Architecture (`src/navigation/`)

Every React application must structure its routes in a modular way in `src/navigation/`, isolating protection logic, lazy loading, and layouts from `App.tsx`.

### 📂 Mandatory File Structure

```text
src/navigation/
├── paths.ts             # Constant PATHS object with all routes
├── PrivateRoute.tsx     # Authenticated route guard (<Outlet /> / <Navigate />)
├── PublicRoute.tsx      # Public route guard (<Outlet /> / <Navigate />)
├── private.routes.tsx   # Private RouteObject with lazy imports and MainLayout
├── public.routes.tsx    # Public RouteObject with lazy imports
└── index.ts             # Barrel that exposes <AppRoutes /> via useRoutes()
```

### 📄 3.1 `src/navigation/paths.ts`

```typescript
export const PATHS = {
  HOME: '/',
  LOGIN: '/login',
  USERS: '/users',
  SETTINGS: '/settings',
} as const;
```

### 📄 3.2 `src/navigation/PrivateRoute.tsx` & `PublicRoute.tsx`

```typescript
// PrivateRoute.tsx
import { Navigate, Outlet } from 'react-router-dom';
import { useAuth } from '@/providers/AuthProvider';
import { PATHS } from './paths';

export const PrivateRoute = () => {
  const { isAuthenticated, isLoading } = useAuth();

  if (isLoading) return <LoadingSpinner />;
  return isAuthenticated ? <Outlet /> : <Navigate to={PATHS.LOGIN} replace />;
};
```

### 📄 3.3 `src/navigation/private.routes.tsx`

```typescript
import { lazy, Suspense } from 'react';
import { Outlet, type RouteObject } from 'react-router-dom';
import { MainLayout } from '@/layouts/MainLayout';
import { PrivateRoute } from '@/navigation/PrivateRoute';
import { PATHS } from '@/navigation/paths';

const DashboardPage = lazy(() => import('@/modules/dashboard/pages/DashboardPage'));
const UsersPage = lazy(() => import('@/modules/users/pages/UsersPage'));

const SuspenseWrapper = ({ children }: { children: React.ReactNode }) => (
  <Suspense fallback={<LoadingSpinner />}>
    {children}
  </Suspense>
);

export const privateRoutes: RouteObject = {
  element: <PrivateRoute />,
  children: [
    {
      element: (
        <MainLayout>
          <SuspenseWrapper>
            <Outlet />
          </SuspenseWrapper>
        </MainLayout>
      ),
      children: [
        { path: PATHS.HOME, element: <DashboardPage /> },
        { path: PATHS.USERS, element: <UsersPage /> },
        { path: '*', element: <DashboardPage /> },
      ],
    },
  ],
};
```

### 📄 3.4 `src/navigation/index.ts` and Ultra-Clean `App.tsx`

```typescript
// src/navigation/index.ts
import { useRoutes } from 'react-router-dom';
import { privateRoutes } from './private.routes';
import { publicRoutes } from './public.routes';

export * from './paths';
export const AppRoutes = () => useRoutes([publicRoutes, privateRoutes]);

// src/App.tsx
export const App = () => (
  <ErrorBoundary>
    <QueryProvider>
      <ThemeProvider>
        <AuthProvider>
          <BrowserRouter>
            <AppRoutes />
          </BrowserRouter>
        </AuthProvider>
      </ThemeProvider>
    </QueryProvider>
  </ErrorBoundary>
);
```

---

## 4. TanStack Query Standard (Custom Hooks per Module)

Making direct HTTP requests or `useEffect` for fetching inside UI components is strictly prohibited. All API consumption must be encapsulated in custom hooks within `src/modules/<module>/hooks/`.

### 📄 Example: `src/modules/users/hooks/useUsersQuery.ts`

```typescript
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { usersService } from '../services/users.service';
import type { UserFilterParams, UserStatus } from '../types/users.types';

export const USERS_QUERY_KEY = ['users'];

// Hook for reads (Queries)
export const useUsersQuery = (params: Partial<UserFilterParams>) => {
  return useQuery({
    queryKey: [...USERS_QUERY_KEY, params],
    queryFn: () => usersService.listUsers(params),
  });
};

// Hook for mutations with cache invalidation
export const useToggleUserStatusMutation = () => {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: ({ id, currentStatus }: { id: string; currentStatus: UserStatus }) =>
      usersService.toggleUserStatus(id, currentStatus),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: USERS_QUERY_KEY });
    },
  });
};
```

---

## 5. ErrorBoundary Standard (Global Exception Catching)

An `ErrorBoundary` must be included at the root of the application to prevent blank screens from runtime rendering exceptions.

### 📄 Example: `src/components/ErrorBoundary/ErrorBoundary.tsx`

```typescript
import { Component, type ErrorInfo, type ReactNode } from 'react';
import { Button } from '@/components/Button';

interface ErrorBoundaryProps {
  children: ReactNode;
}

interface ErrorBoundaryState {
  hasError: boolean;
  error: Error | null;
}

export class ErrorBoundary extends Component<ErrorBoundaryProps, ErrorBoundaryState> {
  public override state: ErrorBoundaryState = {
    hasError: false,
    error: null,
  };

  public static getDerivedStateFromError(error: Error): ErrorBoundaryState {
    return { hasError: true, error };
  }

  public override componentDidCatch(error: Error, errorInfo: ErrorInfo): void {
    console.error("🔴 ErrorBoundary caught a runtime error:", error, errorInfo);
  }

  private handleReset = (): void => {
    this.setState({ hasError: false, error: null });
    window.location.reload();
  };

  public override render(): ReactNode {
    if (this.state.hasError) {
      return (
        <div className="min-h-screen w-full bg-slate-900 text-[#F8FAFC] flex items-center justify-center p-6">
          <div className="max-w-md w-full bg-[#151C28] border border-red-500/30 rounded-xl p-6 space-y-4">
            <h2 className="text-lg font-bold">Something went wrong</h2>
            <p className="text-xs text-slate-400">Runtime exception.</p>
            <Button variant="primary" size="sm" onClick={this.handleReset}>
              Retry Loading
            </Button>
          </div>
        </div>
      );
    }

    return this.props.children;
  }
}
```

---

## 6. Hooks Hygiene & Anti-Patterns

### ❌ BAD (Redundant State via useEffect)

```typescript
// ❌ BAD: Synchronizing state that could be derived directly during render
const [fullName, setFullName] = useState('');
useEffect(() => {
  setFullName(`${firstName} ${lastName}`);
}, [firstName, lastName]);
```

### ✅ GOOD (Derived State)

```typescript
// ✅ GOOD: Compute directly during render
const fullName = `${firstName} ${lastName}`;
```
