---
name: architecture
description: Vertical Slicing and Clean Architecture standards. Use when working on files matching: src/**.
metadata:
  hermes:
    tags: [team-pinky, coding-standards]
    category: engineering
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
