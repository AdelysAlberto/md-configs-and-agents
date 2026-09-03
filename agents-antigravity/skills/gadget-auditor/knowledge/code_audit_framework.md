# Code Hygiene & Dead Code Audit Framework - Inspector Gadget

This document details the cross-reference code auditing standards and static inspection rules enforced by **Inspector Gadget**.

---

## 1. Inspector Gadget's Audit Directives

1. **Unused API & Endpoint Detection**:
   - Scan all HTTP service definitions and API clients.
   - Perform cross-references (grep/AST) across the entire codebase (UI, hooks, components).
   - Report any exported endpoint or method that has 0 invocations in public/private portals.
2. **Semantic & HTTP Verb Discrepancies**:
   - Detect contradictions between the function name and the actual action (e.g. `deleteInvoicingGroup` calling `privateApi.editInvoicingGroup`).
   - Identify incorrect HTTP method usage (e.g. `POST` or `DELETE` calling edit/read endpoints).
3. **Dead Code & Unused Artifacts**:
   - Detect exported TypeScript types, utilities, hooks, or components that are genuinely never used.
   - Detect exposed sensitive endpoints (e.g. `/refresh-token`) without client consumption to reduce the attack surface.
4. **Pragmatic Risk Categorization**:
   - **CRITICAL**: Endpoints that execute destructive actions with incorrect verbs or severe exposure.
   - **WARNING**: Orphaned endpoints/services or functions with no calls in the UI.
   - **INFO**: Obsolete types or options to clean up.

---

## 2. Deliverable Artifact Structure

- `artifacts/code_audit.md`: Complete code health report, unused endpoint inventory, semantic contradictions, and cleanup recommendations.
