---
description: Universal engineering invariants, cross-language standards, Screaming Architecture, and anti-pattern prevention
globs:
  - "**/*.ts"
  - "**/*.tsx"
  - "**/*.go"
  - "**/*.py"
  - "**/*.rs"
scope:
  - "tool:edit(*)"
  - "tool:write(*)"
condition:
  - ":\\s*any\\b|as\\s+any\\b"
  - "\\bclass\\s+[A-Z]"
---

# Universal Engineering Invariants

Mandatory engineering standards for all agents across all languages (TypeScript, Go, Python, Rust).
Apply these principles universally to eliminate technical debt and prevent anti-patterns.

---

## 1. Paradigm Rigor & Functional Simplicity

- **Pure Functional Core**: Strictly avoid unnecessary state mutation and object-oriented boilerplate (`class`, `this`). Prefer pure functions, composition, and explicit parameters.
- **Zero `any` Policy**: Never use `any` in TypeScript. Use strict types, `unknown`, or validation schemas (Zod, Valibot) at system boundaries.
- **Explicit Error Handling (Result Pattern)**:
  - Services must never throw unhandled exceptions or panic across boundaries.
  - Return typed Result shapes `{ success: true, data } | { success: false, error }` in TypeScript, or idiomatic `(data, error)` in Go.

---

## 2. Screaming Architecture & Vertical Slicing

- The directory structure must **scream the business domain**, not the technical framework:
  - Structure by feature modules: `src/modules/<FeatureName>/` (containing feature-specific components, hooks, services, types).
  - Avoid horizontal dumping grounds (e.g. monolithic `controllers/`, `services/`, `views/` containing unrelated domains).
- Inter-module communication occurs strictly through the public API defined in the module's root `index.ts`. Never deep-import private module internals.

---

## 3. Decoupling, Line Limits & Pure Utilities

- **Hard Limit of 250 LOC**: No source code file may exceed **250 lines of code**. If an implementation approaches this limit, immediately decouple it into subcomponents, hooks, or helper modules.
- **Pure Utilities in `utils/`**:
  - Any logic performing date manipulation, currency conversion, string formatting, or stateless mathematical calculation that does not depend on closures, React hooks, or external state **must be extracted to `utils/`**.

---

## 4. Single Source of Truth (SSOT) & Dependency Inversion (DIP)

- **Zero Hardcoded Constants**: URLs, ports, environment strings, timeouts, and magic numbers must reside in a single authoritative config (`env.ts`, `constants.ts`, or design tokens).
- **Provider Adapter Pattern (DIP)**: High-level domain logic must depend only on agnostic provider interfaces (`src/providers/<domain>/`), never on third-party vendor SDKs directly.

---

## 5. Dependency Management & Zero Deprecated APIs

- **Latest Stable Versions**: When installing or proposing new libraries, research and verify the latest stable release. Pin exact versions in `package.json`.
- **Zero Deprecated APIs**: Never write code using deprecated functions, methods, props, or options. Before completing a task, verify that all used APIs are current and supported.
- **Canonical Tooling**: Use Biome (`biome.json`) for deterministic linting and formatting across the project.
