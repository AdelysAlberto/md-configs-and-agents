---
name: clean-architecture
description: Clean Architecture standards, Vertical Slicing, pure functional TypeScript, Result Pattern, and strict anti-pattern elimination. Use when structuring project modules, domain logic, or services.
license: MIT
compatibility: opencode
metadata:
  domain: architecture
  paradigm: functional
---

# Clean Architecture & Engineering Standards

Master guide for modular, scalable, and predictable software architecture based on **Pure Functional Programming**, **Vertical Slicing**, and the **Result Pattern**.

## 1. Core Engineering Invariants

1. **Pure Functional TypeScript**: Strictly prohibit `class` and `this`. Prefer composable pure functions and explicit closures.
2. **Zero `any` Policy**: Every type must be explicitly modeled or inferred with strict typing (`unknown` + type guards for untrusted input).
3. **Vertical Slicing (Feature Folders)**: Organize code by business domain in `src/modules/<FeatureName>/`, not by technical layer.
4. **Result Pattern**: Services, APIs, and domain actions must return `{ success: true, data } | { success: false, error }`. Never throw unhandled exceptions across architectural boundaries.
5. **Provider Adapter Pattern (DIP)**: High-level domain logic must depend only on agnostic provider interfaces, never on vendor-specific SDKs or concrete implementations.
6. **Single Source of Truth (SSOT)**: Zero hardcoded magic numbers, URLs, or environment strings. All configuration lives in central configs (`envs.ts` or design tokens).

---

## 2. Vertical Slicing Module Structure

```text
src/modules/<FeatureName>/
├── components/         # Feature-specific UI components
│   ├── FeatureWidget/
│   │   ├── FeatureWidget.tsx
│   │   └── FeatureWidget.module.css
│   └── index.ts
├── hooks/              # Orchestration and state hooks
│   ├── useFeatureData.ts
│   └── index.ts
├── services/           # External API & domain calls (Result Pattern)
│   ├── feature.service.ts
│   └── index.ts
├── types/              # Domain entities, DTOs, and contracts
│   ├── feature.types.ts
│   └── index.ts
└── index.ts            # Public API of the module
```

### Module Boundary Rules
- Modules must never access the private internals of another module.
- Inter-module communication happens strictly through the target module's root `index.ts`.
- Shared cross-cutting utilities reside in `src/shared/` or `src/infrastructure/`.

---

## 3. Result Pattern Implementation

```typescript
// Shared Result contract
export type Result<T, E = Error> =
  | { success: true; data: T }
  | { success: false; error: E };

export const Ok = <T>(data: T): Result<T, never> => ({ success: true, data });
export const Err = <E>(error: E): Result<never, E> => ({ success: false, error });

// Domain Service implementation
export const fetchUserProfile = async (
  userId: string
): Promise<Result<UserProfile, AppError>> => {
  try {
    const response = await httpClient.get<UserProfileDTO>(`/users/${userId}`);
    if (!response.ok) {
      return Err({ code: 'NOT_FOUND', message: 'User does not exist' });
    }
    const profile = mapToUserProfile(response.data);
    return Ok(profile);
  } catch (error) {
    return Err({
      code: 'NETWORK_FAILURE',
      message: error instanceof Error ? error.message : 'Unknown network failure',
    });
  }
};
```

---

## 4. Provider Adapter Pattern (DIP)

Never couple business modules to third-party SDKs directly:

```typescript
// ❌ BAD: Domain directly calling Stripe SDK
import Stripe from 'stripe';
export const charge = async (amount: number) => { ... };

// ✅ GOOD: Agnostic contract + isolated adapter
// src/providers/payment/payment.contract.ts
export interface PaymentProvider {
  processPayment(amount: number, currency: string): Promise<Result<PaymentConfirmation>>;
}

// src/providers/payment/providers/stripe.adapter.ts
export const createStripeAdapter = (apiKey: string): PaymentProvider => ({
  async processPayment(amount, currency) {
    // Isolated vendor logic
  }
});
```

---

## 5. Universal Anti-Pattern Elimination

- **Zero Unbounded Maps/Arrays**: Every in-memory cache must feature LRU eviction or explicit TTL to prevent memory leaks and OOM crashes.
- **Pub/Sub Loop Deduplication**: Event publishers must filter their own `senderId` to prevent self-echo and duplicated execution.
- **Scoped Subscriptions**: Wildcard subscriptions (`*`, `#`) on shared event buses are banned in tenant/client streams.
