---
trigger: model_decision
---

<!-- Adaptado para Antigravity -->
---
applyTo: "src/services/**, src/hooks/**, src/adapters/**"
---

# Services & Hooks

## HTTP Architecture

HTTP is handled via Axios through:
- `src/infrastructure/http/privateRequest.ts` — authenticated requests (includes auth headers, 401/403 handling)
- `src/infrastructure/http/publicRequest.ts` — unauthenticated requests
- `src/services/api/` — URL builders and endpoint definitions per domain
- TanStack Query (`useQuery`, `useMutation`) — for all data fetching in components

## Service Pattern

Services define typed URL builders — they do NOT make fetch calls directly:

```typescript
// src/services/api/accounts.api.ts
export const accountApi = {
  getAccount: (id: string) => `${envs.API.API_URL}/v1/accounts/${id}`,
  updateAccount: (id: string) => `${envs.API.API_URL}/v1/accounts/${id}`,
};
```

Always import `envs` from `src/utils/envs.ts` for URL construction.

## Custom Hook Pattern (MANDATORY for every service)

For every data-fetching operation, create a custom hook using TanStack Query with Axios:

```typescript
// src/hooks/useGetAccount.hook.ts
import { useQuery } from '@tanstack/react-query';
import { privateRequest } from 'src/infrastructure/http/privateRequest';
import { accountApi } from 'src/services/api/accounts.api';

interface IUseGetAccountParams {
  accountId: string;
}

const useGetAccount = ({ accountId }: IUseGetAccountParams) => {
  return useQuery({
    queryKey: ['account', accountId],
    queryFn: () => privateRequest({ url: accountApi.getAccount(accountId) }),
    enabled: !!accountId,
  });
};

export default useGetAccount;
```

## Mutation Pattern

```typescript
// src/hooks/useUpdateAccount.hook.ts
import { useMutation, useQueryClient } from '@tanstack/react-query';
import { privateRequest } from 'src/infrastructure/http/privateRequest';
import { accountApi } from 'src/services/api/accounts.api';

const useUpdateAccount = () => {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: (data: IUpdateAccountPayload) =>
      privateRequest({ url: accountApi.updateAccount(data.id), method: 'PUT', data }),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['account'] });
    },
  });
};
```

## Query Keys Convention

Use consistent query key arrays:
```typescript
['entity', id]          // single resource
['entity', 'list']      // collection
['entity', id, 'sub']   // nested resource
```

## Error Handling in Hooks

- Throw errors in `queryFn` — TanStack Query catches them
- Use `useErrorStore` for global error display
- Never swallow errors silently

## Adapters

Adapters (`src/adapters/`) transform API response shapes to UI-ready types:

```typescript
// src/adapters/AdapterAccount.ts
import type { IApiAccount } from 'src/interfaces/api/account';
import type { IAccount } from 'src/interfaces/account';

const AdapterAccount = (raw: IApiAccount): IAccount => ({
  id: raw.accountId,
  name: raw.displayName,
  balance: raw.currentBalance ?? 0,
});

export default AdapterAccount;
```

## Rules Summary

| Rule | Requirement |
|---|---|
| Data fetching | Always via `useQuery` / `useMutation` |
| URL construction | Always via `src/services/api/` + `envs.ts` |
| Every service | Must have a corresponding custom hook |
| Response shaping | Use adapters in `src/adapters/` |
| Auth tokens | Managed by TanStack Query config — do not manually attach |
