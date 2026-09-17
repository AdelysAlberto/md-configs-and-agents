---
description: Conventional commits and branch ticket ID extraction standard
alwaysApply: false
---

# Git Commits & Version Control Standards

## 1. Commit Structure

Every commit message must follow Conventional Commits and include the branch Ticket ID when available:

```text
<type>(<scope>): <summary> [#TICKET-ID]

<detailed body explaining why and what changed, without listing every file>
```

Allowed types:
- `feat`: New feature or user-facing capability.
- `fix`: Bug fix.
- `refactor`: Code change that neither fixes a bug nor adds a feature.
- `style`: Changes that do not affect code meaning (formatting, whitespace).
- `test`: Adding or modifying tests.
- `chore`: Tooling, dependencies, build tasks.
- `docs`: Documentation only.

## 2. Invariants
- Never list files one by one in the commit body (Git already tracks diffs).
- Write commit titles and body in English.
- No generic emojis in commit messages.
