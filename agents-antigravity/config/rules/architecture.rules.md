---
description: "Vertical Slicing and Clean Architecture standards"
trigger: model_decision
applyTo: "src/**"
---

# Architecture & Vertical Slicing Standards

## 1. Core Rule: Vertical Slicing
All business domain features must be self-contained modules located inside `src/modules/<FeatureName>/`. Horizontal layering across the entire application root is strictly prohibited.

```text
src/
├── app/                  # Application bootstrap, routing and global providers
│   ├── App.tsx
│   ├── router.tsx
│   └── main.tsx
├── shared/               # Truly global utilities, design tokens, and shared primitives
│   ├── components/       # Generic UI primitives (Button, Input, Modal)
│   ├── utils/            # Pure helper functions
│   └── types/            # Global type declarations
└── modules/              # Vertical slices by feature
    └── <FeatureName>/    # Self-contained domain module
        ├── components/   # Feature-specific UI components
        ├── services/     # Feature-specific API calls & Result Pattern handlers
        ├── hooks/        # Feature-specific custom React hooks
        ├── state/        # Feature-specific Zustand slice / local state
        ├── types/        # Feature-specific models & contracts
        └── index.ts      # Public API boundary of the module
```

---

## 2. Module Encapsulation Contract
- Modules must expose their public interface strictly through `src/modules/<FeatureName>/index.ts`.
- Direct deep imports into another module's internal files (e.g. `import { x } from '@/modules/auth/services/internalHelper'`) are strictly forbidden.
- Modules may import from `src/shared/`, but `src/shared/` can **NEVER** import from `src/modules/`.

---

## 3. Naming Conventions

| Entity | Convention | Example |
| :--- | :--- | :--- |
| Components | PascalCase | `UserProfileCard.tsx` |
| Hooks | camelCase + `use` prefix | `useUserProfile.ts` |
| State Stores | camelCase + `use` prefix + `Store` | `useUserStore.ts` |
| Services | camelCase + `.service.ts` | `user.service.ts` |
| Types / Models | PascalCase | `User`, `UserProfileResponse` |
| CSS Modules | `[ComponentName].module.css` | `UserProfileCard.module.css` |

---

## 4. Anti-Patterns vs. Required Patterns

### ❌ BAD (Horizontal Layering / Leaky Boundaries & Vendor Coupling)
```typescript
// ❌ BAD: Storing all services in a root /src/services folder and deep importing
import { getUser } from '../../../services/userService';

// ❌ BAD: Domain service coupled directly to vendor-specific provider
import { calculateValhallaRoute, type ValhallaTrip } from '@/providers/valhalla';
```

### ✅ GOOD (Vertical Slicing & Agnostic Provider Adapter)
```typescript
// ✅ GOOD: Importing from the module's public boundary
import { UserProfileCard, useUserStore } from '@/modules/user';

// ✅ GOOD: Domain service importing agnostic provider adapter port
import { calculateRouteEngine, type RoutingTrip } from '@/providers/routing';
```

---

## 5. Provider Isolation Architecture & Single Point of Change (DIP Invariant)

Every external solution, third-party service, or vendor integration (LLM, STT, Geocoder, Routing, Payment, Email, S3 Storage, Redis, DB) **MUST** be isolated inside `src/providers/<domain>/`.

### Required Directory Structure

```text
src/providers/<domain>/
├── interfaces/
│   └── <domain>.interface.ts       # Domain-agnostic contract & DTOs (e.g. GeocoderSearchResult)
├── adapters/
│   ├── <vendor_a>/                 # Vendor A concrete implementation (e.g. photon/)
│   └── <vendor_b>/                 # Vendor B concrete implementation (e.g. nominatim/)
├── services/
│   └── <domain>.service.ts         # Unified service entry point exposing the active adapter
└── index.ts                        # Public API export boundary
```

### Invariants:
1. **Single Point of Change**: Changing an underlying vendor (e.g. swapping `Photon` for `Nominatim`, or `OMLX` for `DeepSeek`) **MUST be done in exactly 1 single file** (`src/providers/<domain>/services/<domain>.service.ts` or adapter registry).
2. **Zero Vendor Leakage**: Domain code in `src/modules/` is strictly forbidden from importing vendor-specific functions or vendor DTO types. They consume **only** domain-agnostic interfaces exported by `@/providers/<domain>`.
