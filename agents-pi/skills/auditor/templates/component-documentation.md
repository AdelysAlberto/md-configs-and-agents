# {{ArtifactType}}: {{ArtifactName}}

## 1. Summary and Metadata

- **Artifact Name**: `{{ArtifactName}}`
- **Type**: {{ArtifactType}}
- **Source Location**: `{{SourceLocation}}`
- **Entry Points**: {{EntryPoints}}
- **Module / Package**: {{PackageName}}
- **Technology Stack**: {{TechnologyStack}}
- **Version (if available)**: {{Version}}

---

## 2. Purpose and Context

### Functional Purpose

Describe the purpose of the artifact from a business and functional perspective.

### Technical Purpose

Describe the technical responsibility and role within the system.

### Why It Exists

Explain what problem it solves and why the implementation exists.

---

## 3. Scope

### In Scope

List responsibilities included within this artifact.

- Responsibility 1
- Responsibility 2
- Responsibility 3

### Out of Scope

List responsibilities intentionally excluded.

- Responsibility 1
- Responsibility 2
- Responsibility 3

---

## 4. Business Context

### Business Capability

Explain the business capability enabled by this artifact.

### Business Rules

Document any inferred business rules.

### Business Dependencies

Describe dependencies on external actors, workflows or processes.

---

## 5. Consumers and Usage

### Consumers

Identify:

- Components
- Modules
- Features
- Services
- External applications

that depend on this artifact.

### Typical Usage Scenarios

#### Scenario 1

Description.

#### Scenario 2

Description.

#### Scenario 3

Description.

---

## 6. Architecture Analysis

### Responsibilities

Describe the responsibilities currently implemented.

### Separation of Concerns

Evaluate whether responsibilities are correctly distributed.

### Architectural Role

Explain where this artifact sits in the system architecture.

Examples:

- Presentation Layer
- Application Layer
- Domain Layer
- Infrastructure Layer
- Shared Library
- Cross-Cutting Concern

### Design Decisions Identified

List significant technical decisions inferred from the implementation.

---

## 7. Interfaces

### Inputs

| Name | Type | Required | Description |
|--------|--------|--------|--------|
| {{Input}} | {{Type}} | Yes/No | Description |

### Outputs

| Name | Type | Description |
|--------|--------|--------|
| {{Output}} | {{Type}} | Description |

### Callbacks / Events

| Event | Trigger Condition | Expected Behavior |
|--------|--------|--------|
| {{Event}} | {{Condition}} | {{Behavior}} |

---

## 8. Dependencies

### Internal Dependencies

List internal modules, services, utilities and components.

### External Dependencies

List third-party libraries and frameworks.

### Dependency Diagram

```text
Artifact
├── Dependency A
├── Dependency B
└── Dependency C
```

---

## 9. Technical Flow

### Execution Flow

Describe the complete execution flow.

```text
[Start]
   │
   ▼
[Step 1]
   │
   ▼
[Step 2]
   │
   ▼
[Result]
```

### State Changes

Document relevant state mutations and side effects.

---

## 10. Validation Rules

Document all validations found.

### Validation Rule 1

- Condition:
- Expected Outcome:
- Failure Outcome:

### Validation Rule 2

- Condition:
- Expected Outcome:
- Failure Outcome:

---

## 11. Error Handling

| Scenario | Detection | Outcome |
|--------|--------|--------|
| Error Scenario | Detection Logic | Error Handling |

---

## 12. Design Patterns Identified

### Pattern

#### Name

{{PatternName}}

#### Evidence

Code locations or implementation details.

#### Purpose

Reason for usage.

#### Benefits

Observed benefits.

#### Limitations

Observed limitations.

---

## 13. Architectural Findings

### Positive Findings

#### Finding

##### Observation

Description.

##### Impact

Why it is beneficial.

---

### Findings Requiring Attention

#### Finding

##### Classification

- Improvement
- Bad Practice
- Technical Debt
- Anti-Pattern
- Technical Risk

##### Evidence

Observed implementation.

##### Impact

Observed consequence.

##### Severity

- Critical
- High
- Medium
- Low

##### Recommendation

Practical recommendation.

---

## 14. Technical Debt Analysis

### Critical

List critical debt items.

### High

List high-priority debt items.

### Medium

List medium-priority debt items.

### Low

List low-priority debt items.

---

## 15. Risk Assessment

### Functional Risks

List risks affecting business behavior.

### Technical Risks

List risks affecting architecture, performance, scalability or reliability.

---

## 16. Change Impact Analysis

### Direct Dependencies

List immediate consumers.

### Indirect Dependencies

List indirectly affected areas.

### Change Risk

- Low
- Medium
- High

### Safe Refactoring Recommendations

Recommended migration path.

---

## 17. Documentation Assessment

### Existing Documentation

List related documentation discovered.

### Consistency Review

Identify gaps or inconsistencies.

### Missing Documentation

List relevant missing artifacts.

---

## 18. Quality Assessment

| Area | Score | Notes |
|--------|--------|--------|
| Architecture | X/10 | Notes |
| Code Quality | X/10 | Notes |
| Documentation | X/10 | Notes |
| Testing | X/10 | Notes |
| Maintainability | X/10 | Notes |
| Scalability | X/10 | Notes |

---

## 19. Executive Summary

Provide a concise summary containing:

- Main strengths.
- Main weaknesses.
- Critical risks.
- Recommended next actions.

---

# Technical Debt and Improvement Backlog

## Critical

- [ ] Action item

## High

- [ ] Action item

## Medium

- [ ] Action item

## Low

- [ ] Action item

---

# Final Assessment

Summarize:

1. What the artifact does.
2. Why it exists.
3. How it works.
4. Main risks identified.
5. Main technical debt identified.
6. Recommended evolution path.