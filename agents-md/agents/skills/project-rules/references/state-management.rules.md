---
trigger: model_decision
---

<!-- Adaptado para Antigravity -->
---
applyTo: "src/states/**, src/hooks/**"
---

# State Management

## Two Types of State

| Type | Tool | Location |
|---|---|---|
| Server/async state | TanStack Query | `src/hooks/` via `useQuery`/`useMutation` |
| Client/global UI state | Zustand | `src/store/` |

Never use TanStack Query for UI state. Never use Zustand for server data.

## Zustand Store Pattern

All stores follow a functional pattern — no classes. Stores live in `src/states/`:

```typescript
// src/states/example/useExample.state.ts
import { create } from 'zustand';

interface IExampleState {
  isOpen: boolean;
  setIsOpen: (open: boolean) => void;
  reset: () => void;
}

const initialState = { isOpen: false };

export const useExampleStore = create<IExampleState>((set) => ({
  ...initialState,
  setIsOpen: (isOpen) => set({ isOpen }),
  reset: () => set(initialState),
}));
```

## Key Stores

| Store | Location | Purpose |
|---|---|---|
| `useAuthStore` | `src/states/global/useAuth.store.ts` | Authentication state, user session |
| `theme.state.ts` | `src/states/theme.state.ts` | UI theme state |
| `localStorage.ts` | `src/infrastructure/store/localStorage.ts` | Persistent localStorage helpers |
| `sessionStorage.ts` | `src/infrastructure/store/sessionStorage.ts` | Persistent sessionStorage helpers |

## useAuthStore — Authentication

Always use `useAuthStore` for checking auth state — never read raw tokens from storage:

```typescript
// ✅ CORRECT
import { useAuthStore } from 'src/states/global/useAuth.store';
const { readWrite, readOnly } = useAuthStore();

// ❌ WRONG
const token = localStorage.getItem('token');
```

## useErrorStore — Global Errors

```typescript
import { useErrorStore } from 'src/store/useErrorStore.store';

const { setError, clearError } = useErrorStore();

// Display a global error
setError({ message: 'Something went wrong', code: 500 });
```

## Zustand Rules

- **Name stores with `use` prefix** — `useMyFeature`, not `myFeatureStore`
- Always expose a `reset()` action that restores initial state
- Keep store slices small and feature-scoped
- No derived state in store — compute it in the component or with `useMemo`
- Prefer separate stores over one massive global store

## Consuming Zustand (CRITICAL for v5+)

When reading state from a Zustand store inside a component, **you MUST use `useShallow`** if you are returning an object or an array. Returning new objects/arrays from a selector without `useShallow` will cause infinite re-renders (`Maximum update depth exceeded`) in Zustand 5+.

```tsx
import { useShallow } from 'zustand/react/shallow';

// ❌ WRONG — Returns a new object every time, causing infinite re-renders
const { isOpen, setIsOpen } = useExampleStore((state) => ({ 
  isOpen: state.isOpen, 
  setIsOpen: state.setIsOpen 
}));

// ✅ CORRECT — Using useShallow for objects
const { isOpen, setIsOpen } = useExampleStore(useShallow((state) => ({ 
  isOpen: state.isOpen, 
  setIsOpen: state.setIsOpen 
})));

// ✅ CORRECT — Selecting a single primitive value doesn't need useShallow
const isOpen = useExampleStore((state) => state.isOpen);
const setIsOpen = useExampleStore((state) => state.setIsOpen);
```

## Persistence

Use the helpers in `src/infrastructure/store/` for persisting state:

```typescript
// ❌ WRONG — direct localStorage
localStorage.setItem('key', JSON.stringify(data));

// ✅ CORRECT — use storage helper
import { getStorage, setStorage } from 'src/infrastructure/store/localStorage';
```

## TanStack Query Config

- Use `queryClient.invalidateQueries()` after mutations to keep server state fresh
- Set appropriate `staleTime` and `gcTime` per use case
- See `.github/instructions/services-hooks.instructions.md` for query patterns
