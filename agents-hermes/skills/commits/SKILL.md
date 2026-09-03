---
name: commits
description: Conventional commits and Git workflow standards. Use when working on files matching: **.
metadata:
  hermes:
    tags: [team-pinky, coding-standards]
    category: engineering
---

# Git Commits

## Format

Every commit MUST follow this format:

```
#<BRANCH_ID> <type>(<scope>): <description>
```

The `#<BRANCH_ID>` is the numeric ID extracted from the branch name.

## Commit Process (Required — Every Time)

```bash
# Step 1: Get branch ID
git branch | grep '\*'
# e.g. "* feature/249517" → ID is 249517

# Step 2: Review staged changes
git status
git diff --cached

# Step 3: Commit with ID + conventional message
git commit -m "#249517 feat(account): add balance display to account summary"
```

## Commit Types

| Type | When to Use |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting (no logic change) |
| `refactor` | Code restructure (no bug fix, no feature) |
| `test` | Adding or fixing tests |
| `chore` | Build tools, config, dependencies |
| `perf` | Performance improvement |
| `ci` | CI/CD pipeline changes |
| `build` | Build system or external dependency changes |

## Examples

```bash
git commit -m "#249517 feat(auth): add Google OAuth login support"
git commit -m "#249517 fix(api): handle token expiry during account fetch"
git commit -m "#249517 chore(biome): update biome to v2.4.6"
git commit -m "#249517 refactor(hooks): extract useAccountBalance from AccountSummary"
git commit -m "#249517 test(components): add BaseButton unit tests"
```

## Before Every Commit

```bash
pnpm fix   # Run Biome auto-fixes
pnpm check # Verify no remaining issues
```

Do NOT use `--no-verify` to bypass pre-commit hooks unless authorized.
