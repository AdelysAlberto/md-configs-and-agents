---
trigger: model_decision
---

# Architecture

## Pattern: Vertical Slicing + Screaming Architecture
Organize by **feature/domain**, not by technical layer. The directory structure must communicate what the app does.

```
src/
├── components/      # Shared UI — Base components
├── hooks/           # Shared hooks
├── pages/           # Route-level screens (feature slices here)
│   └── private/
│       └── Wallet/
│           ├── components/   # Feature-specific UI
│           ├── hooks/        # Feature-specific hooks
│           └── index.tsx
├── services/        # HTTP service functions
├── store/           # Zustand global stores
├── adapters/        # API response → UI model transformers
├── util/            # Pure utility functions
├── Infra/           # HTTP client, auth infrastructure
└── config/          # Environment config, constants
```

## Service Layer Pattern (MANDATORY)
```
UI Component  ←→  Custom Hook (React Query + Adapter)  ←→  Service Layer
```

1. **UI Component** — pure presentation, no business logic, no direct API calls
2. **Custom Hook** — `useQuery`/`useMutation`, handles loading/error states, calls adapter
3. **Adapter/Mapper** — transforms API response shape → UI model (decouples backend changes)
4. **Service** — HTTP call only, returns `TResponseRequest<T>` — see [result-pattern-services.md](result-pattern-services.md)

```ts
// ✅ Correct layering
// service
const getUser = (): Promise<TResponseRequest<UserApiResponse>> =>
  privateRequest.get({ url: API.user });

// hook
const useUser = () => useQuery({
  queryKey: ["user"],
  queryFn: getUser,
  select: response => adaptUser(response.data),
});

// component
const Profile: FC = () => {
  const { data: user } = useUser();
  return <p>{user?.name}</p>;
};
```

## Component Design Rules
- Max **200 lines** per component — extract to hooks/utils if larger
- Components focus on UI only — no calculations, no business logic
- ❌ Avoid multiple `useEffect` in one component
- Extract stateful logic → `src/hooks/`
- Extract pure logic → `src/util/`

## useEffect Rules
- Always include ALL dependencies in the dependency array
- Always return cleanup functions to prevent memory leaks
- Never fetch data directly in `useEffect` — use React Query

## DRY: Reuse Utilities (MANDATORY)
Before writing any helper, search first:
```bash
grep -r "functionName" src/util/
```

| Need | File |
|---|---|
| Currency | `src/util/formatCurrency.js` |
| Dates | `src/util/formatDates.js` |
| Balance | `src/util/balance.util.js` |
| Email | `src/util/email.js` |
| Validation | `src/util/validation.js` |
| URL params | `src/util/urlParams.js`, `src/util/queryParams.js` |
| Avatar | `src/util/avatarUtils.js` |
| Logo | `src/util/logoUtils.js` |

If a utility is needed in 2+ files and doesn't exist → create in `src/util/`.

## HTTP Client
- **Private requests** (require token): `src/Infra/http/private.request` (or `src/Infra/http/providers/request.http`)
- **Environment config**: `src/config/config.ts` (not `utils/envs.ts`)
- **Token validation**: use `react-jwt`
