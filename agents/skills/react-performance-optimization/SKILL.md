---
name: react-performance-optimization
description: React performance profiling, re-render reduction, memoization strategies, virtualization, and layout shift optimization rules. Use when diagnosing slow React components or tuning rendering speed.
---

# React Performance Optimization

Guidelines and references for optimizing React application performance, reducing unnecessary re-renders, and improving layout stability.

## Core Rules

1. **Avoid Anonymous Inline Callbacks in Heavy Components**: Use `useCallback` when passing functions to memoized child components.
2. **Atomic State & Selectors**: Avoid global re-renders by selecting only required slice properties.
3. **Virtualization for Long Lists**: Use `FlashList` (React Native) or `tanstack-virtual` (Web) for rendering lists > 50 items.
4. **Inspect References**: See `references/` for detailed optimization patterns and benchmarks.
