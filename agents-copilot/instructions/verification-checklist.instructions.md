---
description: 'Pre-completion deterministic verification checklist'
applyTo: '**'
---

# Pre-Completion Verification Checklist

Run through this checklist and execute deterministic verification before marking any task as done.

## 1. Code Quality & Invariants
- [ ] Strictly functional TypeScript (no `class`, no `this`, no `React.FC`).
- [ ] No `any` types used.
- [ ] Vertical slicing preserved: domain code in `src/modules/<FeatureName>/`.
- [ ] Result Pattern implemented in services (`{ success: true, data } | { success: false, error }`). Never throw exceptions.
- [ ] **Provider Adapter Pattern (DIP)**: Domain services (`src/modules/`) import only domain-agnostic contracts from `src/providers/<domain>/`, never vendor-specific functions/DTOs (e.g. `calculateValhallaRoute`, `ValhallaTrip`). Vendor implementations must reside in `src/providers/<domain>/providers/<vendor>/`.
- [ ] Zustand store accessed via atomic selectors or `useShallow` (no full store destructuring).
- [ ] CSS Modules used exclusively (`*.module.css`) with design tokens.
- [ ] All user-facing text wrapped in `t('key')` i18n keys.
- [ ] Exact dependency versions pinned in `package.json` (no `^` or `~`).
- [ ] **Mandatory Drizzle Migrations**: Every DB schema change (`src/db/schema.ts`) must have a corresponding physical SQL migration file in `drizzle/*.sql` and journal entry in `drizzle/meta/_journal.json` executed autonomously via `runMigrations()`. Never rely on external MCP or manual SQL execution.

## 2. Interface & DTO UX Contract Verification
- [ ] **Cross-reference DTO fields against Functional/UX Specs**: Verify every UI component data requirement in `artifacts/functional_specs/*.md` against the DTO return type.
- [ ] **No Overlapping/Duplicate Properties**: Ensure distinct UI state needs have explicit separate properties (e.g. `currentStreetName` for active road vs `nextStreetName` for turn maneuver target).
- [ ] **Third-Party API Payload Inspection**: Verify edge cases in raw third-party responses (e.g. optional/missing fields in intermediate maneuvers) and map fallbacks deterministically.

---

## 2. Deterministic Verification Gate (Terminal Commands)
Execute in the terminal and ensure 100% pass:

```bash
bun run biome:check && bun run check && bun test
# OR (pnpm)
pnpm fix && pnpm tsc --noEmit && pnpm test
```
