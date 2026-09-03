---
name: services-hooks
description: API Services, Result Pattern, and Error Handling standards. Use when working on files matching: src/**/services/**, src/**/hooks/**.
metadata:
  hermes:
    tags: [team-pinky, coding-standards]
    category: engineering
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

## 5. Regla Invariante: Views Nunca Invocan Services Directamente

- **Prohibido**: Ninguna vista, página o componente de UI puede importar o llamar un `service` (`src/**/services/**`) directamente.
- **Obligatorio**: Todo acceso a datos pasa por un `custom hook` (`src/**/hooks/**`) que use TanStack Query (`useQuery` / `useMutation`) y que internamente invoque el service.
- **Flujo correcto**: `View/Component` → `useCustomHook` (TanStack Query) → `service` (Result Pattern).
- Esto aplica sin excepción, incluso para llamadas "simples" o de un solo uso.

```typescript
// ❌ Incorrecto: la vista invoca el service directamente
import { fetchUserProfile } from '../services/user.service';

const UserPage = () => {
  useEffect(() => {
    fetchUserProfile(id); // Prohibido
  }, [id]);
  // ...
};
```

```typescript
// ✅ Correcto: la vista solo consume el custom hook
import { useUserProfile } from '../hooks/useUserProfile';

const UserPage = () => {
  const { user, isLoading } = useUserProfile(id);
  // ...
};
```

---

## 6. TanStack Query: `invalidateQueries` vs `removeQueries` vs `setQueryData`

### Prioridad recomendada para agentes IA

1. `setQueryData` cuando se conoce el nuevo estado (actualización instantánea, sin refetch).
2. `invalidateQueries` después de mutaciones (Create/Update/Delete) para revalidar datos con el servidor.
3. `removeQueries` únicamente para logout, limpieza de caché o eliminación explícita de datos sensibles.

**Nunca recomendar `removeQueries` como sustituto de `invalidateQueries` tras una operación CRUD normal.**

### ✅ `invalidateQueries` — datos desactualizados pero válidos

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

- Mantiene los datos en caché y marca la query como `stale`.
- Las queries activas hacen refetch en segundo plano, sin parpadeos ni estados vacíos.

### ✅ `removeQueries` — eliminar datos por completo (logout, datos sensibles)

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

- Elimina la query del caché sin refetch automático; la próxima solicitud actúa como nueva.

### ✅ `setQueryData` — se conoce el nuevo valor exacto

```typescript
queryClient.setQueryData(['user', user.id], updatedUser);
```

### Guía de decisión

| Pregunta | Acción |
| :--- | :--- |
| ¿Los datos siguen siendo válidos pero pueden estar desactualizados? | `invalidateQueries` |
| ¿Los datos deben desaparecer completamente del caché? | `removeQueries` |
| ¿Conoces exactamente el nuevo valor? | `setQueryData` |

### Malas prácticas prohibidas

- ❌ Usar `removeQueries` después de un UPDATE/CREATE/DELETE normal (provoca hard loading y estados vacíos temporales).
- ❌ Usar `invalidateQueries` para limpiar datos sensibles en logout (los datos siguen en caché hasta el refetch).
- ❌ Invalidar todo el caché sin `queryKey` (`queryClient.invalidateQueries()`), salvo necesidad explícita justificada; siempre acotar por `queryKey`.
