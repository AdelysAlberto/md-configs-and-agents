---
name: plan
description: Interactive technical planning and architectural blueprint drafting. Generates <TOPIC>_PLAN.md with PENDING status, Q&A alignment loop, and Technical Analysis & Best Practices.
---

# Interactive Technical Planning & Architecture Blueprint Skill

This skill governs the systematic deconstruction of complex tasks into an executable technical blueprint (`<TOPIC>_PLAN.md`) in the workspace root.

## Construction Hierarchy Metaphor
- **The Architect (`@sheldon`)**: Evaluates requirements, models system architecture, and authors the technical blueprint.
- **The Master Builder (`@profesor`)**: Coordinates overall project strategy, context initialization, and phase transitions.
- **The Senior Worker (`@homero`)**: Executes construction across Frontend, Backend, Mobile, Go, Rust, Python, and Infrastructure with Clean Code.
- **The UI/UX Specialist (`@edna`)**: Shapes design systems, layout wireframes, tokens, and presentation components.
- **The Inspector (`@bob`)**: Performs strict read-only reviews of code diffs and verification gates.

---

## 3-Phase Execution Protocol

### Phase 1 — Discovery & Clarifying Q&A Loop
Before writing any plan:
1. Deeply inspect the workspace context, existing rules, state stores, and contracts.
2. Formulate targeted questions to resolve ambiguities, stack preferences, or edge cases.
3. If necessary, notify the user or prompt for decision alignment before committing to a plan.

### Phase 2 — Technical Analysis & Best Practices Formulation
Analyze:
- **Design Patterns**: Service-Repository, DIP adapters, Result Pattern, Screaming Architecture.
- **Modularity & LOC Limits**: Keep every view, hook, and module under 250 LOC (screens < 100 LOC).
- **Zero Deprecated APIs & SSOT**: Ensure all dependencies use current APIs and configuration lives in a single source of truth.
- **Pure Utilities**: Place pure functions in `utils/` (no closures).

### Phase 3 — Draft `<TOPIC>_PLAN.md`
Generate or update the plan file in the workspace root adhering to this structure:

```markdown
# [Short Descriptive Title] Plan

> **Target File:** `<TOPIC>_PLAN.md` (e.g., `REFACTOR_AUTH_PLAN.md`, `SEARCH_FEATURE_PLAN.md`)
> **Status:** `PENDING` <!-- Changes to IN_PROGRESS or COMPLETED during execution -->
> **Architect:** Sheldon Cooper
> **Assigned Worker:** Homer Simpson (Homero)

---

## 1. Technical Analysis & Recommended Best Practices
- **Architecture & Patterns:** [Vertical slicing, Result Pattern, DIP]
- **State Management & Data Fetching:** [Zustand atomic selectors, TanStack Query hooks]
- **LOC & Decoupling Audit:** [All components strictly < 250 LOC]
- **SSOT & Clean Code Invariants:** [Zero magic numbers, pure utils in utils/]

---

## 2. Proposed Changes

### Component / Module: [Name]
#### [NEW / MODIFY / DELETE] `path/to/file.ts`
- Detailed change description and rationale.

---

## 3. Verification Gate
```bash
bun run biome:check && bun run check && bun test
```
```
