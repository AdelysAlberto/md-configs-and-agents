---
name: product-requirements
description: Product Requirements Document (PRD) creation, Product Briefs, user journey mapping, MoSCoW prioritization, and functional/non-functional specifications. Use when defining new features, scopes, or writing PRDs.
license: MIT
compatibility: opencode
metadata:
  domain: product
  framework: prd-moscow
---

# Product Requirements Document (PRD) Standards

Master guide for creating rigorous, comprehensive, and unambiguous Product Requirements Documents (PRDs) and Product Briefs.

## 1. Core Product Invariants

1. **Zero Ambiguity**: Never leave edge cases, error states, or empty states unspecified. Every feature must declare what happens on network failure, empty data, and authorization denial.
2. **MoSCoW Prioritization**: Every requirement must be classified into:
   - **Must Have (P0)**: Absolute blocker for release; system cannot function without it.
   - **Should Have (P1)**: High-value feature; critical for full user journey.
   - **Could Have (P2)**: Desirable enhancement if time permits.
   - **Won't Have (P3)**: Explicitly out of scope for the current milestone/version.
3. **Traceability to User Value**: Every functional requirement must tie directly back to an identified user persona pain point or business objective.
4. **Deliverable**: Generated PRD resides in `artifacts/prd.md`.

---

## 2. PRD Standard Template (`artifacts/prd.md`)

```markdown
# Product Requirements Document: [Feature / Product Name]

## 1. Executive Summary & Vision
Concise overview of what is being built, for whom, and what core metric it influences.

## 2. Goals & Success Metrics
- **Business Goal**: (e.g. Increase route completion rate by 25%).
- **User Goal**: (e.g. Reduce navigation setup time to under 10 seconds).
- **KPI / Measurement**: Concrete analytical events to track.

## 3. User Personas & Core Journeys
- **Primary Persona**: Detailed actor profile.
- **End-to-End User Flow**:
  1. User triggers action A.
  2. System validates input and displays state B.
  3. User receives feedback C.

## 4. Functional Requirements (MoSCoW)

### Must Have (P0)
- **FR-01**: [Requirement name]. System must allow user to [action].
  - *Input*: [parameters]
  - *Output*: [result]
  - *Error States*: [behavior on failure]

### Should Have (P1)
- **FR-02**: [Requirement name].

### Out of Scope (Won't Have)
- Explicit list of features deliberately deferred to prevent scope creep.

## 5. Non-Functional Requirements (NFR)
- **Performance**: P95 latency under 200ms for all API responses; 60fps UI transitions.
- **Security**: OWASP compliant; HTTPS; zero sensitive data in client storage.
- **Reliability & Offline**: Graceful fallback when network connection is severed.

## 6. Edge Cases & Exception Handling
- What happens when location permissions are denied?
- What happens when the route calculation times out?
```
