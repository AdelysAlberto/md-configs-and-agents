---
name: plan
description: Plan Mode protocol for thorough exploration, read-only investigation, requirement clarification, and generating or updating structured PLAN.md documents with mandatory execution pause for user approval.
license: MIT
compatibility: opencode
metadata:
  domain: workflow
  mode: planning
---

# Plan Mode Protocol

When Plan Mode is activated, the objective is to plan, explore, and design without altering the project or executing side-effect commands. Follow this strict workflow:

## Phase 1 — Explore & Investigate (Read-Only)
- **Local Inspection**: Read relevant codebase and state using strictly read-only tools and commands without side effects (`grep`, `find`, `git status`, reading files).
- **External Research**: If the task involves third-party libraries, external APIs, or specific version syntax, search official documentation before planning.
- **Golden Rule**: DO NOT edit or create executable code. DO NOT run commands that modify the workspace or environment.

## Phase 2 — Clarify (Only when necessary)
- If there are ambiguities regarding requirements or key architectural decisions, formulate concise questions (maximum 3-4 direct questions).
- If the goal and context are clear, skip this phase and proceed directly to writing the plan.

## Phase 3 — Draft PLAN.md
- Generate or update the `PLAN.md` file in the workspace root adhering strictly to this schema:

```markdown
# Plan: <Clear & Descriptive Title>

## Goal
What is being changed, exact scope, and technical justification.

## Context & Research
Relevant codebase findings or external documentation reviewed.

## Steps
1. [ ] Step 1 (involved files and concrete action)
2. [ ] Step 2 (involved files and concrete action)

## Verification
Commands or tests to deterministically validate each step.

## Notes & Trade-offs
Risks, dependencies, or open technical decisions.
```

## Phase 4 — Mandatory Stop for User Approval
- **Mandatory Execution Pause**: Halt execution immediately after writing or updating `PLAN.md`.
- Wait for the user's explicit approval before touching any source file or executing mutating commands.
