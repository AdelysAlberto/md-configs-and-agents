---
description: 'ReactJS development standards and best practices'
applyTo: '**/*.jsx, **/*.tsx, **/*.js, **/*.ts, **/*.css, **/*.scss'
---

# ReactJS Development Instructions

Instructions for building high-quality ReactJS applications with modern patterns, hooks, and best practices following the official React documentation at https://react.dev.

## Important References
- **Summary Documentation Guidelines**: See `.github/copilot-summary-instructions.md` for when and how to create final summary documentation in the `docs/` folder.
- **JavaScript Refactoring Instructions**: See `.github/refactoring-instructions.md` for JavaScript/TypeScript refactoring best practices and loop structure guidelines.

## Project Context
- Latest React version (React 19+)
- TypeScript for type safety (when applicable)
- Functional components with hooks as default
- Follow React's official style guide and best practices
- Use modern build tools (Vite, Create React App, or custom Webpack setup)
- Implement proper component composition and reusability patterns

## Development Standards

### Architecture
- Use functional components with hooks as primary pattern
- Implement component composition over inheritance
- Organize components by feature or domain for scalability
- Separate presentational and container components clearly
- Use custom hooks for reusable stateful logic
- Implement proper component hierarchies with clear data flow

### TypeScript Integration
- Use TypeScript interfaces for props, state, and component definitions
- Define proper types for event handlers and refs
- Implement generic components where appropriate
- Use strict mode in `tsconfig.json` for type safety
- Leverage React's built-in types (`React.FC`, `React.ComponentProps`, etc.)

### Component Design
- Follow single responsibility principle
- Use descriptive and consistent naming conventions (PascalCase for components, camelCase for functions)
- Implement proper prop validation with TypeScript or PropTypes
- Keep components small, focused, testable and reusable
- Use composition patterns (render props, children as functions, HOCs)

### State Management
- Use `useState` for local component state
- Implement `useReducer` for complex state logic
- Leverage `useContext` for sharing state across component trees
- Consider external state management (Redux Toolkit, Zustand) for complex apps
- Use React Query or SWR for server state management
- Keep state as close as possible to where it's used
- Use immutable data patterns

### Hooks and Effects
- Use `useEffect` with proper dependency arrays to avoid infinite loops
- Implement cleanup functions in effects to prevent memory leaks
- Use `useMemo` and `useCallback` judiciously for performance optimization
- Create custom hooks for reusable stateful logic
- Follow the rules of hooks (only call at the top level)
- Use `useRef` for accessing DOM elements and storing mutable values

### Styling
- Use CSS Modules, Styled Components, or modern CSS-in-JS solutions
- Implement responsive design with mobile-first approach
- Follow BEM methodology or similar naming conventions
- Use CSS custom properties (variables) for theming
- Implement consistent spacing, typography, and color systems

### Performance
- Use `React.memo` for component memoization when appropriate
- Implement code splitting with `React.lazy` and `Suspense`
- Optimize bundle size with tree shaking and dynamic imports
- Implement virtual scrolling for large lists
- Profile components with React DevTools to identify bottlenecks

### Data Fetching
- Use modern data fetching libraries (React Query, SWR, Apollo Client)
- Implement proper loading, error, and success states
- Handle race conditions and request cancellation
- Use optimistic updates for better user experience
- Handle offline scenarios and network errors gracefully

### Forms and Validation
- Use controlled components for form inputs
- Implement proper form validation with libraries like Formik, React Hook Form
- Handle form submission and error states appropriately
- Use debounced validation for better user experience
- Handle file uploads and complex form scenarios

### Routing
- Use React Router for client-side routing
- Implement nested routes and route protection
- Handle route parameters and query strings properly
- Implement lazy loading for route-based code splitting
- Use proper navigation patterns and back button handling

### Testing
- Write unit tests for components using React Testing Library
- Test component behavior, not implementation details
- Use Jest for test runner and assertion library
- Mock external dependencies and API calls appropriately
- Test accessibility features and keyboard navigation

### Additional Guidelines
- Use meaningful commit messages following conventional commits
- Document complex components and custom hooks with JSDoc
- Use Biome for code formatting and linting
- Keep dependencies up to date and audit for security vulnerabilities
- Use React Developer Tools for debugging and performance analysis

## Code Quality Standards

### Clean Code & Architecture
- **Always prioritize clean code** with best practices and SOLID principles
- Follow **Clean Architecture** patterns for scalable and maintainable code
- Use **Functional Programming**: Avoid classes whenever possible, prefer functional components and pure functions
- **Keep code simple and easy to understand** - simplicity is priority over clever solutions
- Use **modern ECMAScript** standards and best practices (latest ES features)

### Comments & Documentation
- **Avoid inline comments** for obvious code - code should be self-explanatory
- ❌ Don't do: `// Set modal width to 678px when DepositMethods is shown`
- ✅ Use **JSDoc comments only for functions** that need documentation
  - Keep JSDoc comments brief and concise
  - Only document the "why" if not obvious, not the "what"
- Example of good JSDoc:
  ```javascript
  /**
   * Triggers registration modal if user data is incomplete
   * @param {string} context - The context where registration is needed
   * @param {boolean} forceRefresh - Force data refresh from API
   * @returns {Promise<boolean>} Whether modal was triggered
   */
  const triggerRegistrationModalIfNeeded = async (context, forceRefresh) => {
    // implementation
  };
  ```

### Function Design
- Break complex functions into smaller, focused functions
- Each function should do one thing and do it well
- Use descriptive function names that clearly indicate intent
- Prefer pure functions (no side effects when possible)
- Use arrow functions as the default

## Security & Accessibility

### Security
- Sanitize user inputs to prevent XSS attacks
- Validate and escape data before rendering
- Use HTTPS for all external API calls
- Implement proper authentication and authorization patterns
- Use Content Security Policy (CSP) headers
- Never expose sensitive data (tokens, user IDs, metadata) in console.log
- Remove all debug logging before production deployment

### Accessibility
- Use semantic HTML elements appropriately
- Implement proper ARIA attributes and roles
- Ensure keyboard navigation works for all interactive elements
- Provide alt text for images and descriptive text for icons
- Implement proper color contrast ratios
- Test with screen readers and accessibility tools

## Clean Architecture Implementation

### Process
1. **Domain Design** - Define entities, value objects, services and TypeScript types
2. **Repository Layer** - Database access in `module/[domain]/repository/`
3. **Use Cases Layer** - Orchestrate business operations and workflows
4. **Custom Hooks** - Consume use cases, manage client-side state
5. **UI Components** - Pure presentation, consume hooks

### Standards
- **Biome** for code formatting and linting
- **Meaningful commits** following conventional commits
- **JSDoc** for complex business logic only
- **Type-driven development** with strict TypeScript
- **Function composition** over class inheritance
- **Immutable data structures** by default
- **Pure functions** without side effects
- **Dependency injection** for testability

## 🔍 **Mandatory Post-Task Verification** (OBLIGATORIO)
### **After completing ANY task, you MUST verify:**

#### ✅ Type Safety Verification
- [ ] **No `any` types**: Search entire codebase for `any` usage and replace with proper types
- [ ] **Explicit return types**: All functions must have explicit return type annotations
- [ ] **Type inference validation**: Verify TypeScript can properly infer all types
- [ ] **Generic types**: Use generics where appropriate for reusable type-safe code
- [ ] **No type assertions**: Avoid using `as` unless absolutely necessary and type-safe

#### ✅ Type Reusability & Organization
- [ ] **Reuse existing interfaces/types**: Check if similar types already exist before creating new ones
- [ ] **Use TypeScript utility types**: Leverage `Pick<>`, `Omit<>`, `Partial<>`, `Required<>`, `Readonly<>`, `Record<>`, `Extract<>`, `Exclude<>` when appropriate
- [ ] **Extend existing types**: Use `extends` or intersection types (`&`) to build on existing type definitions
- [ ] **Domain-specific types**: Organize types in `module/[domain]/types/` for domain boundaries
- [ ] **Shared types**: Common types in `lib/types/` for cross-domain usage

#### ✅ Code Cleanliness
- [ ] **Remove unused variables**: Delete all declared but unused variables
- [ ] **Remove unused imports**: Clean up all unused import statements
- [ ] **Remove unused functions**: Delete functions that are no longer called
- [ ] **Remove debug code**: Eliminate all `console.log`, `console.error`, `console.warn` statements
- [ ] **Remove commented code**: Delete old commented-out code blocks

#### ✅ Deprecated Code Check
- [ ] **No deprecated libraries**: Verify all npm packages are actively maintained and not deprecated
- [ ] **No deprecated APIs**: Check that no deprecated React, Next.js, or library APIs are used
- [ ] **No deprecated properties**: Ensure component props and function parameters are current
- [ ] **Check migration guides**: Review official documentation for breaking changes and deprecations
- [ ] **Update dependencies**: Use latest stable versions when possible

#### ✅ Code Quality Verification
- [ ] **No linting errors**: Run linter and fix all errors
- [ ] **No runtime errors**: Test code execution in all relevant scenarios
- [ ] **Proper error handling**: All async operations have try/catch or .catch()
- [ ] **Accessibility compliance**: ARIA attributes, semantic HTML, keyboard navigation
- [ ] **Performance check**: No unnecessary re-renders, proper memoization where needed

#### ✅ Architecture Compliance
- [ ] **Clean Architecture**: Business logic in domain layer, UI in presentation layer
- [ ] **DDD principles**: Proper domain boundaries and entity definitions
- [ ] **SOLID principles**: Single responsibility, proper abstractions
- [ ] **Functional programming**: Pure functions, immutability, no unnecessary classes
- [ ] **Repository pattern**: All database queries in repository layer

### **Verification Commands**
\`\`\`bash

# Linting
pnpm biome check .

# Find unused exports
pnpm find-unused-exports

# Check for deprecated packages
pnpm outdated
\`\`\`

### **Before Completing Task**
1. Run all verification commands listed above
2. Fix all errors and warnings found
3. Review changed files one more time
4. Ensure all checklist items are completed
5. Commit changes with descriptive message in English

> **IMPORTANT**: Do not mark a task as complete until ALL verification items pass. This is non-negotiable and mandatory for every single task.

> **Mantra**: Always use the most updated solutions and latest JavaScript/ECMAScript practices
