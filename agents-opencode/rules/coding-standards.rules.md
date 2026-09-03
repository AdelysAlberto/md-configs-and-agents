
# Coding Standards

## Core Principle: Truth Over Agreement

Do NOT agree with the user when their approach is incorrect. Always provide the BEST solution:

- ✅ Identify problems clearly, explain why, provide the correct solution, educate
- ❌ Never accept bad practices, implement wrong solutions, or ignore code smells

## Functional Programming — MANDATORY

This project is **strictly functional**. No OOP constructs allowed:

```typescript
// ❌ WRONG — class-based
class UserService {
  constructor(private http: Http) {}
  getUser() { return this.http.get('/user'); }
}

// ✅ CORRECT — functional
const getUser = () => http.get('/user');
```

Banned: `class`, `constructor`, `this`, `extends`, `implements` (except for interfaces/types).

## TypeScript Rules

- **Never use `any`** — use specific types or `unknown`
- Prefer `interface` for object shapes, `type` for unions/aliases
- Use `strict: true` in tsconfig (already configured)
- Use generics for reusable type-safe functions
- Prefer union types over `enum`
- Never use `React.FC` — this project avoids it for consistency; use direct function declarations

```typescript
// ❌ WRONG
const fn = (data: any) => data.value;
enum Status { Active = 'active' }

// ✅ CORRECT
const fn = <T extends { value: string }>(data: T) => data.value;
type Status = 'active' | 'inactive';
```

## React Patterns

- **Do not use `import React from "react"`**: React 18+ and Vite use the new JSX transformer (`jsx-runtime`), so there is no need to import React in every file. Only import the specific hooks or utilities you need (`import { useState, useEffect } from "react"`).

## Comments and Documentation

- **No inline `//` comments** — use JSDoc only
- All JSDoc in English

```typescript
// ❌ WRONG — inline comment
const x = value + 1; // increment by 1

// ✅ CORRECT — JSDoc
/** Increments the balance by the given amount. */
const incrementBalance = (balance: number, amount: number) => balance + amount;
```

## Variables and Declarations

- Use `const` by default, `let` when reassignment is needed, never `var`
- Use optional chaining `?.` and nullish coalescing `??` — never explicit null/undefined checks when avoidable

```typescript
// ❌ WRONG
var name = user && user.profile ? user.profile.name : 'Guest';

// ✅ CORRECT
const name = user?.profile?.name ?? 'Guest';
```

## Modern JavaScript (ES2022+)

- Arrow functions, destructuring, spread, template literals
- `map`, `filter`, `reduce` over imperative `for` loops
- ESM modules (`import`/`export`), no CommonJS `require`
- Node.js built-in imports must use `node:` prefix

```typescript
// ❌ WRONG
import { readFile } from 'fs';

// ✅ CORRECT
import { readFile } from 'node:fs';
```

## Security (OWASP Top 10)

- Sanitize all user inputs before rendering or processing
- Never store sensitive data in localStorage/sessionStorage
- Use HTTPS for all API calls
- Never expose secrets or tokens in client-side code
- Validate at system boundaries using Zod schemas

## Error Handling

- Always wrap async operations in try/catch or use `.catch()`
- Provide meaningful error messages — use `useErrorStore` for global errors
- Use Error Boundaries for React component-level errors

## Function Size

- Max ~20 lines per function — break down longer logic into composable helpers
- Single Responsibility Principle: one function = one concern

## ESLint Auto-fixes

Run before every commit:

```bash
pnpm lint   # eslint --fix
```

Auto-fix patterns:

- Remove unused imports
- Add `node:` prefix to Node.js builtins
- See `.github/instructions/config-setup.instructions.md` for full ESLint config details
