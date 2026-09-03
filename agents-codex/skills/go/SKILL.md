---
name: go
description: Runtime decision. Use when working on files matching: **/*.go.
---

# Go Rules

## ⚡ 4. Non-Negotiable Directives (Quick Reference)

1. **Vertical Slicing**: Group all business domain code by module inside `src/modules/<FeatureName>/`.
2. **Result Pattern**: Never throw exceptions from services. Return explicit result objects (`{ success, value/error }`).
3. **Pre-Commit Verification**: Always run linter code.
4. **Simplification First**: Fixes should make the system simpler, not more complex. Prefer removing or consolidating code over adding a new layer, flag, or special case. If a fix grows the system's surface area, look for the version that shrinks it.

## Domain Rule Routing

| Trigger | Rule |
| --- | --- |
| HTTP / API | `rules/go-http.rules.md` |
| Testing | `rules/go-testing.rules.md` |
| Database | `rules/go-database.rules.md` |
| Concurrency | `rules/go-concurrency.rules.md` |
| Configuration | `rules/go-config.rules.md` |
