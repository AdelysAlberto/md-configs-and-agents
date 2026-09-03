<!-- Adapted for Opencode -->
---
applyTo: "src/services/**"
---
# Services & Hooks (Result Pattern)

## API Service Guidelines

All API services must follow the **Result Pattern** (`{ ok, data, error }`). Never throw exceptions for expected API flow.

### Service Function Template

```typescript
import { privateRequest } from 'src/infrastructure/http/privateRequest';
import { z } from 'zod';

const getUserSchema = z.object({ id: z.string(), name: z.string() });

export interface User {
  id: string;
  name: string;
}

export type ApiResult<T> = {
  ok: true; data: T;
} | {
  ok: false; error: string;
};

/** Fetches a user by ID using the Result Pattern */
export async function getUser(id: string): Promise<ApiResult<User>> {
  try {
    const response = await privateRequest(`/api/users/${id}`);
    const data = getUserSchema.parse(response.data);
    return { ok: true, data };
  } catch (error: any) {
    const message = error.response?.data?.message || error.message || 'Failed to fetch user';
    return { ok: false, error: message };
  }
}
```

### Hook Pattern

Custom hooks must also return results using the Result Pattern:

```typescript
export function useGetUser(id: string) {
  const [result, setResult] = useState<ApiResult<User> | null>(null);

  useEffect(() => {
    getUser(id).then(setResult);
  }, [id]);

  return result;
}
```

## Services Directory Structure

- `src/services/api/` — Endpoint definitions per domain, each file exports Result Pattern functions.
- `src/services/http/` — HTTP client instances (privateRequest, publicRequest) with interceptor configuration.
- `src/services/schemas/` — Zod schemas for request/response validation.