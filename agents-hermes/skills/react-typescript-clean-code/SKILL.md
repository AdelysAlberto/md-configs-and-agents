---
name: react-typescript-clean-code
description: Master guide and engineering standards for modern React (18/19+) and TypeScript. Emphasizes simplicity over complexity, pure functional code, judicious use of hooks (useEffect, useMemo, useCallback), real performance patterns (virtualization, code-splitting), Clean Architecture, and anti-pattern prevention.
---

# React & TypeScript: Clean Architecture, Simplicity & Performance

Engineering guide for frontend application development in React with TypeScript. Prioritizes **radical simplicity**, functional predictability, and measurable performance over over-engineering and premature optimization.

---

## 🧭 1. Guiding Principle: Simplicity over Complexity

> *"The fastest code with the least bugs is the one that isn't written or the one kept simple."*

1. **Avoid premature optimization**: Do not wrap every variable or function in `useMemo`/`useCallback` by default. The memory cost and cognitive overhead usually outweigh the presumed benefit.
2. **Prioritize pure functions**: Extract calculation logic out of React components into pure TypeScript functions that are easy to test.
3. **Small components with single responsibility**: Maximum ~200 lines per component. If it grows, extract logic into custom hooks or presentational subcomponents.
4. **No `class` and `this`**: 100% functional code with strict TypeScript.

---

## 🔍 2. Diagnosis and Judicious Use of Hooks

### A. `useEffect`: When to Use and When to AVOID?

`useEffect` is an escape hatch for synchronizing with **external systems**. It is **NOT** a mechanism for handling user events or synchronizing internal state.

| Use Case | Use `useEffect`? | Correct Alternative (Simplicity) |
| :--- | :---: | :--- |
| **Compute derived state** | ❌ **NEVER** | Compute directly in the component body during render. |
| **Reset state when a prop changes** | ❌ **AVOID** | Use a unique `key` on the component to force React's natural reset. |
| **Handle user actions (clicks, submit)** | ❌ **NEVER** | Execute the logic inside the event handler (`onClick`, `onSubmit`). |
| **Manual Data Fetching** | ❌ **AVOID** | Use **TanStack Query** (handles cache, deduplication, abort, and retries). |
| **Subscription to external events (global DOM, WebSockets, Canvas, Timers)** | ✅ **YES (With Cleanup)** | External synchronization with mandatory cleanup function in the `return`. |

#### ❌ Anti-pattern: Derived state with `useEffect` (Double Render + Lag)
```tsx
// BAD: Causes an extra render with inconsistent state
const ProductSummary = ({ price, discount }: Props) => {
  const [total, setTotal] = useState(0);

  useEffect(() => {
    setTotal(price - discount);
  }, [price, discount]);

  return <div>Total: ${total}</div>;
};
```

#### ✅ Clean Solution: Calculation in Render
```tsx
// GOOD: No extra hooks, synchronous and bug-free
const ProductSummary = ({ price, discount }: Props) => {
  const total = price - discount;
  return <div>Total: ${total}</div>;
};
```

#### ✅ Legitimate Use: External Synchronization with Cleanup
```tsx
export const useWindowWidth = () => {
  const [width, setWidth] = useState(() => window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', handleResize);

    // MANDATORY: Cleanup to avoid memory leaks
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return width;
};
```

---

### B. `useMemo`: When is it Really Necessary?

`useMemo` caches the result of a computation between re-renders.

| Scenario | Use `useMemo`? | Technical Reason |
| :--- | :---: | :--- |
| Simple operations (`a + b`, `.length`, `toLowerCase()`) | ❌ **NO** | The cost of creating closures and checking dependencies is greater than the computation. |
| Small arrays (< 500 elements) | ❌ **NO** | Modern CPUs process thousands of operations in less than 0.1ms. |
| Heavy filtering/sorting (> 1,000 elements or O(n²) transformations) | ✅ **YES** | Avoids freezing the main thread on recurrent renders. |
| Preserving referential equality of objects passed to `React.memo` or hook dependencies | ✅ **YES** | Avoids invalidating memoization of optimized child components. |

#### ❌ Anti-pattern: Unnecessary trivial memoization
```tsx
// BAD: Over-engineering for an instantaneous operation
const fullName = useMemo(() => `${firstName} ${lastName}`, [firstName, lastName]);
const itemsCount = useMemo(() => items.length, [items]);
```

#### ✅ Solution: Simple and direct
```tsx
// GOOD: Expressive, no extra memory or overhead
const fullName = `${firstName} ${lastName}`;
const itemsCount = items.length;
```

#### ✅ Legitimate Use: Large-volume filtering
```tsx
// GOOD: Justified by data volume
const visibleItems = useMemo(() => {
  return largeDataset
    .filter((item) => item.status === activeFilter)
    .sort((a, b) => b.timestamp - a.timestamp);
}, [largeDataset, activeFilter]);
```

---

### C. `useCallback`: When to Use and When to Omit?

`useCallback` preserves the **reference of a function** between renders.

| Scenario | Use `useCallback`? | Technical Reason |
| :--- | :---: | :--- |
| Callbacks passed to native HTML elements (`<button onClick={...}>`) | ❌ **NO** | HTML elements are always re-evaluated in the virtual DOM; brings no benefit. |
| Callbacks passed to normal child components (not wrapped in `React.memo`) | ❌ **NO** | The child will re-render anyway when the parent changes. |
| Callbacks passed to child components optimized with `React.memo` | ✅ **YES** | Prevents the child's `React.memo` from invalidating on each parent render. |
| Functions that are part of a `useEffect` or custom hook dependency array | ✅ **YES** | Prevents the effect from running on each render cycle. |

#### ❌ Anti-pattern: `useCallback` in components without memoization
```tsx
// BAD: Unnecessary memory expense and noisy syntax
export const UserList = () => {
  const handleClick = useCallback((id: string) => {
    console.log('Clicked', id);
  }, []);

  return (
    <div>
      {/* NativeButton is not memoized with React.memo */}
      <NativeButton onClick={handleClick} />
    </div>
  );
};
```

#### ✅ Solution: Standard direct function
```tsx
// GOOD: Clean and direct code
export const UserList = () => {
  const handleClick = (id: string) => {
    console.log('Clicked', id);
  };

  return <NativeButton onClick={handleClick} />;
};
```

#### ✅ Legitimate Use: Callback to memoized child
```tsx
// The child is explicitly memoized
const MemoizedRow = React.memo(({ item, onDelete }: RowProps) => {
  return (
    <div>
      <span>{item.name}</span>
      <button onClick={() => onDelete(item.id)}>Delete</button>
    </div>
  );
});

export const DataTable = ({ items }: TableProps) => {
  // GOOD: Keeps the reference stable so it doesn't invalidate MemoizedRow's memo
  const handleDelete = useCallback((id: string) => {
    api.deleteItem(id);
  }, []);

  return (
    <div>
      {items.map((item) => (
        <MemoizedRow key={item.id} item={item} onDelete={handleDelete} />
      ))}
    </div>
  );
};
```

---

## ⚡ 3. High-Impact Performance Techniques

### A. Virtualization of Long Lists
When rendering lists with more than 50-100 elements, virtualization is 10x more effective than any micro-optimization with `useMemo`.

- **Web**: Use `@tanstack/react-virtual`.
- **React Native**: Use `@shopify/flash-list`.

```tsx
import { useVirtualizer } from '@tanstack/react-virtual';
import { useRef } from 'react';

export const VirtualList = ({ items }: { items: Item[] }) => {
  const parentRef = useRef<HTMLDivElement>(null);

  const rowVirtualizer = useVirtualizer({
    count: items.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 48,
    overscan: 5,
  });

  return (
    <div ref={parentRef} style={{ height: '400px', overflow: 'auto' }}>
      <div style={{ height: `${rowVirtualizer.getTotalSize()}px`, position: 'relative' }}>
        {rowVirtualizer.getVirtualItems().map((virtualRow) => (
          <div
            key={virtualRow.key}
            style={{
              position: 'absolute',
              top: 0,
              left: 0,
              width: '100%',
              height: `${virtualRow.size}px`,
              transform: `translateY(${virtualRow.start}px)`,
            }}
          >
            {items[virtualRow.index].name}
          </div>
        ))}
      </div>
    </div>
  );
};
```

### B. Code-Splitting by Routes with `React.lazy`
Split the main bundle loading routes and heavy components on demand.

```tsx
import { lazy, Suspense } from 'react';
import { Routes, Route } from 'react-router-dom';

const DashboardModule = lazy(() => import('./modules/Dashboard'));
const SettingsModule = lazy(() => import('./modules/Settings'));

export const AppRoutes = () => (
  <Suspense fallback={<div className="loading-spinner">Loading...</div>}>
    <Routes>
      <Route path="/dashboard" element={<DashboardModule />} />
      <Route path="/settings" element={<SettingsModule />} />
    </Routes>
  </Suspense>
);
```

---

## 🛡️ 4. Strict TypeScript & Clean Typing

1. **Zero `any`**: Use `unknown`, generics `T`, or specific interfaces.
2. **Discriminated Unions instead of Enums**:
   ```typescript
   // ❌ BAD: Numeric or heterogeneous Enums
   enum Status { Active, Inactive }

   // ✅ GOOD: Discriminated literal types (type-safe and tree-shakeable)
   type Status = 'active' | 'inactive' | 'pending';
   ```
3. **Props with descriptive interfaces**:
   ```typescript
   interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
     variant?: 'primary' | 'secondary' | 'danger';
     isLoading?: boolean;
   }
   ```
4. **Result Pattern in Services**:
   ```typescript
   type Result<T, E = string> = 
     | { success: true; value: T }
     | { success: false; error: E };
   ```

---

## 📋 5. Verification & Quality Checklist

Before marking any component or module as complete:

- [ ] **Simplicity**: Was derived state computed during render instead of using a `useEffect`?
- [ ] **Justified hooks**: Do all `useMemo` and `useCallback` have a real technical justification (children with `React.memo` or heavy datasets)?
- [ ] **Clean effects**: Does every `useEffect` that subscribes to external events have its respective cleanup/return function?
- [ ] **TypeScript**: Zero `any` and strict types in all props and function signatures?
- [ ] **Component size**: Are components kept under ~200 lines and decoupled from heavy logic?
- [ ] **Zero render leaks**: Are global state selectors protected with `useShallow` or atomic selectors?
