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

## 3. Pinned Exact Dependencies (Zero Caret / Tilde)

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

## 4. TypeScript Rules

- **Zero `any`**: Use explicit generics or `unknown` with type guards.
- **Types vs Interfaces**: Use `interface` for extensible object shapes and `type` for unions/primitives.
- **Enums Prohibited**: Use TypeScript union literals (e.g. `type Status = 'idle' | 'loading' | 'success' | 'error'`).
- **No `React.FC`**: Type props directly in function signatures.

---

## 5. Lookup Dictionary Pattern Over `switch` Statement

Prefer declarative Lookup Dictionaries (`Record<string, T>`) over imperative `switch(string)` or multiple `if/else` conditionals for factory functions, state mappers, or strategy selection.

- **Performance**: Constant `O(1)` hash lookup vs `O(N)` sequential string comparison.
- **Maintainability (SOLID - Open/Closed)**: Adding new keys is declarative and risk-free without mutating control flow logic.

```typescript
// ❌ BAD: Imperative switch statement
switch (key) {
  case "openai":
  case "chatgpt":
    return openaiProvider;
  case "gemini":
  default:
    return geminiProvider;
}

// ✅ GOOD: Declarative Lookup Dictionary (O(1))
const PROVIDER_MAP: Record<string, AIProviderInterface> = {
  openai: openaiProvider,
  chatgpt: openaiProvider,
  gemini: geminiProvider,
};

export const getAIProvider = (key?: string): AIProviderInterface => {
  return PROVIDER_MAP[key?.toLowerCase().trim() || "gemini"] ?? geminiProvider;
};
```

---

## 6. Strict DRY Invariant (Zero Duplication of Prompts, Logic & Strings)

Never duplicate system prompts, magic strings, complex regex, configuration objects, or validation logic across multiple files.

- **System Prompts & LLM Schemas**: Extract into single shared constants (e.g. `constants/aiPrompts.constant.ts`) and import across adapters.
- **Environment Variables & Config**: Use `configEnvs` from `src/utils/env.ts` instead of scatter-reading `process.env.*`.

```typescript
// ❌ BAD: Duplicating system prompts or env reads across adapters
const systemPrompt = "Eres un copiloto y planificador de rutas..."; // repeated in 4 files
const key = process.env.API_KEY;

// ✅ GOOD: Single source of truth constants & configEnvs
import { AI_ROUTE_SYSTEM_PROMPT } from "../constants/aiPrompts.constant";
import { configEnvs } from "../../../utils";
```

---

## 7. Deterministic Pre-Delivery Gate

Before marking any technical task as done, verify:

```bash
bun run biome:check && bun run check && bun test
# OR
pnpm fix && pnpm tsc --noEmit && pnpm test
```
