---
name: engineering-invariants
description: Engineering invariants. Use when working on files matching: src/**.
metadata:
  hermes:
    tags: [team-pinky, coding-standards]
    category: engineering
---

# Engineering Invariants

> Universal engineering standards for all agents.
> Apply these principles regardless of language, framework, or technology.

## Core Principles

1. **Correctness First**
   Prioritize correctness, maintainability, security, and reliability over implementation speed.

2. **Simplicity First**
   Prefer the simplest solution that satisfies the requirements. Remove unnecessary complexity before adding abstractions.

3. **Single Responsibility**
   Keep modules, functions, components, and services focused on one clear responsibility.

4. **DRY with Judgment**
   Eliminate meaningful duplication, but do not abstract coincidental similarities prematurely.

5. **Explicit Design**
   Prefer explicit dependencies, data flow, contracts, side effects, and boundaries over hidden behavior.

6. **Separation of Concerns**
   Keep domain logic, infrastructure, presentation, persistence, and external integrations appropriately separated.

7. **Dependency Direction**
   Protect architectural boundaries. High-level business rules must not become unnecessarily coupled to low-level implementation details.

8. **Composition over Inheritance**
   Prefer composition and well-defined contracts over inheritance-heavy designs unless the language or domain clearly benefits from inheritance.

9. **No Known Anti-Patterns**
   Do not introduce known anti-patterns, unnecessary global state, circular dependencies, speculative abstractions, or excessive coupling.

10. **Existing Architecture First**
    Understand the existing architecture, conventions, and constraints before introducing new patterns or structural changes.

11. **Pattern by Need**
    Use design patterns only when they solve an actual problem. Never introduce patterns for their own sake.

12. **Change Minimization**
    Modify the smallest necessary surface area while preserving correctness and architectural consistency.

13. **Type and Contract Safety**
    Prefer strong typing, explicit contracts, boundary validation, and compile-time guarantees when supported by the language.

14. **Explicit Error Handling**
    Errors must be explicit, predictable, diagnosable, and handled at the appropriate architectural boundary. Never silently swallow failures.

15. **Testability**
    Design important business behavior so it can be verified independently and reliably.

16. **Security by Default**
    Treat external input as untrusted. Validate boundaries, minimize privileges, protect secrets, and avoid exposing sensitive information.

17. **Observability**
    Important failures and operationally relevant events must be diagnosable without exposing sensitive data.

## Technology & Evidence

 1. **Language and Framework Standards**
    Follow idiomatic practices, established conventions, and official best practices of the language and framework being used.

 2. **Official Sources First**
    When behavior, API usage, configuration, or library capabilities are uncertain, consult authoritative official documentation or specifications before reverse-engineering dependency internals.

 3. **Verify Before Completion**
    Validate the implementation using the project's appropriate formatter, linter, type checker, tests, build, or equivalent verification mechanisms before declaring the task complete.

 4. **Requirement Before Implementation**
    Do not introduce technical complexity to solve an unrequested or hypothetical problem. Implement the actual requirement first and expand scope only when evidence or explicit requirements justify it.

## Language & Technology Rules

Before writing or modifying code, identify the language and
technology involved and load the corresponding rules.

- JavaScript / TypeScript → `rules/javascript-typescript.rules.md`
- Go → `rules/go.rules.md`
- Python → `rules/python.rules.md`
- Other languages → load the corresponding language rule if available.

After loading the language rules, load only the framework,
architecture, domain, tooling, and testing rules relevant to the task.
