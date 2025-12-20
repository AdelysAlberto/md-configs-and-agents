# Copilot Instructions: Clean Code and Best Practices

This document outlines the clean code principles, updated rules, and best practices for using TypeScript and JavaScript in this project. These guidelines aim to ensure high performance, maintainability, and readability of the codebase.

## Important References
- **Summary Documentation Guidelines**: See `.github/copilot-summary-instructions.md` for when and how to create final summary documentation in the `docs/` folder.
- **JavaScript Refactoring Instructions**: See `.github/refactoring-instructions.md` for JavaScript/TypeScript refactoring best practices and loop structure guidelines.

## Documentation Standards
1. **Language**: All documentation must be written in English
2. **Format**: Use clear, concise language with proper Markdown formatting
3. **Structure**: Follow consistent documentation patterns across the project
4. **Examples**: Include practical code examples and usage scenarios

## General Principles
1. **Write Readable Code**: Code should be self-explanatory. Use meaningful variable and function names.
2. **Keep It Simple**: Avoid over-engineering. Solve the problem at hand without unnecessary complexity. Simplicity is priority over clever solutions.
3. **Follow DRY (Don't Repeat Yourself)**: Reuse code wherever possible to reduce redundancy.
4. **No Deprecated/Legacy Code**: Always use the most updated solutions and latest JavaScript/ECMAScript practices (ES2022+). We're in December 2025, ensure all practices are current.

## TypeScript Best Practices
1. **Avoid `any`**: Always use specific types or `unknown` if the type is uncertain.
2. **Use Interfaces and Types**: Prefer `interface` for object shapes and `type` for unions or more complex definitions.
3. **Enable Strict Mode**: Ensure `strict` is enabled in `tsconfig.json` to catch potential issues early.
4. **Leverage Generics**: Use generics to create reusable and type-safe components or functions.
5. **Use Enums Sparingly**: Prefer `union types` over `enums` for better type safety and flexibility.
6. **Use `const` and `let`**: Avoid using `var`. Use `const` for constants and `let` for variables that will change.
7. **Avoid `null` and `undefined`**: Use `optional chaining` and `nullish coalescing` to handle cases where values may not exist.
8. **Type-Driven Development**: Define types first, then implement functionality.
9. **Leverage React Built-in Types**: Use `React.FC`, `React.ComponentProps`, `React.ReactNode`, etc.

## JavaScript and Performance Best Practices
1. **Use Modern Syntax**: Always use ES2022+ features like `const`, `let`, arrow functions, destructuring, optional chaining, nullish coalescing.
2. **ESM Modules**: Use Node.js (20+) ESM modules exclusively.
3. **Optimize Loops**: Use `map`, `filter`, and `reduce` instead of traditional `for` loops when appropriate.
4. **Avoid Global Variables**: Encapsulate variables and functions to prevent polluting the global scope.
5. **Debounce and Throttle**: Use these techniques to optimize performance in event handlers.
6. **Lazy Loading**: Load resources or components only when needed to improve performance.
7. **Prefer Pure Functions**: Functions without side effects are easier to test and reason about.
8. **Use Arrow Functions**: As default function syntax for consistency and lexical `this` binding.

## Comments & Documentation
1. **Avoid Inline Comments**: Code should be self-explanatory. Don't comment on obvious code.
   - ❌ Don't do: `// Set modal width to 678px when DepositMethods is shown`
   - ✅ Use descriptive variable/function names instead
2. **Use JSDoc Only for Functions**: Document complex functions with JSDoc, keep it brief and concise.
   - Only document the "why" if not obvious, not the "what"
   - Example:
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
3. **No Commented-Out Code**: Remove old commented code blocks before committing.
4. **Documentation in English**: All comments, documentation, and commit messages must be in English.

## Clean Code Practices
1. **Single Responsibility Principle**: Each function or class should have one responsibility.
2. **Avoid Long Functions**: Break down functions longer than 50 lines into smaller, reusable functions.
3. **Consistent Formatting**: Use Biome for linting and formatting to maintain consistent code style.
4. **Error Handling**: Always handle errors gracefully using `try-catch` or proper fallback mechanisms.
5. **Write Tests**: Ensure all critical functionality is covered by unit and integration tests.
6. **No Debug Code in Production**: Remove all `console.log`, `console.error`, `console.warn` before committing.
7. **Never Expose Sensitive Data**: Never log tokens, user IDs, or metadata in console.

## Architecture

### Project Architecture Pattern
- **Vertical Slicing**: Organize code by feature/domain, not by technical layer.
- **Screaming Architecture**: Directory structure should scream what the application does, not what frameworks it uses.
- **Functional Programming Only**: This project strictly follows functional programming principles.
  - Avoid using object-oriented programming constructs such as `class`, `constructor`, `this`, and inheritance.
  - All logic should be implemented using functions, pure data structures, hooks, and composable patterns.
  - Prefer `type` and `interface` over class-based models.
  - Components and utilities must be stateless unless explicitly needed and managed via hooks or Zustand.

### Service Layer Architecture
Always respect the following architecture for API integration:

```
UI Component <-> Custom Hook (React Query + Mapper/Adapter) <-> Service Layer
```

1. **UI Components**: Pure presentation, consume hooks, no business logic.
2. **Custom Hooks**: Use `useQuery`/`useMutation` from React Query, handle loading/error states.
3. **Mapper/Adapter**: Transform API responses to prevent UI breaks when backend changes.
4. **Service Layer**: HTTP calls, business logic, error handling.

### Component Design
- **Extract Complex Logic from Components**:
  - ✅ **Custom Hooks**: Extract stateful logic, side effects, and complex state management to custom hooks.
  - ✅ **Utility Functions**: Extract pure calculations, data transformations, and business logic to utility files.
  - ✅ **Keep components under 200 lines**: If a component exceeds 200 lines, it's a signal to refactor.
  - ✅ **Components should focus on UI**: Logic for data fetching, validation, calculations should be in hooks/utils.
  - ❌ **Avoid**: Multiple `useEffect`, complex calculations, or extensive business logic directly in components.
- **Follow Single Responsibility Principle**: Each component does one thing well.
- **Use Descriptive Naming**: PascalCase for components, camelCase for functions.
- **Implement Proper Prop Validation**: Use TypeScript interfaces for all props.
- **Keep Components Small**: Focused, testable, and reusable.
- **Use Composition Patterns**: Render props, children as functions, Higher-Order Components (HOCs) when appropriate.

### State Management
- **Local State**: Use `useState` for component-local state.
- **Complex State Logic**: Implement `useReducer` for complex state machines.
- **Global State**: Use **Zustand** for persistent global state management.
- **Server State**: Use **React Query** or **SWR** for server state management.
- **Context API**: Use `useContext` sparingly for sharing state across component trees.
- **Keep State Close**: State should be as close as possible to where it's used.
- **Immutable Data Patterns**: Always use immutable data structures.

### Hooks and Effects
- **Use `useEffect` with Proper Dependencies**: Always include all dependencies to avoid infinite loops.
- **Implement Cleanup Functions**: Prevent memory leaks by cleaning up in effects.
- **Use `useMemo` and `useCallback` Judiciously**: Only for performance optimization when needed.
- **Create Custom Hooks**: For reusable stateful logic.
- **Follow Rules of Hooks**: Only call at the top level, not in loops/conditions.
- **Use `useRef`**: For accessing DOM elements and storing mutable values.

## Project-Specific Guidelines
1. **Component Structure**: Organize components in a modular and reusable way following Vertical Slicing.
2. **State Management**: Use **Zustand** for global state persistence.
3. **Avoid Inline Styles**: Use CSS modules or styled-components for styling.
4. **Environment Variables**: Use `utils/envs.ts` for managing environment-specific configurations.
5. **API Integration**: Use the `infra/http` module for all HTTP requests and error handling.
6. **Domain Modules**: Always respect domain boundaries in Vertical Slicing architecture.
7. **State Persistence**:
   - Use **CookieStore API** (https://developer.mozilla.org/en-US/docs/Web/API/CookieStore) for persistent state (new in 2025).
   - Use `sessionStorage` when temporary session-based storage is needed.
8. **Token Validation**: Use **react-jwt** for token verification and validation.
9. **Internationalization (i18n)**:
   - Always use i18n for all user-facing text.
   - Before adding new translations, verify that the key doesn't already exist.
   - If the translation key doesn't exist, create it in all language files.
   - This is mandatory and part of the Post-Task Verification.
10. **HTTP Requests**: Use the `src/infra/http/http.ts` module for all private HTTP requests that require a token. Ensure proper error handling and token management.

## Clean Architecture Implementation

### Development Process
1. **Domain Design** - Define entities, value objects, services and TypeScript types.
2. **Repository Layer** - Database/API access in `module/[domain]/repository/`.
3. **Use Cases Layer** - Orchestrate business operations and workflows.
4. **Custom Hooks** - Consume use cases, manage client-side state with React Query.
5. **UI Components** - Pure presentation, consume hooks, no business logic.

### Architecture Standards
- **Biome** for code formatting and linting.
- **Meaningful Commits** following conventional commits.
- **JSDoc** for complex business logic only.
- **Type-Driven Development** with strict TypeScript.
- **Function Composition** over class inheritance.
- **Immutable Data Structures** by default.
- **Pure Functions** without side effects.
- **Dependency Injection** for testability.

### Function Design
- Break complex functions into smaller, focused functions.
- Each function should do one thing and do it well.
- Use descriptive function names that clearly indicate intent.
- Prefer pure functions (no side effects when possible).
- Use arrow functions as the default.
- **Extract Logic Strategically**:
  - Functions > 50 lines should be split into smaller functions.
  - If logic is reused, extract to utility file (`src/util/`).
  - If logic uses React state/effects, extract to custom hook (`src/hooks/`).
  - Group related utilities in feature-specific files (e.g., `dateUtils.ts`, `validationUtils.ts`).

## Security & Accessibility

### Security
- **Sanitize User Inputs**: Prevent XSS attacks by validating and escaping all user inputs.
- **Validate Data**: Escape data before rendering.
- **Use HTTPS**: For all external API calls.
- **Proper Authentication**: Implement proper authentication and authorization patterns.
- **Content Security Policy**: Use CSP headers.
- **Never Expose Sensitive Data**: Never log tokens, user IDs, or metadata in console.
- **Remove Debug Logging**: Remove all `console.log`, `console.error`, `console.warn` before production deployment.

### Accessibility
- **Semantic HTML**: Use appropriate HTML elements (header, nav, main, article, section, etc.).
- **ARIA Attributes**: Implement proper ARIA attributes and roles.
- **Keyboard Navigation**: Ensure all interactive elements work with keyboard.
- **Alt Text**: Provide alt text for images and descriptive text for icons.
- **Color Contrast**: Implement proper color contrast ratios (WCAG AA/AAA).
- **Screen Reader Testing**: Test with screen readers and accessibility tools.

## Component Usage Guidelines

When implementing UI elements, always prioritize using the pre-built components available in the `src/components` directory. These components are designed to be reusable and consistent across the project. Examples include:

- **BaseButton**: For all button elements. Ensure to strictly follow the `IButton` interface and avoid passing unsupported props like `children`. Verify and include all required props as defined in the interface.
- **BaseInput**: For input fields. Ensure to check the `IBaseInput` interface for required props and include them.
- **BaseImage**: For displaying images. Verify the interface for any mandatory props.
- **BaseLabel**: For text labels. Ensure all required props are included as per the interface.
- **BaseLink**: For hyperlinks. Check the interface for required props and include them.
- **BaseLoading**: For loading indicators. Verify and include all mandatory props.
- **BaseMessage**: For displaying messages or alerts. Ensure all required props are included as per the interface.

To simplify imports and avoid multiple import statements for BaseComponents, a barrel file is provided in the `src/components` directory. Use this barrel file to group and manage imports efficiently. For example:

```typescript
import { BaseButton, BaseInput, BaseImage } from 'src/components';
```

Using these components ensures consistency in design and behavior throughout the application. If a required component does not exist, consider creating it in the `src/components` directory before using external libraries or custom implementations.

## Biome Code Quality and Automated Fixes

When working with this project, automatically detect and fix the following common Biome issues:

### **Automatic Fixes to Apply**

1. **Unused Imports (`lint/correctness/noUnusedImports`)**
   - **Action**: Remove any unused import statements immediately
   - **Example**: Remove `import { unused } from 'module'` if `unused` is not used in the file
   - **Always apply**: Yes, this is always safe to fix

2. **Node.js Import Protocol (`lint/style/useNodejsImportProtocol`)**
   - **Action**: Add `node:` prefix to Node.js builtin module imports
   - **Example**: Change `import { create } from "domain"` to `import { create } from "node:domain"`
   - **Always apply**: Yes, this improves code clarity

3. **Multiple Unused Imports**
   - **Action**: When multiple imports from the same module are unused, remove only the unused ones
   - **Example**: In `import { used, unused1, unused2 } from 'module'`, keep only `import { used } from 'module'`

### **Biome Fix Commands**
When you detect these issues, suggest or automatically apply:

```bash
# Fix all safe issues
pnpm biome check --write src

# Fix all issues including unsafe ones (when specifically requested)
pnpm biome check --write --unsafe src
```

### **Detection Patterns**
Automatically identify and fix these patterns when seen in code:

- Any import statement where the imported item is not used in the file scope
- Node.js builtin modules imported without the `node:` protocol (`fs`, `path`, `domain`, `process`, etc.)
- Destructured imports where some items are unused

### **Before Committing**
Always run Biome fixes before any git operations to ensure code quality standards:

```bash
pnpm fix  # This runs biome check --fix ./src
```

---

## Tools and Configurations
1. **ESLint**: Ensure the project adheres to the defined linting rules.
2. **Biome**: Use BiomeJs for consistent code formatting.
3. **Husky**: Set up pre-commit hooks to enforce linting and testing before commits.
4. **TypeScript Config**: Keep `tsconfig.json` updated with the latest compiler options.

## Commits

To maintain a clear and consistent commit history, use the **Conventional Commits** standard. This format helps identify the purpose of each change and facilitates automatic changelog generation.

### Commit Format

The basic format must start with the branch ID number, preceded by `#`, followed by the rules of **Conventional Commits**.

#### Example Format
If the branch name is `feature/232574`, the commit must start with: `#232574 feat: description of the change`

#### Rules
1. The prefix `#<ID>` must match the branch number.
2. After the prefix, follow the **Conventional Commits** format.

### Commit Process (Required Steps)

**Before creating any commit, always follow these steps:**

1. **Verify the current branch** to obtain the ID number:
   ```bash
   git branch | grep '\*'
   ```
   Extract the ID from the branch name (e.g., `feature/249517` → use `249517`)

2. **Check the staged changes** to understand what was modified:
   ```bash
   git status
   git diff --cached
   ```

3. **Create a descriptive message** based on the actual changes made, following the format:
   ```
   #<BRANCH_ID> <type>(<scope>): <description>
   ```

4. **Execute the commit** with the correct branch ID and descriptive message:
   ```bash
   git commit -m "#249517 fix(account): improve account name validation error message"
   ```

**This ensures every commit has the correct branch ID and an accurate description of the changes.**

### Commit Types

- **feat**: Use for introducing a new feature.
- **fix**: Use for fixing a bug.
- **docs**: Changes related to documentation (e.g., README, comments).
- **style**: Changes that do not affect logic (e.g., formatting, spaces, semicolons).
- **refactor**: Code changes that neither fix a bug nor add a feature (e.g., performance improvements, code cleanup).
- **test**: Adding or correcting tests.
- **chore**: Changes to build tasks or auxiliary tools (e.g., ESLint, Husky configuration).
- **perf**: Changes that improve performance.
- **ci**: Changes to continuous integration configuration.
- **build**: Changes that affect the build system or external dependencies.

#### Commit Examples

- **Adding a new feature:**  
  `#232574 feat(auth): add support for Google authentication`

- **Fixing a bug:**  
  `#232574 fix(api): fix token validation issue`

- **Configuration changes:**  
  `#232574 chore(eslint): update linting rules`

---

## 🔍 **Mandatory Post-Task Verification** (OBLIGATORIO)

### **After completing ANY task, you MUST verify:**

#### ✅ Code Cleanliness
- [ ] **Remove unused variables**: Delete all declared but unused variables
- [ ] **Remove unused imports**: Clean up all unused import statements
- [ ] **Remove unused functions**: Delete functions that are no longer called
- [ ] **Remove debug code**: Eliminate all `console.log`, `console.error`, `console.warn` statements
- [ ] **Remove commented code**: Delete old commented-out code blocks

#### ✅ Deprecated Code Check
- [ ] **No deprecated libraries**: Verify all npm packages are actively maintained and not deprecated
- [ ] **No deprecated APIs**: Check that no deprecated React, Node.js, or library APIs are used
- [ ] **No deprecated properties**: Ensure component props and function parameters are current
- [ ] **Check migration guides**: Review official documentation for breaking changes and deprecations
- [ ] **Update dependencies**: Use latest stable versions when possible (December 2025 standards)

#### ✅ Code Quality Verification
- [ ] **No linting errors**: Run linter and fix all errors
- [ ] **No runtime errors**: Test code execution in all relevant scenarios
- [ ] **Proper error handling**: All async operations have try/catch or .catch()
- [ ] **Accessibility compliance**: ARIA attributes, semantic HTML, keyboard navigation
- [ ] **Performance check**: No unnecessary re-renders, proper memoization where needed

#### ✅ Architecture Compliance
- [ ] **Clean Architecture**: Business logic in domain layer, UI in presentation layer
- [ ] **Vertical Slicing**: Code organized by feature/domain, respecting domain boundaries
- [ ] **Service Layer Pattern**: UI <-> Hook (React Query + Mapper) <-> Service
- [ ] **SOLID principles**: Single responsibility, proper abstractions
- [ ] **Functional programming**: Pure functions, immutability, no classes/constructors/this
- [ ] **Component size**: UI components under 200 lines, complex logic extracted to hooks/utils

#### ✅ Project-Specific Verification
- [ ] **Zustand for global state**: All persistent state uses Zustand stores
- [ ] **CookieStore API**: Used for state persistence (not localStorage)
- [ ] **react-jwt**: Token validation implemented correctly
- [ ] **i18n translations**: 
  - All user-facing text uses i18n keys
  - Verified translation keys don't already exist
  - Created new keys in ALL language files if needed
  - No hardcoded strings in UI components
- [ ] **HTTP module**: All API calls use `src/infra/http/http.ts`
- [ ] **Custom hooks**: Services consumed via React Query hooks with proper mappers/adapters

### **Verification Commands**
```bash
# Run Biome linting and formatting
pnpm biome check --write src

# Check for linting errors
pnpm fix

# Check for outdated/deprecated packages
pnpm outdated

# Run tests
pnpm test
```

### **Before Completing Task**
1. ✅ Run all verification commands listed above
2. ✅ Fix all errors and warnings found
3. ✅ Review changed files one more time
4. ✅ Ensure ALL checklist items are completed
5. ✅ Verify i18n translations are properly created
6. ✅ Confirm no sensitive data is logged
7. ✅ Commit changes with descriptive message in English following Conventional Commits

> **CRITICAL**: Do not mark a task as complete until ALL verification items pass. This is non-negotiable and mandatory for every single task.

> **Mantra**: Always use the most updated solutions and latest JavaScript/ECMAScript practices (ES2022+). We're in December 2025 - ensure all code follows current standards, not deprecated or legacy patterns.

---

By following these guidelines, we can maintain a high-quality codebase that is easy to understand, extend, and maintain.
