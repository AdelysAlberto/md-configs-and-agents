---
name: scrum-planning
description: Agile planning, Epics and User Story decomposition, granular 1-by-1 developer tasks, strict technical dependency ordering, and verifiable acceptance criteria. Use when organizing sprint plans or breaking down roadmaps.
license: MIT
compatibility: opencode
metadata:
  domain: agile
  methodology: scrum
---

# Scrum Planning & Task Breakdown Standards

Master guide for decomposing Product Requirement Documents (PRDs), architecture specs, and visual wireframes into granular, ordered, and executable developer tasks.

## 1. Core Planning Invariants

1. **Strict Dependency Order**: Always order tasks chronologically according to architectural dependencies:
   `Data Layer (DB / Schema / Migrations)` ➔ `Service Layer (Contracts / DTOs / Result Pattern)` ➔ `State & Hooks` ➔ `UI Components` ➔ `Integration & End-to-End Tests`.
2. **Zero Task Ambiguity**: Every individual task must declare:
   - Target files to create/modify (`file:///path/to/file`).
   - The concrete function, type, or component to implement.
   - The deterministic verification step (e.g. terminal test command, expected return value).
3. **Atomic Task Sizing**: Keep tasks small and self-contained (1–2 hours maximum of implementation effort). If a task takes longer, break it down further.
4. **Verifiable Acceptance Criteria (AC)**: Use Gherkin (`Given / When / Then`) or explicit checklists for every User Story.

---

## 2. Epic Structure Template

```markdown
# Epic [ID]: [Epic Name]

## Description
High-level explanation of the business value and module boundary.

## User Stories

### Story [ID.1]: [User Story Title]
**As a** [user persona]
**I want to** [action]
**So that** [business benefit]

#### Acceptance Criteria
- [ ] **Given** an authenticated user with valid session, **when** they navigate to `/orders`, **then** the first 10 orders render within 300ms.
- [ ] **Given** a network failure, **when** fetching orders fails, **then** a descriptive error alert appears with a retry action button.
```

---

## 3. Granular Developer Sprint Plan (1x1 Tasks)

```markdown
# Sprint Plan: [Feature / Milestone]

## Phase 1: Data Contracts & Persistence
- [ ] **Task 1.1**: Define PostgreSQL Drizzle schema for `orders` and `order_items` in `src/db/schema/orders.ts`.
  - *Verification*: Run `bun run drizzle-kit generate` and confirm physical SQL migration in `drizzle/`.
- [ ] **Task 1.2**: Write unit tests for Order domain types and DTO mappers in `src/modules/Orders/types/orders.test.ts`.

## Phase 2: Domain Services & Hooks
- [ ] **Task 2.1**: Implement `fetchOrders` service with Result Pattern in `src/modules/Orders/services/orders.service.ts`.
  - *Verification*: `bun test src/modules/Orders/services/orders.service.test.ts` passes.
- [ ] **Task 2.2**: Implement `useOrders` custom hook with TanStack Query in `src/modules/Orders/hooks/useOrders.ts`.

## Phase 3: UI Components & Styling
- [ ] **Task 3.1**: Create `OrderCard` component and `OrderCard.module.css` with BEM tokens in `src/modules/Orders/components/OrderCard/`.
  - *Verification*: Renders order status pill and price with design tokens.
- [ ] **Task 3.2**: Assemble `OrderHistory` screen in `src/modules/Orders/components/OrderHistory/`.

## Phase 4: Integration Verification
- [ ] **Task 4.1**: Create MSW handlers in `src/modules/Orders/__tests__/orders.msw.ts`.
- [ ] **Task 4.2**: Run full verification gate (`bun run biome:check && bun test`).
```
