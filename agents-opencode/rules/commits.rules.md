<!-- Adapted for Opencode -->
---
applyTo: "src/**"
---
# Commits & Verification Checklist

## Conventional Commits

All commits must follow **Conventional Commits** format:

```
<type>(<scope>): <subject>
```

### Allowed Types

- `feat` — New feature
- `fix` — Bug fix
- `docs` — Documentation changes
- `style` — Formatting, missing semicolons, missing braces (no code change)
- `refactor` — Code refactoring
- `test` — Adding missing tests
- `chore` — Routine maintenance updates

### Example Commits

```bash
# ✅ CORRECT
git commit -m "feat(auth): add login with Google OAuth"

# ✅ CORRECT
git commit -m "fix(api): resolve race condition in user fetch"

# ❌ WRONG
git commit -m "changed some stuff"

# ❌ WRONG
git commit -m "fix: thing"
```

## Pre-Commit Verification Checklist

Always run **before** creating a commit:

```bash
pnpm fix          # Biome formatting fix
pnpm tsc --noEmit  # Type check
pnpm test         # Run unit/integration tests
```

If any of the above fail, fix the issues before committing.

## Verification Checklist (Per Feature)

Before marking a task as completed, verify:

- [ ] Code follows functional programming principles (no `class`, `this`, `extends`)
- [ ] TypeScript types are correct (`pnpm tsc --noEmit` passes)
- [ ] CSS uses CSS Modules with design tokens (no hardcoded values)
- [ ] Result Pattern is used for all async operations
- [ ] Zustand selector pattern is used (no store destructuring)
- [ ] i18n keys are used for all user-facing text
- [ ] Conventional Commit message format is correct
- [ ] No console.log statements in production code
- [ ] No `any` types used (specific types or `unknown` only)
- [ ] Barrel files (`index.ts`) exist for all major directories
- [ ] Tests pass (`pnpm test` with coverage threshold met)