---
name: runtime
description: Runtime decision. Use when working on files matching: src/**.
metadata:
  hermes:
    tags: [team-pinky, coding-standards]
    category: engineering
---

# Agent Runtime Policy

> Universal execution policy for all agents.
> Optimize for correctness, progress, context efficiency, latency, and token usage.
> These rules apply to every task unless explicitly overridden.

## 1. Reasoning Control

- Treat reasoning as a bounded resource, not an infinite process.
- Reason only while analysis can materially improve the outcome.
- Execute as soon as sufficient confidence is reached.
- Never solve the same problem twice.
- Never reconsider a validated decision without new evidence.
- Prefer a good, verified decision over exhaustive exploration.
- Do not optimize indefinitely when the requested objective is already satisfied.

### Reasoning Circuit Breaker

Immediately stop analysis when detecting:

- repeated reasoning or conclusions;
- recursive planning;
- equivalent alternatives being reconsidered;
- analysis that produces no new information;
- repeated attempts using the same strategy.

When triggered:

1. Consolidate current evidence.
2. Select the best available decision.
3. Execute.
4. Verify.
5. Continue only if verification exposes a real issue.

Never remain in a reasoning loop.

---

## 2. Search Strategy

Search with a specific question or hypothesis.

Prefer information sources in this order:

1. Official documentation.
2. Official specifications or standards.
3. Official source repository.
4. Project documentation and source code.
5. Installed dependency source.
6. Generated artifacts or indirect sources.

- Prefer authoritative documentation over reverse engineering.
- Do not inspect `node_modules` to discover how a library should be used when official documentation can answer it.
- Do not browse directories recursively without a concrete reason.
- Stop searching once sufficient evidence is obtained.
- Do not perform exhaustive searches when a reliable answer is already available.

---

## 3. Tool Budget

Every tool invocation must provide meaningful progress.

Before using a tool, determine whether:

- it is necessary;
- a previous result already answers the question;
- a cheaper or more targeted operation can achieve the same result.

Prefer the smallest operation that can answer the question.

Avoid:

- duplicate searches;
- duplicate file reads;
- repeated commands;
- unnecessary indexing;
- repeated documentation lookups;
- broad exploration when targeted inspection is sufficient.

If a strategy repeatedly produces no useful information, stop and change strategy.

---

## 4. Context Budget

Treat context as a finite and expensive resource.

- Keep loaded context minimal and task-relevant.
- Read only the files, sections, or documentation required.
- Prefer targeted retrieval over entire directories or large files.
- Reuse information already obtained.
- Never repeat established conclusions unnecessarily.
- Do not generate summaries unless they provide new value.
- Do not load unrelated skills, rules, dependencies, or documentation.
- Avoid carrying obsolete or irrelevant information forward.

Context expansion must be justified by the task.

---

## 5. Scope Control

Work within the smallest scope capable of solving the task.

- Start from the files and modules directly related to the request.
- Do not inspect the entire repository for a localized change.
- Do not modify unrelated code.
- Expand scope only when evidence shows it is necessary.
- Do not investigate hypothetical problems that are outside the requested objective.
- Preserve existing architecture and conventions unless the task requires changing them.

For localized tasks, repository-wide exploration is prohibited unless justified.

---

## 6. Progressive Execution

Use the execution cycle:

**Understand → Decide → Execute → Verify**

- Do not spend excessive time planning before making useful progress.
- Prefer small, reversible changes.
- Verify meaningful changes before expanding the scope.
- Use verification results as new evidence rather than restarting the entire analysis.

---

## 7. Progress Monitoring

Continuously evaluate whether the current strategy is producing progress.

If several consecutive actions:

- produce no meaningful information;
- repeat previously discovered information;
- fail to reduce uncertainty;
- or do not move the implementation forward;

then:

1. Stop the current strategy.
2. Identify why it is unproductive.
3. Select a more direct information source or approach.
4. Continue only if the new strategy has a clear purpose.

Never continue an unproductive search simply because it has already started.

---

## 8. Escalation

Increase reasoning depth and exploration only when justified by risk or complexity.

Escalate for:

- architecture changes;
- security-sensitive operations;
- data integrity;
- destructive or irreversible operations;
- complex cross-module dependencies;
- significant performance or scalability decisions;
- ambiguous requirements with materially different outcomes.

Routine tasks should use lightweight reasoning and targeted execution.

---

## 9. Verification

Verify outcomes, not assumptions.

After meaningful changes, verify:

- the requested behavior;
- consistency with project rules;
- obvious regressions;
- relevant tests or validation.

Verify once after a meaningful change.

Repeat verification only when new changes or new evidence justify it.

---

## 10. Completion

Stop when:

- the requested objective is fulfilled;
- the result has been appropriately verified;
- no known high-impact issue remains.

Do not continue exploring, refactoring, optimizing, or researching after completion unless explicitly required.

## Runtime Objective

**Use the minimum reasoning, tools, context, exploration, and tokens necessary to produce a correct, maintainable, verified result.**
