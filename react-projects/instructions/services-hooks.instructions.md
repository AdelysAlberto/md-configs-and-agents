---
applyTo: "src/services/**, src/hooks/**, src/adapters/**, src/providers/http/**, src/pages/**/use*.ts"
---

# Services, Hooks & HTTP Client — r365-backoffice

## Flujo obligatorio

```
UI Component → Custom Hook (TanStack Query) → Adapter (opcional) → Service → httpRequest
```

## HTTP Client

Usa `httpRequest` de `@/providers/http/request.http` (interfaz `IRequest`).

```typescript
// Tipo de respuesta
type TResponseRequest<T> = { data?: T; status?: number; message?: string };
```

- `fetchInstance` lanza excepciones en errores HTTP → TanStack Query las captura
- Token JWT: `useAuthStore.getState().accessToken` (Zustand → sessionStorage)

## APIs disponibles (`src/constants/config.ts`)

| Variable | Uso |
|---|---|
| `config.apiBaseUrl` | Auth, users, países, KYC, compliance, blacklist, audit, push |
| `config.walletApiUrl` | Transacciones, wallets, transferencias, comisiones, tipos de pago, exchange rates |
| `config.onboardingApiUrl` | Onboarding, niveles |

## Patrón de Services

```typescript
import { config } from "@/constants/config";
import type { TResponseRequest } from "@/interfaces/http.interface";
import { buildQueryUrl } from "@/providers/http/request.instance";
import httpRequest from "@/providers/http/request.http";

// GET sin params
export const getItemService = async (id: string): Promise<TResponseRequest<Item>> => {
  return httpRequest.get<Item>({ url: `${config.apiBaseUrl}/bo/items/${encodeURIComponent(id)}` });
};

// GET con query params → SIEMPRE usar buildQueryUrl
export const getItemsService = async (filters: ItemFilters = {}): Promise<TResponseRequest<ItemListResult>> => {
  const raw = await httpRequest.get<ItemApiResponse>({
    url: buildQueryUrl(`${config.apiBaseUrl}/bo/items`, filters as Record<string, unknown>),
  });
  return { ...raw, data: raw.data ? toItemList(raw.data) : undefined };
};

// POST
export const createItemService = async (body: CreateItemDto): Promise<TResponseRequest<Item>> => {
  return httpRequest.post<Item>({ url: `${config.apiBaseUrl}/bo/items`, body });
};

// PATCH — body obligatorio (pasar {} si no hay datos)
export const toggleItemService = async (id: string): Promise<TResponseRequest<void>> => {
  return httpRequest.patch<void>({ url: `${config.apiBaseUrl}/bo/items/${encodeURIComponent(id)}/toggle`, body: {} });
};

// DELETE — body obligatorio (pasar {})
export const deleteItemService = async (id: string): Promise<TResponseRequest<void>> => {
  return httpRequest.delete<void>({ url: `${config.apiBaseUrl}/bo/items/${encodeURIComponent(id)}`, body: {} });
};

// Endpoint público (sin JWT)
export const loginService = async (body: LoginDto): Promise<TResponseRequest<LoginTokens>> => {
  return httpRequest.post<LoginTokens>({ url: `${config.apiBaseUrl}/auth/login`, body, isPublic: true });
};
```

## Patrón de Hooks (TanStack Query)

```typescript
const useItems = (filters: ItemFilters) => {
  return useQuery({
    queryKey: ['items', filters],
    queryFn: async () => {
      const { data } = await getItemsService(filters);
      if (data === undefined) throw new Error('No data');
      return data;
    },
    staleTime: 30_000,
  });
};
```

## Adapters

Transforman respuesta API → tipo UI. Protegen la UI de cambios de backend.
Ubicación: `src/adapters/`. Se usan dentro del service o del hook.

## Reglas críticas

- ✅ `encodeURIComponent` en TODOS los path params
- ✅ DELETE y PATCH sin body deben pasar `body: {}`
- ✅ `fetchInstance` ya lanza en errores HTTP; NO envolver services en try/catch
- ✅ En `queryFn`, hacer `throw` si `data === undefined`
- ✅ Endpoints públicos: pasar `isPublic: true`
- ❌ No usar el patrón viejo `coreApi` / `Result<T>` — **eliminado**
- ❌ No hacer `throw` directo en services — `fetchInstance` ya lo hace
- ❌ Nunca catch silencioso
