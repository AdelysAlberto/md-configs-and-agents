---
trigger: model_decision
---

# State Management

## State Decision Tree

| State type | Solution |
|---|---|
| Component-local UI state | `useState` |
| Complex local state machine | `useReducer` |
| Server / async data | React Query (`useQuery`/`useMutation`) |
| Global app state (persisted) | Zustand |
| Shared subtree state | React Context (sparingly) |

Rule: **Keep state as close as possible to where it is used.**

## Zustand — Selector Pattern (MANDATORY)

❌ **Wrong** — subscribes to entire store (causes unnecessary re-renders):
```ts
const { user, setUser, logout } = useAuthStore();
```

✅ **Correct** — subscribe only to needed slices:
```ts
const user = useAuthStore(state => state.user);
const setUser = useAuthStore(state => state.setUser);
const logout = useAuthStore(state => state.logout);
```

### Creating a new Zustand store

```ts
// src/store/useAuthStore.ts
import { create } from "zustand";

interface AuthState {
  user: User | null;
  setUser: (user: User | null) => void;
  logout: () => void;
}

const useAuthStore = create<AuthState>()(set => ({
  user: null,
  setUser: user => set({ user }),
  logout: () => set({ user: null }),
}));

export default useAuthStore;
```

Rules:
- Always `.ts` extension for stores
- Define a proper TypeScript interface
- Use `create<Interface>()(...)` with proper generic
- No classes — use plain functions inside Zustand

## State Persistence

| Need | Solution |
|---|---|
| Session-only | `sessionStorage` |
| Cross-session persistent | CookieStore API (async, modern browsers) |
| Store persistence in Zustand | Zustand `persist` middleware |

```ts
// Persistent Zustand store with persist middleware
import { create } from "zustand";
import { persist, createJSONStorage } from "zustand/middleware";

const useSettingsStore = create<SettingsState>()(
  persist(
    set => ({ theme: "dark", setTheme: theme => set({ theme }) }),
    { name: "settings-storage" },
  ),
);
```

## Token Validation

Use `react-jwt` for JWT token verification:
```ts
import { isExpired, decodeToken } from "react-jwt";

const isTokenValid = !isExpired(token);
const payload = decodeToken<TokenPayload>(token);
```

## React Query Server State

```ts
// Standard query with stale time
const useData = () => useQuery({
  queryKey: ["data"],
  queryFn: fetchData,
  staleTime: 5 * 60 * 1000, // 5 min
});

// Invalidate after mutation
const useUpdate = () => {
  const qc = useQueryClient();
  return useMutation({
    mutationFn: updateData,
    onSuccess: () => qc.invalidateQueries({ queryKey: ["data"] }),
  });
};
```
