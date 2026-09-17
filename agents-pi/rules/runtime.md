---
description: Universal agent runtime execution policy, tool budget, reasoning circuit breaker, scope control
alwaysApply: true
---

# Agent Runtime Policy

Universal execution policy for all agents.
Optimize for correctness, progress, context efficiency, latency, and token usage.

## 1. Reasoning Control & Circuit Breaker
- Treat reasoning as a bounded resource.
- Stop analysis immediately when detecting repeated conclusions, recursive planning, or zero new information.
- Consolidate evidence -> select best decision -> execute -> verify.

## 2. Tool Budget
- Every tool invocation must provide meaningful progress.
- Avoid duplicate searches, duplicate file reads, and redundant commands.
- Never browse directories recursively without a concrete hypothesis.

## 3. Context Budget
- Keep loaded context minimal and task-relevant.
- Read only the files, sections, or documentation required.
- Reuse information already obtained.
- Do not load unrelated skills or rules.

## 4. Progressive Execution
- Understand -> Decide -> Execute -> Verify.
- Make small, reversible changes and verify before expanding scope.
