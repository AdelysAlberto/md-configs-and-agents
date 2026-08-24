---
trigger: model_decision
description: 'API Services, Result Pattern, and Error Handling standards'
applyTo: 'src/**/services/**, src/**/hooks/**'
---

# Services & Result Pattern Standards

## 1. Core Invariants
- **Never Throw Exceptions**: Service functions must never throw unhandled errors. Always return a typed `Result<T, E>`.
- **Vertical Placement**: Services belong to their feature slice: `src/modules/<FeatureName>/services/`.
- **Pure Functional**: Functions must be standalone, exportable, and free of `class` or `this`.

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
