---
trigger: model_decision
description: 'Pure functional TypeScript, Biome, and coding standards'
applyTo: 'src/**/*.ts, src/**/*.tsx'
---

# Universal Coding Standards

## 1. Radical Simplicity & Technical Honesty
- **Simplicity First**: Always prioritize the simplest, most readable solution. Avoid over-engineering, speculative layers, abstractions for future use cases, or unnecessary complexity.
- **Technical Honesty ("Say I don't know")**: Never invent solutions, fake APIs, or hallucinate non-existent features. If a requirement is ambiguous or a capability is unverified, acknowledge it explicitly and ask rather than guessing.

---

## 2. Functional Programming Invariant (Zero OOP)
This codebase is **strictly functional**. Object-oriented structures are prohibited.
- **Banned**: `class`, `this`, `constructor`, `extends` (on classes), `implements`.
- **Allowed**: Pure functions, closures, function composition, factory functions.

```typescript
// ❌ BAD: OOP / Class-based
class UserService {
  constructor(private http: HttpClient) {}
  getUser(id: string) { return this.http.get(`/users/${id}`); }
}

// ✅ GOOD: Pure functional
export const getUser = (http: HttpClient) => (id: string) => http.get(`/users/${id}`);
```

---

## 2. Pinned Exact Dependencies (Zero Caret / Tilde)
In `package.json`, wildcards (`^`, `~`) are strictly forbidden. Always pin exact versions to ensure deterministic, reproducible builds.

```json
// ❌ BAD
"dependencies": {
  "react": "^19.0.0",
  "zustand": "~5.0.0"
}

// ✅ GOOD
"dependencies": {
  "react": "19.0.0",
  "zustand": "5.0.3"
}
```

---

## 3. TypeScript Rules
- **Zero `any`**: Use explicit generics or `unknown` with type guards.
- **Types vs Interfaces**: Use `interface` for extensible object shapes and `type` for unions/primitives.
- **Enums Prohibited**: Use TypeScript union literals (e.g. `type Status = 'idle' | 'loading' | 'success' | 'error'`).
- **No `React.FC`**: Type props directly in function signatures.

---

## 4. Deterministic Pre-Delivery Gate
Before marking any technical task as done, verify:
```bash
bun run biome:check && bun run check && bun test
# OR
pnpm fix && pnpm tsc --noEmit && pnpm test
```
