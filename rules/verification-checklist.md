# Post-Task Verification Checklist

**Run this after EVERY task. Do not mark work as complete until ALL items pass.**

## Code Cleanliness
- [ ] No unused variables or imports
- [ ] No `console.log`, `console.error`, `console.warn`
- [ ] No commented-out code
- [ ] No debug/temporary code

## TypeScript
- [ ] No `any` types introduced
- [ ] All new files are `.ts`/`.tsx` (no new `.jsx` files)
- [ ] `pnpm tsc --noEmit` passes without new errors

## Biome
```bash
pnpm fix   # biome check --fix ./src
```
- [ ] No linting errors
- [ ] Unused imports removed
- [ ] Node.js builtins use `node:` prefix

## Architecture
- [ ] Components under 200 lines
- [ ] No business logic in UI components
- [ ] Services return `TResponseRequest<T>` (no `throw`)
- [ ] No `useState + useEffect` data-fetching chains — use React Query `select`
- [ ] Zustand uses selectors: `useStore(state => state.field)`

## i18n
- [ ] No hardcoded user-facing strings
- [ ] New translation keys added to ALL language files
- [ ] No duplicate keys created

## Styling
- [ ] All styles in `.module.css` files
- [ ] No Tailwind, no inline styles (except dynamic values)
- [ ] Mobile-first: base styles for 320px, breakpoints for 768px+
- [ ] Touch targets ≥ 48px

## AntD Migration (when touching AntD files)
- [ ] All AntD imports removed from touched files
- [ ] Replaced with Base components from `@/components`
- [ ] Original `.jsx` deleted after `.tsx` conversion

## Security
- [ ] No tokens, user IDs, or sensitive data in console
- [ ] User inputs sanitized/validated
- [ ] HTTPS for all API calls

## Accessibility
- [ ] Semantic HTML elements used
- [ ] ARIA attributes added where needed
- [ ] Interactive elements keyboard-accessible

## Before Committing
```bash
pnpm fix           # Biome lint + format
pnpm tsc --noEmit  # TypeScript check
pnpm build         # Ensure build passes
```

Commit: `#TICKET type(scope): description` — in English, Conventional Commits format.
