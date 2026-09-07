---
name: zustand
description: Advanced patterns and best practices for global and UI state management with Zustand 5+. Covers functional architecture, selector hygiene with useShallow, infinite render loop prevention, modular slices, secure storage isolation (sessionStorage / createAppStore), and out-of-React access.
---

# Zustand 5: Best Practices & Architecture

Comprehensive guide for managing global and UI state with Zustand 5 in a predictable, functional, and high-performance manner.

---

## 1. Fundamental Architecture Rules

1. **Server State vs UI State**:
   - ❌ **Don't use Zustand for server data**: Use TanStack Query for fetching, caching, synchronization, and invalidation.
   - ✅ **Use Zustand for UI/Client state**: Modals, wizards, active UI filters, collapsed sidebar, session preferences.
2. **Pure Functional Code**:
   - Classes and `this` are prohibited. All stores are created with pure functions and explicit TypeScript types.
3. **Naming & Structure**:
   - Name store hooks with the `use` prefix (e.g., `useAuthStore`, `useFilterStore`).
   - Always expose a `reset()` action to restore the initial state cleanly.

---

## 2. Store Creation & Secure Persistence

### A. In-Memory Store (Non-persistent)
Ideal for volatile state that should reset on page refresh.

```typescript
import { create } from 'zustand';

interface ModalState {
  isOpen: boolean;
  activeId: string | null;
  openModal: (id: string) => void;
  closeModal: () => void;
  reset: () => void;
}

const initialState = {
  isOpen: false,
  activeId: null,
};

export const useModalStore = create<ModalState>((set) => ({
  ...initialState,
  openModal: (id) => set({ isOpen: true, activeId: id }),
  closeModal: () => set({ isOpen: false, activeId: null }),
  reset: () => set(initialState),
}));
```

### B. Persistent Store (Session Isolation)
**SECURITY WARNING**: Avoid persisting session data or tokens directly in `localStorage` without isolation, as it can cause data leaks between users on the same browser. Prefer `sessionStorage` or the project's secure wrapper (`createAppStore`).

```typescript
import { create } from 'zustand';
import { persist, createJSONStorage } from 'zustand/middleware';

interface WizardState {
  currentStep: number;
  draftData: Record<string, unknown>;
  setStep: (step: number) => void;
  updateDraft: (data: Record<string, unknown>) => void;
  reset: () => void;
}

const initialState = {
  currentStep: 1,
  draftData: {},
};

export const useWizardStore = create<WizardState>()(
  persist(
    (set) => ({
      ...initialState,
      setStep: (step) => set({ currentStep: step }),
      updateDraft: (data) =>
        set((state) => ({ draftData: { ...state.draftData, ...data } })),
      reset: () => set(initialState),
    }),
    {
      name: 'app-wizard-session',
      storage: createJSONStorage(() => sessionStorage), // Per-tab/session isolation
      partialize: (state) => ({ currentStep: state.currentStep }), // Persist only what's needed
    }
  )
);
```

---

## 3. Safe Consumption & Re-render Prevention (`useShallow`)

### Critical Rule in Zustand 5:
- ❌ **DESTRUCTURING THE FULL STORE IS FORBIDDEN**: `const { prop1, prop2 } = useStore();` (Re-renders the component on **ANY** store change).
- ❌ **RETURNING LITERAL OBJECTS/ARRAYS WITHOUT MEMOIZATION IS FORBIDDEN**: Creates a new memory pointer on each evaluation and causes infinite loops (`Maximum update depth exceeded`).

```tsx
// ❌ BAD: Returns new reference every render -> Potential infinite loop
const { currentStep, draftData } = useWizardStore((state) => ({
  currentStep: state.currentStep,
  draftData: state.draftData,
}));
```

### ✅ Solution 1: Atomic Selectors (Recommended for 1 or 2 values)
```tsx
export const StepIndicator = () => {
  const currentStep = useWizardStore((state) => state.currentStep);
  const setStep = useWizardStore((state) => state.setStep);

  return <div>Current step: {currentStep}</div>;
};
```

### ✅ Solution 2: `useShallow` (Mandatory when selecting multiple properties on an object/array)
```tsx
import { useShallow } from 'zustand/react/shallow';

export const WizardHeader = () => {
  const { currentStep, reset } = useWizardStore(
    useShallow((state) => ({
      currentStep: state.currentStep,
      reset: state.reset,
    }))
  );

  return (
    <header>
      <h1>Step {currentStep}</h1>
      <button onClick={reset}>Restart</button>
    </header>
  );
};
```

---

## 4. Modular Slices Pattern (Complex Stores)

For large modules, split state into independent slices and combine them into a single root store.

```typescript
import { create, StateCreator } from 'zustand';

// Slices
interface UserSlice {
  user: { id: string; name: string } | null;
  setUser: (user: { id: string; name: string } | null) => void;
}

interface CartSlice {
  items: string[];
  addItem: (item: string) => void;
}

const createUserSlice: StateCreator<UserSlice & CartSlice, [], [], UserSlice> = (set) => ({
  user: null,
  setUser: (user) => set({ user }),
});

const createCartSlice: StateCreator<UserSlice & CartSlice, [], [], CartSlice> = (set) => ({
  items: [],
  addItem: (item) => set((state) => ({ items: [...state.items, item] })),
});

// Unified store
export const useRootStore = create<UserSlice & CartSlice>()((...args) => ({
  ...createUserSlice(...args),
  ...createCartSlice(...args),
}));
```

---

## 5. Async Actions & Result Pattern

Handle errors explicitly without throwing uncontrolled exceptions:

```typescript
interface ProductsState {
  items: Product[];
  isLoading: boolean;
  error: string | null;
  loadProducts: () => Promise<{ success: boolean; error?: string }>;
}

export const useProductsStore = create<ProductsState>((set) => ({
  items: [],
  isLoading: false,
  error: null,

  loadProducts: async () => {
    set({ isLoading: true, error: null });
    try {
      const response = await api.getProducts();
      if (!response.success) {
        set({ error: response.error, isLoading: false });
        return { success: false, error: response.error };
      }
      set({ items: response.value, isLoading: false });
      return { success: true };
    } catch (err) {
      const message = err instanceof Error ? err.message : 'Unexpected error';
      set({ error: message, isLoading: false });
      return { success: false, error: message };
    }
  },
}));
```

---

## 6. Access Outside React Components

Zustand allows reading or subscribing to state in services or network interceptors without hooks:

```typescript
// Read current state
const token = useAuthStore.getState().token;

// Dispatch action
useAuthStore.getState().logout();

// Subscribe to changes
const unsubscribe = useAuthStore.subscribe((state) => {
  console.log('Authentication state updated:', state.isAuthenticated);
});
```

---

## 7. Zustand Quality Checklist

- [ ] Does the store handle only client/UI state and not duplicate the server cache?
- [ ] Do all multi-property selectors use `useShallow`?
- [ ] Do stores expose a `reset()` action for cleanup?
- [ ] Does persistence use secure storage (`sessionStorage` or explicit partitioning)?
- [ ] Are action names clear, descriptive verbs?
