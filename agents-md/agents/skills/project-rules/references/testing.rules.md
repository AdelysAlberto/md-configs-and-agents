---
trigger: model_decision
---

<!-- Adaptado para Antigravity -->
---
applyTo: "src/**/__tests__/**, src/**/*.test.ts, src/**/*.test.tsx, src/test/**"
---

# Testing

> [!IMPORTANT]
> **REGLA DE FRECUENCIA DE PRUEBAS**:
> - **NO** ejecutar la suite de pruebas (`pnpm test` / `vitest`) tras cada cambio pequeño o reparación intermedia.
> - Ejecutar la suite de pruebas **ÚNICAMENTE AL FINAL FINAL** de toda la tarea.
> - Durante pasos intermedios, usar solo `pnpm typecheck`.

## Stack

- **Vitest** — test runner (not Jest)
- **React Testing Library** — component testing
- **Setup** — `src/setupTests.ts`

## Test File Location

Los tests deben ir siempre junto al módulo que están probando, preferiblemente dentro de una carpeta `__test__` (o `test`). Esto agrupa los archivos de pruebas y mantiene la estructura limpia.

```
src/components/MyComponent/
├── MyComponent.tsx
├── MyComponent.module.css
└── __test__/
    └── MyComponent.test.tsx

src/hooks/__test__/
└── useGetAccount.test.ts
```

## Mocking Rules

- **Ubicación de Mocks Globales**: Todos los mocks globales, especialmente los handlers de MSW (Mock Service Worker), **DEBEN ir en la carpeta `src/mocks`**.
- **MSW sobre vi.mock()**: Prefiere aislar peticiones de red usando MSW en lugar de mockear hooks o librerías HTTP directamente.

## Component Testing Principles

Test **behavior**, not implementation details:

```typescript
// ❌ WRONG — testing implementation
expect(wrapper.state().isLoading).toBe(true);

// ✅ CORRECT — testing behavior
expect(screen.getByRole('progressbar')).toBeInTheDocument();
```

## Basic Component Test Template

```typescript
import { render, screen, fireEvent } from '@testing-library/react';
import { describe, it, expect, vi } from 'vitest';
import MyComponent from '../MyComponent';

describe('MyComponent', () => {
  it('renders the title', () => {
    render(<MyComponent title="Hello" />);
    expect(screen.getByText('Hello')).toBeInTheDocument();
  });

  it('calls onSubmit when button is clicked', () => {
    const onSubmit = vi.fn();
    render(<MyComponent onSubmit={onSubmit} />);
    fireEvent.click(screen.getByRole('button', { name: /submit/i }));
    expect(onSubmit).toHaveBeenCalledOnce();
  });
});
```

## Mocking

```typescript
// Mock a module
vi.mock('src/store/useAuth.store', () => ({
  useAuth: () => ({ user: { id: '1', name: 'Test User' }, isAuthenticated: true }),
}));

// Mock a hook return value
vi.mock('src/hooks/useGetAccount.hook', () => ({
  default: () => ({ data: mockAccount, isLoading: false }),
}));
```

## TanStack Query in Tests

Wrap components in a test `QueryClientProvider`:

```typescript
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

const createTestQueryClient = () => new QueryClient({
  defaultOptions: { queries: { retry: false } },
});

const renderWithQuery = (ui: React.ReactElement) => {
  const client = createTestQueryClient();
  return render(
    <QueryClientProvider client={client}>{ui}</QueryClientProvider>
  );
};
```

## Accessibility in Tests

Test keyboard and ARIA accessibility:

```typescript
it('is keyboard navigable', () => {
  render(<MyForm />);
  const input = screen.getByRole('textbox', { name: /email/i });
  input.focus();
  expect(input).toHaveFocus();
});
```

## What to Test

- ✅ Component renders correctly with required props
- ✅ User interactions (click, type, submit)
- ✅ Conditional rendering (loading, error, empty states)
- ✅ Custom hooks (with `renderHook`)
- ✅ Critical business logic in utilities
- ❌ Don't test library internals or implementation details
