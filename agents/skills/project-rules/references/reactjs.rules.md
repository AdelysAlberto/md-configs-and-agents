---
trigger: model_decision
---

<!-- Adaptado para Antigravity -->
---
description: 'ReactJS development standards and best practices'
applyTo: '**/*.tsx, **/*.ts, **/*.css'
---

# ReactJS Development Instructions

Instructions for building high-quality ReactJS applications with modern patterns, hooks, and best practices following the official React documentation at https://react.dev.

## Project Context
- React 18.2.0 — functional components with hooks only
- TypeScript 5.7.2 with strict mode
- Vite 5 build tool
- Zustand 4 for client state, TanStack Query 5 for server state
- ESLint 9 for linting (no Biome, no Prettier)
- UI library: `@sice/frontend-components` (use before building custom UI)
- Plain CSS files / CSS Modules for styling (no CSS-in-JS, no styled-components)

## Development Standards

### Architecture
- Use functional components with hooks as the primary pattern
- Implement component composition over inheritance
- Organize components by feature or domain for scalability
- Separate presentational and container components clearly
- Use custom hooks for reusable stateful logic
- Implement proper component hierarchies with clear data flow

### TypeScript Integration
- Use TypeScript interfaces for props, state, and component definitions
- Define proper types for event handlers and refs
- Implement generic components where appropriate
- Use strict mode in `tsconfig.json` for type safety (already configured)
- **Never use `React.FC`** — deprecated in React 19; use direct function declarations
- Create union types for component variants and states

### Component Design
- Follow the single responsibility principle for components
- Use descriptive and consistent naming conventions
- Implement proper prop validation with TypeScript or PropTypes
- Design components to be testable and reusable
- Keep components small and focused on a single concern
- Use composition patterns (render props, children as functions)

### State Management
- Use `useState` for local component state
- Implement `useReducer` for complex state logic within a component
- Use **Zustand** for global client state (not Redux, not Context for global state)
- Use **TanStack Query** for all server/async state — not Zustand
- See `.github/instructions/state-management.instructions.md` for store patterns

### Hooks and Effects
- Use `useEffect` with proper dependency arrays to avoid infinite loops
- Implement cleanup functions in effects to prevent memory leaks
- Use `useMemo` and `useCallback` for performance optimization when needed
- Create custom hooks for reusable stateful logic
- Follow the rules of hooks (only call at the top level)
- Use `useRef` for accessing DOM elements and storing mutable values

### Styling
- **Plain CSS files or CSS Modules only** — no Styled Components, no CSS-in-JS
- Implement responsive design with mobile-first approach
- Use CSS custom properties (variables) for theming
- Implement consistent spacing, typography, and color systems
- Ensure accessibility with proper ARIA attributes and semantic HTML
- See `.github/instructions/styling.instructions.md` for full rules

### Performance Optimization
- Use `React.memo` for component memoization when appropriate
- Implement code splitting with `React.lazy` and `Suspense`
- Optimize bundle size with tree shaking and dynamic imports
- Use `useMemo` and `useCallback` judiciously to prevent unnecessary re-renders
- Implement virtual scrolling for large lists
- Profile components with React DevTools to identify performance bottlenecks

### Data Fetching
- Use **TanStack Query** (`useQuery`, `useMutation`) exclusively — no SWR, no Apollo
- Every service must have a corresponding custom hook in `src/hooks/`
- Implement proper loading, error, and success states
- Use optimistic updates for better user experience
- Implement proper caching strategies with `queryKey` arrays
- See `.github/instructions/services-hooks.instructions.md` for full pattern

### Error Handling
- Implement Error Boundaries for component-level error handling
- Use proper error states in data fetching
- Implement fallback UI for error scenarios
- Log errors appropriately for debugging
- Handle async errors in effects and event handlers
- Provide meaningful error messages to users

### Forms and Validation
- Use controlled components for form inputs
- Use **Zod 3** for schema validation — no Formik, no React Hook Form
- Custom hooks in `src/hooks/` for form logic
- Handle form submission and error states appropriately
- Implement accessibility features for forms (labels, ARIA attributes)
- Use debounced validation for better user experience

### Routing
- Use React Router for client-side routing
- Implement nested routes and route protection
- Handle route parameters and query strings properly
- Implement lazy loading for route-based code splitting
- Use proper navigation patterns and back button handling
- Implement breadcrumbs and navigation state management

### Testing
- Write unit tests for components using React Testing Library
- Test component behavior, not implementation details
- Use **Vitest** as test runner (not Jest)
- Implement integration tests for complex component interactions
- Mock external dependencies and API calls appropriately
- Test accessibility features and keyboard navigation
- See `.github/instructions/testing.instructions.md` for full patterns

### Security
- Sanitize user inputs to prevent XSS attacks
- Validate and escape data before rendering
- Use HTTPS for all external API calls
- Implement proper authentication and authorization patterns
- Avoid storing sensitive data in localStorage or sessionStorage
- Use Content Security Policy (CSP) headers

### Accessibility
- Use semantic HTML elements appropriately
- Implement proper ARIA attributes and roles
- Ensure keyboard navigation works for all interactive elements
- Provide alt text for images and descriptive text for icons
- Implement proper color contrast ratios
- Test with screen readers and accessibility tools

## Implementation Process
1. Plan component architecture and data flow
2. Set up project structure with proper folder organization
3. Define TypeScript interfaces and types
4. Implement core components with proper styling
5. Add state management and data fetching logic
6. Implement routing and navigation
7. Add form handling and validation
8. Implement error handling and loading states
9. Add testing coverage for components and functionality
10. Optimize performance and bundle size
11. Ensure accessibility compliance
12. Add documentation and code comments

## Additional Guidelines
- Follow React's naming conventions (PascalCase for components, camelCase for functions)
- Use meaningful commit messages — see `.github/instructions/commits.instructions.md`
- Implement proper code splitting and lazy loading strategies
- Document complex components and custom hooks with JSDoc
- Use **ESLint** for linting (no Biome, no Prettier) — run `pnpm lint`
- Keep dependencies up to date and audit for security vulnerabilities
- Implement proper environment configuration — use `src/utils/envs.ts`, never `import.meta.env`
- Use React Developer Tools for debugging and performance analysis

## Common Patterns
- Higher-Order Components (HOCs) for cross-cutting concerns
- Render props pattern for component composition
- Compound components for related functionality
- Provider pattern for context-based state sharing
- Container/Presentational component separation
- Custom hooks for reusable logic extraction
