# Artifact Template: Technical Standards (`technical_standards.md`)

```markdown
# Code and Scaffolding Technical Standards

**Project**: [Product Name]
**Date**: [Current Date]
**Tech Lead / Architect**: Vicky (Technical Architect)

---

## 1. Screaming Architecture & Folder Structure

```text
src/
├── modules/
│   ├── [feature_1]/
│   │   ├── components/
│   │   ├── services/
│   │   ├── hooks/
│   │   └── types/
│   └── [feature_2]/
└── shared/
    ├── ui/
    ├── lib/
    └── utils/
```

---

## 2. Code Conventions and Patterns
- **Result Pattern**: Async functions return `{ ok: boolean, data?: T, error?: String }`.
- **Early Returns**: Prior validation of edge cases before main execution.
- **SOLID**: Single Responsibility per file and module.

---

## 3. Code Quality and Linters
- **Linter & Formatter**: ESLint + Prettier / Biome.
- **Type Checking**: TypeScript in strict mode (`strict: true`).

---

## 4. Testing and Scaffolding Strategy
- **Unit Tests**: Vitest / Jest for domain services.
- **Component Tests**: React Testing Library for shared components.
```
