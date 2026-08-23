<!-- Adaptado para Opencode -->
---
applyTo: "src/**/*.{ts,tsx}"
---
# React Guidelines

## React Best Practices

- **React 18+ Features**: Use automatic batching, useTransition, and useDeferredValue where appropriate.
- **Custom Hooks**: Extract component logic into custom hooks (`useFetch`, `useForm`, etc.). Keep components thin.
- **Error Boundaries**: Implement Error Boundaries at top-level routes for unhandled errors.
- **Ref Forwarding**: Use `React.forwardRef` only for composite components that need ref access.
- **Memoization**: Use `React.memo` for expensive components, `useMemo` for computed values, and `useCallback` for stable callbacks.

## State Management

- Use **Zustand** for global state with the selector pattern: `useStore(s => s.field)`.
- Never destructure the entire Zustand store.
- Persist only necessary state using `persist` middleware.
- Local component state should use `useState`; avoid lifting state unnecessarily.

## Forms

- Use React Hook Form with Zod validation schema.
- Never manage form state manually with `useState`.
- Display errors using field-specific error messages.

## Accessibility

- All interactive elements must have accessible labels (`aria-label`, `aria-labelledby`, or visible text).
- Focus management must be handled for modals, tabs, and dynamic content.
- Use semantic HTML elements (`button`, `nav`, `header`, `main`, `footer`).

## Performance

- Code-split route-level components with `lazy` + `Suspense`.
- Avoid inline functions in render; use `useCallback` instead.
- Use `React.dev` profiler to identify re-render bottlenecks.