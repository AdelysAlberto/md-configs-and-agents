---
description: 'Guidelines for creating summary documentation'
applyTo: '**'
---

# Summary Documentation Guidelines

## When to Create Summaries

Only create final summary `.md` files in specific cases:

### ✅ Create Summaries For:
- **Major features**: New significant functionality additions (e.g., new game provider integration, major refactoring)
- **Breaking changes**: Changes that affect multiple components or the application architecture
- **Important fixes**: Critical bug fixes that impact functionality
- **Migration guides**: Database migrations, dependency upgrades, or major workflow changes
- **Implementation decisions**: Complex architectural decisions that need documentation

### ❌ Do NOT Create Summaries For:
- Bug fixes (unless critical)
- Minor UI updates or styling changes
- Small code refactors
- Dependency updates (unless breaking)
- Routine maintenance tasks
- Configuration tweaks

## Location

**All summary files must be created in the `docs/` folder** at the root of the project:

```
docs/
```

## File Naming Convention

Use descriptive names following the pattern:
- `FEATURE_NAME_SUMMARY.md` - For new features
- `REFACTOR_NAME_SUMMARY.md` - For major refactors
- `MIGRATION_NAME.md` - For migrations or breaking changes

Example: `HUSKY_LINT_SETUP.md`, `AUTH_MODULE_REFACTOR.md`

## Summary Contents

Include:
1. **Overview**: Brief description of what was done
2. **Why**: Reason or motivation for the change
3. **Changes**: List of key modifications
4. **Files Modified**: Files affected by the change
5. **Impact**: How it affects the project/team
6. **Next Steps** (if applicable): Future work related to this

## Example

```markdown
# Feature: Husky Pre-Push Linting

## Overview
Added Husky with pre-push hook to run `pnpm lint` automatically before allowing code to be pushed.

## Why
To prevent code with linting errors from being pushed to the repository and ensure code quality standards.

## Changes
- Added Husky configuration
- Created `.husky/pre-push` hook
- Updated `package.json` with prepare script

## Files Modified
- `.husky/pre-push` (new)
- `package.json`

## Impact
All developers now have automatic linting validation before push.
```
