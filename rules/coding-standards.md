---
trigger: model_decision
---

# Coding Standards

## Core Principle: Truth Over Agreement
Do NOT agree with the user to please them. Always provide the BEST solution. Identify problems, explain why, provide the correct solution, educate.

## Language & Syntax
- ES2022+ exclusively: `const`/`let` (never `var`), arrow functions, optional chaining (`?.`), nullish coalescing (`??`), destructuring
- ESM modules only — no CommonJS `require()`
- Strict equality `===` — never `==`
- Early returns to reduce nesting

## TypeScript Rules
- **No `any`** — use specific types or `unknown`
- Prefer `interface` for object shapes, `type` for unions/intersections
- `strict: false` in this project (per `tsconfig.json`) — still avoid `any`
- Use generics for reusable, type-safe code
- Prefer union types over enums: `type Status = "active" | "inactive"` not `enum Status`
- React built-in types: `FC`, `ComponentProps`, `ReactNode`, `MouseEvent`, etc.

## Functional Programming (MANDATORY)
❌ NO classes, constructors, `this`, inheritance  
✅ Pure functions, immutable data, composition

```ts
// ❌ Wrong
class UserService {
  getUser(id: string) { return fetch(`/users/${id}`); }
}

// ✅ Correct
const getUser = (id: string) => fetch(`/users/${id}`);
```

## Functions
- Max 50 lines per function — split if longer
- Single responsibility: one function, one job
- Descriptive names: `getUserByEmail` not `getData`
- Extract reused logic → `src/util/`, stateful logic → `src/hooks/`

## Loop Structure (choose carefully)
| Construct | Use when |
|---|---|
| `map` | Transform array → same-length array, no side effects |
| `filter` | Select subset of array |
| `reduce` | Accumulate to single value |
| `forEach` | Side effects only (no return value) |
| `for...of` | Need `break`/`continue` or `await` inside loop |
| `for` | Index-dependent logic, performance-critical |
| `for...in` | Object keys only — **never** for arrays |

## Comments
❌ No inline comments explaining obvious code  
❌ No commented-out code blocks  
✅ JSDoc only for complex functions — document the "why", not the "what"  
✅ All comments and docs in **English**

```ts
// ❌ // Set width to 678px when DepositMethods is shown
// ✅ const DEPOSIT_MODAL_WIDTH = 678;
```

## Biome (auto-apply these fixes)
```bash
pnpm fix            # biome check --fix ./src
pnpm biome check --write src
```
- Remove unused imports immediately
- Add `node:` prefix to Node.js builtins: `import { readFile } from "node:fs"`
- Run before every commit

## Commits (Conventional Commits)
Format: `#TICKET_ID type(scope): description`

| Type | When |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `refactor` | Code improvement, no behavior change |
| `chore` | Config, tooling |
| `style` | Formatting only |
| `perf` | Performance improvement |
| `docs` | Documentation only |

Example: `#232574 feat(auth): add Google authentication`
