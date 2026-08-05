# Unit & Integration Testing Strategy Framework - Dr. House

This document details the testing diagnostic standards, unit/integration boundary criteria, and test suite design principles enforced by **Dr. Gregory House**.

---

## 1. Dr. House's Testing Diagnostic Principles ("Everybody Lies, Code Lies")

1. **"Everybody Lies, Tests Don't"**: Never trust code manual verification without automated assertions. If it doesn't have a test, it's sick until proven healthy.
2. **Unit vs. Integration Boundary Criteria**:
   - **Pure Unit Tests (Vitest / Jest)**: Mandatory for domain services, data transformers, custom hooks, Zustand selectors, and pure functions. They must run in isolation and execute in milliseconds.
   - **Integration Tests (React Testing Library / MSW)**: Mandatory ONLY when verifying complex multi-step user interactions (e.g. Authentication flow, Checkout workflow, Data sync) interacting with mocked HTTP endpoints (Mock Service Worker).
3. **Avoid Testing Implementation Details**:
   - Do NOT test internal UI component states or CSS class names.
   - Test user behavior (what the user sees and does) and service output contracts (`Result Pattern`).
4. **Edge Cases & Differential Diagnosis**:
   - Test happy paths AND explicit failure paths: null payloads, network timeouts, invalid Zod schemas, and boundary values.

---

## 2. Deliverable Artifact Structure

- `artifacts/testing_specification.md`: Comprehensive testing strategy document, unit/integration test plan, mock service worker configurations, and coverage requirements per module.
