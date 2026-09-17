---
description: Universal engineering invariants, pure functional TypeScript, Result Pattern, zero any, zero class
globs:
  - "**/*.ts"
  - "**/*.tsx"
scope:
  - "tool:edit(*.ts)"
  - "tool:edit(*.tsx)"
  - "tool:write(*.ts)"
  - "tool:write(*.tsx)"
condition:
  - ":\\s*any\\b|as\\s+any\\b"
  - "\\bclass\\s+[A-Z]"
---

# Universal Engineering Invariants

Mandatory engineering standards for all agents.
Apply these principles regardless of language, framework, or technology.

## Core Principles

1. **Pure Functional TypeScript & Paradigm Rigor**
   Strictly prohibit `class` and `this`. Prefer composable pure functions, closures, and explicit parameter passing. Zero `React.FC`.

2. **Zero `any` Policy**
   Never use `any` in TypeScript annotations or assertions. Use `unknown`, explicit generics, or parse schemas (Zod, Valibot) at trust boundaries.

3. **Result Pattern**
   Domain services and API actions must return typed Result shapes: `{ success: true, data } | { success: false, error }`. Never throw unhandled exceptions across architectural boundaries.

4. **Vertical Slicing (Feature Folders)**
   Organize domain logic in `src/modules/<FeatureName>/` (components, hooks, services). Do not create horizontal dumping grounds.

5. **Provider Adapter Pattern (DIP)**
   Domain services must import only agnostic contracts from `src/providers/<domain>/`, never vendor-specific functions or DTOs directly.

6. **Single Source of Truth (SSOT)**
   Zero hardcoded magic numbers, URLs, ports, or environment strings. All configuration lives in central configs (`envs.ts`, design tokens).

7. **Simplicity First**
   Prefer the simplest solution that satisfies requirements. Remove unnecessary complexity before adding abstractions.

8. **Single Responsibility**
   Keep modules, functions, components, and services focused on one clear responsibility.

9. **Security by Default**
   Treat external input as untrusted. Validate boundaries, minimize privileges, protect secrets, and avoid exposing sensitive information.
