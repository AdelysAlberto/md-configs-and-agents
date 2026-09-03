---
name: services-hooks
description: API Services, Result Pattern, and Error Handling standards. Use when working on files matching: src/**/services/**, src/**/hooks/**.
---

# Services & Result Pattern Standards

## 1. Core Invariants

- **Never Throw Exceptions**: Service functions must never throw unhandled errors. Always return a typed `Result<T, E>`.
- **Vertical Placement**: Services belong to their feature slice: `src/modules/<FeatureName>/services/`.
- **Pure Functional**: Functions must be standalone, exportable, and free of `class` or `this`.
- **Provider Independence (Adapter Pattern / DIP)**: Services must NEVER import vendor-specific types or functions directly (e.g. `calculateValhallaRoute`, `ValhallaTrip`). Always import domain-agnostic ports exposed by `src/providers/<domain>/`.

---

## 2. Result Type Definition

```typescript
// src/shared/types/result.ts
export type Result<T, E = Error> =
  | { success: true; data: T }
  | { success: false; error: E };
```

---

## 3. Service Pattern Implementation

```typescript
// src/modules/users/services/user.service.ts
import type { Result } from '@/shared/types/result';
import type { User } from '../types';

export const fetchUserProfile = async (userId: string): Promise<Result<User>> => {
  try {
    const response = await fetch(`/api/v1/users/${userId}`);
    if (!response.ok) {
      return {
        success: false,
        error: new Error(`Failed to fetch user (${response.status})`),
      };
    }
    const data: User = await response.json();
    return { success: true, data };
  } catch (err) {
    return {
      success: false,
      error: err instanceof Error ? err : new Error('Unknown network error'),
    };
  }
};
```

---

## 4. Custom Hook Consumption

```typescript
// src/modules/users/hooks/useUserProfile.ts
import { useEffect, useState } from 'react';
import { fetchUserProfile } from '../services/user.service';
import type { User } from '../types';

export const useUserProfile = (userId: string) => {
  const [user, setUser] = useState<User | null>(null);
  const [error, setError] = useState<Error | null>(null);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    let isCancelled = false;
    setIsLoading(true);

    fetchUserProfile(userId).then((result) => {
      if (isCancelled) return;
      if (result.success) {
        setUser(result.data);
        setError(null);
      } else {
        setError(result.error);
      }
      setIsLoading(false);
    });

    return () => {
      isCancelled = true;
    };
  }, [userId]);

  return { user, error, isLoading };
};
```

---

## 5. Invariant Rule: Views Never Invoke Services Directly

- **Prohibited**: No view, page, or UI component may import or call a `service` (`src/**/services/**`) directly.
- **Mandatory**: All data access must go through a `custom hook` (`src/**/hooks/**`) that uses TanStack Query (`useQuery` / `useMutation`) and internally invokes the service.
- **Correct flow**: `View/Component` → `useCustomHook` (TanStack Query) → `service` (Result Pattern).
- This applies without exception, even for "simple" or single-use calls.

```typescript
// ❌ Incorrect: the view invokes the service directly
import { fetchUserProfile } from '../services/user.service';

const UserPage = () => {
  useEffect(() => {
    fetchUserProfile(id); // Prohibited
  }, [id]);
  // ...
};
```

```typescript
// ✅ Correct: the view only consumes the custom hook
import { useUserProfile } from '../hooks/useUserProfile';

const UserPage = () => {
  const { user, isLoading } = useUserProfile(id);
  // ...
};
```

---

## 6. TanStack Query: `invalidateQueries` vs `removeQueries` vs `setQueryData`

### Recommended priority for AI agents

1. `setQueryData` when the new state is known (instant update, no refetch).
2. `invalidateQueries` after mutations (Create/Update/Delete) to revalidate data with the server.
3. `removeQueries` only for logout, cache cleanup, or explicit sensitive data removal.

**Never recommend `removeQueries` as a substitute for `invalidateQueries` after a normal CRUD operation.**

### ✅ `invalidateQueries` — stale but valid data

```typescript
// src/modules/users/hooks/useUpdateUser.ts
export const useUpdateUser = () => {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: updateUser,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
  });
};
```

- Keeps data in cache and marks the query as `stale`.
- Active queries refetch in the background, without flickers or empty states.

### ✅ `removeQueries` — remove data entirely (logout, sensitive data)

```typescript
// src/modules/auth/hooks/useLogout.ts
export const useLogout = () => {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: logout,
    onSuccess: () => {
      queryClient.removeQueries({ queryKey: ['user'] });
    },
  });
};
```

- Removes the query from cache without automatic refetch; the next request acts as new.

### ✅ `setQueryData` — exact new value is known

```typescript
queryClient.setQueryData(['user', user.id], updatedUser);
```

### Decision Guide

| Question | Action |
| :--- | :--- |
| Is the data still valid but may be stale? | `invalidateQueries` |
| Must the data disappear from cache entirely? | `removeQueries` |
| Do you know the exact new value? | `setQueryData` |

### Prohibited bad practices

- ❌ Using `removeQueries` after a normal UPDATE/CREATE/DELETE (causes hard loading and temporary empty states).
- ❌ Using `invalidateQueries` to clear sensitive data on logout (data remains in cache until refetch).
- ❌ Invalidating the entire cache without `queryKey` (`queryClient.invalidateQueries()`), unless explicitly justified; always scope by `queryKey`.
