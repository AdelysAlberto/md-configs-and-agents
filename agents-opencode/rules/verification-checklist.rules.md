
# Verification Checklist

## Pre-Commit Verification

Always run the following before completing any technical task:

```bash
pnpm biome:check     # Biome formatting fix
pnpm biome:format    # biome format --write .
pnpm typecheck       # typecheck --noEmit
```

## Post-Task Verification Checklist

After completing any code implementation or technical task, and before delivering results to the user:

1. **Invoke Specialist Sub-Agent**:
   - Delegate code audit to **Tio Bob** (`@bob` / code review), **Jefe Gorgory** (`@gorgory` / security & code health), and **Sheldon Cooper** (`@sheldon` / architecture).

2. **Audit Criteria**:
   - Analyze modified code diff verifying Clean Architecture, Result Pattern, unit test coverage, zero unused endpoints, and best practices.
   - Verify zero regressions by checking type compilation (`pnpm typecheck --noEmit`), linter (`pnpm biome:check`), and unit tests (`pnpm test`).

3. **Results Delivery**:
   - Only after sub-agent approval, summarize findings in `walkthrough.md` and complete the task.

## Code Quality Non-Negotiables

- **Pure Functional Code**: Prohibit `class`, `this`, and OOP. Write pure functional TypeScript/JavaScript.
- **Vertical Slicing**: Group all business domain code by module inside `src/modules/<FeatureName>/`.
- **Result Pattern**: Never throw exceptions from services. Return explicit result objects (`{ success, value/error }`).
- **React Native Max File Length (< 250 LOC)**: No screen or component file may exceed 250 LOC. Screens must act strictly as orchestrators (< 100 LOC).
- **React Native DRY ScreenLayout**: Never duplicate `SafeAreaView`, `LinearGradient`, headers, or back buttons. Wrap screens in reusable `<ScreenLayout>`.
- **React Native Logic Separation**: Extract non-trivial state, queries, and effects into dedicated custom hooks (`use[Screen].ts`).
- **React Native Safe Areas & Media**: Strictly use `react-native-safe-area-context` and `expo-image`.
- **Zustand Selector Hygiene**: Never destructure entire global Zustand stores. Use `useShallow` or atomic selectors.
- **Styles**: Use CSS Modules exclusively (`*.module.css`) for web, and StyleSheet/Design Tokens for mobile. No inline styles or TailwindCSS unless explicitly instructed.
- **Internationalization**: All user-facing text must use `t('key')` keys.
- **TanStack Query & Custom Hooks**: All queries and mutations must be encapsulated in dedicated custom hooks with isolated loading components.
- **Mandatory Bruno Collections**: Every backend endpoint created or modified must have a corresponding `.bru` collection file with tests and docs.
- **Zero Deprecated APIs**: No deprecated methods or obsolete libraries allowed.
- **Simplification First**: Fixes should make the system simpler, not more complex. Prefer removing or consolidating code over adding a new layer, flag, or special case.
