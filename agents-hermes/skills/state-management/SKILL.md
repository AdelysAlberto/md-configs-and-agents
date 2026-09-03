---
name: state-management
description: Zustand 5+ and global state management standards. Use when working on files matching: src/**/state/**, src/**/store/**, src/**/stores/**.
metadata:
  hermes:
    tags: [team-pinky, coding-standards]
    category: engineering
---

# State Management Standards (Zustand 5+)

## 1. Core Invariants

- **Zustand for UI/Client State Only**: Never use Zustand to cache server data (use TanStack Query or dedicated Result services).
- **Selector Hygiene**: Never destructure an entire store without selectors. Use atomic selectors or `useShallow`.
- **Pure Functional Stores**: No classes, no `this`.

---

## 2. Store Structure & Creation

```typescript
// ✅ GOOD: Feature slice inside src/modules/<FeatureName>/state/useFeatureStore.ts
import { create } from 'zustand';
import { useShallow } from 'zustand/react/shallow';

interface FeatureState {
  isOpen: boolean;
  activeId: string | null;
  setIsOpen: (isOpen: boolean) => void;
  setActiveId: (id: string | null) => void;
  reset: () => void;
}

const initialState = {
  isOpen: false,
  activeId: null,
};

export const useFeatureStore = create<FeatureState>((set) => ({
  ...initialState,
  setIsOpen: (isOpen) => set({ isOpen }),
  setActiveId: (activeId) => set({ activeId }),
  reset: () => set(initialState),
}));
```

---

## 3. Selector Usage

### ❌ BAD (Full Store Destructuring - Triggers Infinite/Unnecessary Re-renders)

```typescript
// ❌ BAD: Re-renders component whenever ANY field in the store changes
const { isOpen, setIsOpen } = useFeatureStore();
```

### ✅ GOOD (Atomic Selectors or useShallow)

```typescript
// ✅ GOOD: Atomic selector (preferred for single values)
const isOpen = useFeatureStore((state) => state.isOpen);
const setIsOpen = useFeatureStore((state) => state.setIsOpen);

// ✅ GOOD: useShallow for multiple subscribed properties
const { isOpen, activeId } = useFeatureStore(
  useShallow((state) => ({
    isOpen: state.isOpen,
    activeId: state.activeId,
  }))
);
```
