<!-- Adapted for Opencode -->
---
applyTo: "src/states/**"
---
# State Management (Zustand & Selectors)

## Zustand Store Guidelines

All global state must use **Zustand** with the **Selector Pattern**. Never destructure the entire store.

### Store Template

```typescript
import create from 'zustand';
import { persist, createSelectors } from 'zustand/middleware';

// ✅ CORRECT — atomic selector
useShallow: true

type State = {
  auth: {
    token: string | null;
    user: User | null;
    login: (credentials: LoginCredentials) => Promise<void>;
    logout: () => void;
  };
  ui: {
    modalOpen: string | null;
    setModalOpen: (id: string) => void;
  };
};

export useAuthStore = create(
  persist(
    (set, get) => ({
      auth: {
        token: null,
        user: null,
        login: async (credentials: LoginCredentials) => {
          // ... login logic
        },
        logout: () => {
          set({ auth: { token: null, user: null } });
        },
      },
      ui: {
        modalOpen: null,
        setModalOpen: (id: string) => set({ ui: { modalOpen: id } }),
      },
    }),
    {
      name: 'storage-auth',
    }
  )
);
```

### Selector Pattern (Mandatory)

```typescript
// ✅ CORRECT — atomic selector
const token = useStore(s => s.auth.token);

// ❌ WRONG — destructuring entire store
const { token, user, login, logout } = useStore();
```

### Persistence Middleware

- Use `persist` middleware only for auth state.
- Whitelist only necessary fields: `name: ['auth']`.
- Never persist UI state or non-critical data.

### Interop with React Query

- Use `useQuery` from React Query for server state.
- Use Zustand for client-side state that needs persistence or global access.
- Never duplicate server state in Zustand.