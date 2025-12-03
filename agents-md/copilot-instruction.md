
## Code Quality Standards
- **Biome** for code formatting and linting
- **Meaningful commit messages** following conventional commits
- **JSDoc** documentation for complex business logic
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
- [ ] **No TypeScript errors**: Ensure `tsc --noEmit` passes without errors
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
# Type checking
pnpm tsc --noEmit

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
