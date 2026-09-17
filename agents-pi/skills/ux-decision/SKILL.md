---
name: ux-decision
description: UX reasoning and decision-making framework. Problem framing, premise interrogation, state completeness sweep, blindspot detection, accessibility behavior, content design, and evidence-based critique. Use when designing, evaluating, or reviewing any user experience decision.
license: MIT
compatibility: opencode
metadata:
  domain: ux
  version: "1.0.0"
---

# UX Decision

Think before designing. Evaluate after designing. Every decision grounded in evidence, not taste.

---

## Pre-Design: Frame the Real Problem

### Separate Request from Problem

"Build X" is a request, not a problem statement. Before designing:

- What is the triggering request or symptom?
- Who is the user or actor affected?
- What outcome are they trying to achieve?
- What evidence says this problem is real?
- What constraints are real versus merely inherited?
- What assumptions are being treated as requirements?
- What would change if we did nothing?

When evidence is absent, say so. Do not manufacture a user need from a stakeholder preference.

### Challenge the Premise

Probe the highest-risk assumptions, not every possible question:

- What user problem does this solve, and what evidence supports it?
- Are we solving a symptom rather than the cause?
- What simpler solution (no AI, no new component, no new flow) could achieve the same outcome?
- Which users or contexts make this direction fail?
- What would make us reverse this decision later?
- What incentives or business constraints may be distorting the UX premise?

**Intensity levels** (infer from the request):
- "Walk me through it" = guide
- "Challenge this" = challenge
- "Tear this apart" / "red team it" = aggressive scrutiny

More intensity = stronger skepticism, not more words.

---

## State Completeness (Sweep by Transition)

Do NOT produce a generic checklist. For each meaningful user action or system transition, ask what the user sees and can do:

| Transition | Question |
|---|---|
| **Before data** | What does the user see before anything loads? |
| **Loading / in progress** | Is progress communicated? Can the user cancel? Is duplicate submission prevented? |
| **Empty** | Nothing exists yet. Is there guidance to start? |
| **Partial success** | Only part worked. What shows? How to complete? |
| **Invalid input** | Is validation inline and immediate? Does it explain how to fix? |
| **Permission blocked** | User lacks access. Is the reason clear? Is there a path forward? |
| **Stale data** | Data changed or expired. Does the UI reflect it? How to refresh? |
| **Network/service failure** | Connection lost or API failed. Is there a retry? Is entered data preserved? |
| **Timeout** | Operation took too long. Is there feedback? Can the user retry? |
| **Interruption** | User leaves mid-flow, switches apps, or closes. Is progress saved? Can they resume? |
| **Success** | Confirmation shown? Next step clear? State persisted correctly? |

### Anti-Inflation Rule

Do not create a separate visual state when the existing system behavior already handles the condition. Reuse documented component states when possible.

### Priority

States whose absence can **strand a user**, **lose work**, **create false confidence**, or **produce inconsistent implementation** come first.

---

## Blindspot Detection

Look outside the frame the team is already using. Consider only dimensions relevant to the work:

- **People**: Different abilities, familiarity, language, age, roles, permissions, support needs.
- **Context**: Interruption, urgency, shared devices, travel, connectivity, time zones, environmental constraints.
- **Consequences**: Policy, privacy, trust, consent, safety, financial impact.
- **Dependencies**: Upstream and downstream services.
- **Multi-user**: Organizational behavior, concurrent use, role-based views.
- **Lifecycle**: What happens after the happy path ends.
- **Data edge cases**: Unusual but plausible data, inventory, account, or entitlement conditions.
- **Support burden**: Recovery and support cost.

### Priority

Rank blind spots by **likelihood x consequence x cost of discovering late**. Separate a true blind spot from a state already handled in the design.

Do NOT dump every imaginable edge case. Surface the few omissions most likely to change the design.

---

## Accessibility (Behavior, Not Just Appearance)

Review only the dimensions relevant to the work:

- **Semantic structure**: Proper heading hierarchy, landmarks, programmatic relationships.
- **Accessible names**: Labels, instructions, status, errors — all programmatically associated.
- **Keyboard operation**: Every interactive element reachable and operable via keyboard. Visible focus indicator.
- **Focus management**: Focus moves correctly after navigation, validation, async updates, dialogs, and errors.
- **Screen reader**: Meaningful dynamic changes announced. Reading and interaction order logical.
- **Contrast**: Verify actual values (4.5:1 normal text, 3:1 large text, 3:1 UI components).
- **Target size**: Minimum touch/click targets met (44x44pt iOS, 48x48dp Android, 24x24px web minimum).
- **Zoom/reflow**: Content remains usable at 200% zoom. No horizontal scroll at 320px viewport.
- **Motion**: Respect `prefers-reduced-motion`. No flashing content.
- **Cognitive load**: Error prevention, clear recovery, no time pressure without alternative.

### Critical Rule

Never claim WCAG or 508 compliance from visual or code inspection alone. Static review reveals missing intent; it does not prove runtime accessibility. State what still needs to be verified.

---

## Content Design

Words are part of the interaction, not decoration added after the design.

### Review Checklist

- **Action clarity**: Does the label describe what happens when activated?
- **Terminology consistency**: Same concept = same word everywhere.
- **Information timing**: Shown at the moment it is needed, not before.
- **Error language**: Explains what happened AND how to recover. No blame, no vague "something went wrong".
- **Empty state language**: Direction, not mood. Guide the user to the first action.
- **Confirmation language**: Proportional to consequence. Deleting account ≠ saving a draft.
- **No jargon**: Technical or organizational terms not exposed to users unless they are domain language the user knows.
- **No coercion**: No dark patterns, false urgency, or misleading copy.
- **Translation-safe**: Strings must survive translation, expansion, dynamic values, and assistive technology.

### Rewrite Rule

Preserve factual meaning, legal requirements, and uncertainty. Do not make a message friendlier by making it less precise. For high-consequence actions, **clarity wins over personality**.

---

## Evidence-Based Critique

Review the work against its **context and intent**, not personal taste.

### How to Critique

1. **Establish the basis**: Inspect the artifact, design-system patterns, accessibility rules, constraints, and prior decisions.
2. **Apply relevant lenses**: user goal, evidence, hierarchy, interaction behavior, state completeness, content, accessibility, system fit, engineering dependencies.
3. **Point to specific behavior**: Name the element, explain the consequence, recommend a direction.

### Contrast

Bad:
> Improve the visual hierarchy, make the CTA more prominent, simplify the form, and consider accessibility.

Good:
> The primary action appears available before the required travel date is valid. That creates a false affordance: the user can try to continue before the form is ready. Keep the action unavailable until the date is valid, or surface validation early enough that the fix is obvious before submit.

### Priority Order

1. **Blocks task completion** (user stranded, data lost)
2. **Causes exclusion** (accessibility barrier)
3. **Creates loss** (financial, data, trust)
4. **Causes misunderstanding** (wrong action taken)
5. **Polish** (visual refinement, consistency)

When the work is strong, spend fewer words validating it. Do not manufacture issues.

---

## Guardrails

- Never invent user evidence to support a critique or justify a design.
- Never claim accessibility compliance from visual inspection.
- Never prefer novelty over an established system pattern without evidence.
- Do not turn routine design work into a discovery exercise when the user and task are clear.
- Do not produce generic checklists disconnected from the actual product.
- Separate known, inferred, assumed, unknown, and conflicted information when the distinction matters.
