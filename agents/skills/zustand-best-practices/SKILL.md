---
name: zustand-best-practices
description: "Guidelines and strict rules for creating and consuming Zustand stores in the project, including the custom createAppStore wrapper and the usage of useShallow to prevent performance issues."
---

# Zustand Best Practices & Rules

This project enforces strict rules for managing global state with Zustand to prevent memory leaks, "stale data" bugs across sessions, and infinite rendering loops.

## 1. Creating Stores (`createAppStore`)

**CRITICAL RULE:** Do **NOT** use Zustand's native `create` or `persist` directly when defining a store. Native `persist` defaults to `localStorage`, causing data to leak between user sessions and break UI state upon re-login.

Always use the project's custom wrapper: `createAppStore`.

### Non-persisted Store (Memory Only)
For temporary state that should clear on page refresh (F5).
```typescript
import { createAppStore } from '@/utils/store/createAppStore';

interface MyState {
  count: number;
  increment: () => void;
}

export const useMyStore = createAppStore<MyState>()((set, get) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}));
```

### Persisted Store (Session Storage)
For state that must survive page reloads (e.g., active tabs, wizard steps, unsaved forms). The wrapper automatically enforces `sessionStorage`.
```typescript
import { createAppStore } from '@/utils/store/createAppStore';

interface MyPersistedState {
  step: number;
  setStep: (step: number) => void;
}

export const useMyPersistedStore = createAppStore<MyPersistedState>()(
  (set) => ({
    step: 1,
    setStep: (step) => set({ step }),
  }),
  {
    persist: true,
    name: 'my-unique-wizard-storage', // REQUIRED when persist is true
    // persistOptions: { partialize: (state) => ({ step: state.step }) } // Optional
  }
);
```

## 2. Consuming Stores (`useShallow`)

**CRITICAL RULE:** When extracting multiple properties from a store or returning a newly created object/array in the selector, you **MUST** wrap the selector with `useShallow` from `zustand/react/shallow`.

Failing to use `useShallow` when returning objects will cause React to detect a new reference on every store update (even unrelated ones), triggering infinite render loops or severe performance degradation.

### ✅ Correct Usage (Multiple Properties)
```tsx
import { useShallow } from 'zustand/react/shallow';
import { useMyStore } from './myStore';

export const MyComponent = () => {
  // CORRECT: useShallow prevents re-renders if the values inside the object haven't changed
  const { step, setStep } = useMyStore(
    useShallow((state) => ({
      step: state.step,
      setStep: state.setStep,
    }))
  );

  return <div>{step}</div>;
};
```

### ❌ Incorrect Usage (Do not do this)
```tsx
// WRONG: Returns a new object reference every time, causing infinite renders
const { step, setStep } = useMyStore((state) => ({ step: state.step, setStep: state.setStep }));

// WRONG: Using an array also creates a new reference
const [step, setStep] = useMyStore((state) => [state.step, state.setStep]);
```

### Alternative: Atomic Selectors (No `useShallow` needed)
If you only need a single primitive value or function, you can select it directly without `useShallow`.
```tsx
const step = useMyStore((state) => state.step);
const setStep = useMyStore((state) => state.setStep);
```
