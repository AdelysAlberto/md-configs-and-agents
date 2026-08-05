---
trigger: model_decision
---

# Services, Hooks & Adapters

## Result Pattern (MANDATORY for all services)

Services MUST return `TResponseRequest<T>` — no `throw`, no `try/catch` in service layer.

```ts
// src/services/wallet/accounts.ts
import type { TResponseRequest } from "@/Infra/http/interfaces/http.interface";
import privateRequest from "@/Infra/http/providers/request.http";

export const getAccountPagoMovil = (): Promise<TResponseRequest<AccountResponse>> =>
  privateRequest.get({ url: `${Config.apiWallet}/accounts/pago-movil` });
```

```ts
// Type definition
export type TResponseRequest<T> = {
  data?: T;        // present on success
  status?: number; // HTTP status code
  message?: string; // present on error
};
```

❌ **Never** in services:
```ts
// Wrong
const getAccount = async () => {
  try {
    const res = await fetch(url);
    if (!res.ok) throw new Error("Failed"); // ❌ throw
    return res.json();
  } catch (e) {
    console.error(e); // ❌ console.log/error
    throw e; // ❌ re-throw
  }
};
```

## React Query Hooks

### 3-generic pattern for Axios responses
```ts
// ✅ Correct — prevents type mismatch
const useGetAccounts = () =>
  useQuery<ApiResponse<Account[]>, Error, Account[]>({
    queryKey: ["accounts"],
    queryFn: getAccountPagoMovil as () => Promise<ApiResponse<Account[]>>,
    staleTime: 5 * 60 * 1000, // 5 minutes
    select: response => response?.data?.data ?? [],
  });
```

`select` transforms data **inside React Query** — eliminates `useState + useEffect` chain and extra render cycle.

### useMutation pattern
```ts
const useUpdateAccount = () =>
  useMutation({
    mutationFn: (payload: UpdateAccountPayload) => updateAccount(payload),
    onSuccess: () => queryClient.invalidateQueries({ queryKey: ["accounts"] }),
  });
```

## Adapters/Mappers

Transform API shape → UI model. Keeps components isolated from backend changes.

```ts
// src/adapters/bankListAdapter.ts
export interface BankAccount {
  id: number;
  phone: string;
  bankCode: number;
  bankName: string;
}

export const adapterBankWithAccount = (accounts: PagoMovilAccount[]): SelectOption[] =>
  accounts.map(acc => ({
    value: acc.ID,
    label: `${getBankName(acc.bank_code)} — ${acc.phone_number}`,
  }));
```

## Hook Composition

Expose derived data from hooks via `useMemo`/`useCallback`:

```ts
const useGetAccountPagoMovil = () => {
  const { data, ...queryState } = useQuery<ApiResponse, Error, Account[]>({
    queryKey: ["userMobileAccount"],
    queryFn: getAccountPagoMovil as () => Promise<ApiResponse>,
    staleTime: 5 * 60 * 1000,
    select: response => response?.data?.data ?? [],
  });

  const accounts = data ?? [];
  const options = useMemo(() => adapterBankWithAccount(accounts), [accounts]);
  const getAccountById = useCallback(
    (id: number) => accounts.find(a => a.ID === id),
    [accounts],
  );

  return { data: accounts, options, getAccountById, ...queryState };
};
```

## Consulting API Types

Before creating a service or hook, check `.github/api-typescript-types.md` for:
- Request/response types for all endpoints
- Query parameters and body types
- Error response structures
