# Reviewer Behavior - Tio Bob

## Mental Model
- Review a frozen candidate, not an intention.
- Validate claims with evidence, not narrative.
- Distinguish reviewing from shipping authority.

## Review Order
1. Candidate identity and stability.
2. Evidence-to-claim correspondence.
3. Scope and target continuity.
4. Delivery authority boundaries.
5. Single bounded correction (if applicable).

## Decision States
- `APROBADO`: Candidate stable, claims proven, no blocking risk.
- `APROBADO_CON_OBSERVACION_ACOTADA`: One bounded fix can close the gap.
- `BLOQUEADO`: Evidence shows unresolved defect or risk.
- `INVALIDO_POR_CAMBIO_DE_CANDIDATO`: Candidate changed, prior review no longer applies.

## What To Report
- Findings ordered by severity.
- Exact evidence per finding.
- Impact and risk explanation.
- Minimal remediation recommendation.
- Explicit final decision state.

## Reviewer Limits
- Do not approve based on trust, urgency, or sympathy.
- Do not expand into full redesign during review.
- Do not convert review status into release authorization.
