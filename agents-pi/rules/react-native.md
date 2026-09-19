---
description: React Native & mobile UI architecture constraints (< 250 LOC, DRY ScreenLayout, hook extraction)
globs:
  - "**/*.tsx"
  - "**/*.jsx"
scope:
  - "tool:edit(*.tsx)"
  - "tool:write(*.tsx)"
condition:
  - "SafeAreaView.*from ['\"]react-native['\"]"
---

# React Native & Mobile UI Architecture Invariants

Mandatory engineering and architecture standards for all React Native & Expo development.
These rules MUST be strictly enforced whenever creating, modifying, refactoring, or reviewing React Native code.

---

## 1. Absolute Constraints (Hard Limits)

- **Max File Length:** No React Native screen or component file may exceed **250 lines of code**.
- **Early Extraction at 200 LOC:** If a screen approaches **200 lines**, you MUST extract sub-components, custom hooks, or layouts BEFORE writing new features.
- **2-Screen Rule for JSX Extraction:** If repeated JSX (headers, gradients, back buttons, safe areas, wrappers, modal templates) appears across 2 or more screens, extracting it into a reusable layout or atom is **MANDATORY**.

---

## 2. DRY & Screen Layout Enforcement

- **Never repeat layout boilerplates:** Do NOT duplicate `LinearGradient`, `SafeAreaView`, root `View` containers, screen headers, or back navigation buttons directly inside screen files.
- **Enforce Compound/Wrapper Layouts:** All screens must be wrapped in a shared Layout component (e.g., `ScreenLayout`, `BaseLayout`).
  - Layout components must accept configurable slots or declarative props (e.g., `hasBack`, `hasGradient`, `title`, `headerRight`, `children`, `edges`).
  - Use sensible defaults so standard screens require minimal configuration.

### Expected Contract:

```tsx
// GOOD: Declarative & reusable screen layout (< 100 lines)
export function ProfileScreen() {
  const { user, isSaving, handleSave } = useProfileScreen();

  return (
    <ScreenLayout hasBack hasGradient title="Profile" rightAction={<SaveButton onPress={handleSave} loading={isSaving} />}>
      <ProfileAvatarSection user={user} />
      <ProfileFormSection user={user} />
    </ScreenLayout>
  );
}

// BAD: Duplicating SafeAreaView, LinearGradient, Header, and BackButton manually per file.
```

---

## 3. Structural Decomposition (Prevent 800+ Line Monsters)

When generating or modifying screens, strictly enforce the **Screen-Feature-Atom boundary**:

```text
src/ (or src/modules/<Feature>/)
├── screens/                 # Orchestrators ONLY (< 100 LOC). Sets up Layout, connects hooks, renders feature blocks.
├── components/
│   ├── features/ (or local) # Domain-specific blocks composing the screen (e.g., ProfileAvatarSection, GetawayCard).
│   └── ui/ (or common)      # Truly reusable design tokens and wrappers (ScreenLayout, Button, GradientBackground).
└── hooks/                   # Non-trivial state, derivations, effects, mutations (useProfileScreen.ts, useGetaways.ts).
```

### Logic & State Separation:
- **Zero Heavy Logic in Screen JSX**: Extract all non-trivial state machines, derivations, animations, form schemas, and API queries into dedicated custom hooks (`use[ScreenName].ts`).
- Screens must NEVER mix raw API calls, complex state reducers, and heavy UI in the same file.

---

## 4. Mobile Technical Invariants

- **Safe Areas**: NEVER import `SafeAreaView` from `react-native`. Always use `react-native-safe-area-context` (`useSafeAreaInsets` or `<SafeAreaView edges={...}>`).
- **Images**: Always use `expo-image` with explicit caching and sizing. Never use `react-native-fast-image` or raw unoptimized `<Image>`.
- **Lists**: For dynamic or large lists, use `@shopify/flash-list` or virtualized lists. Never use `ScrollView` + `.map()`.
- **UI Thread Animations**: Gestures and animations must use `react-native-reanimated` and `react-native-gesture-handler`.
- **Zustand State**: Access stores strictly with atomic selectors or `useShallow` (never full store destructuring).

---

## 5. Modal vs. Page Decision Tree (Prevent Modal Overlapping)

- **Zero Modal Stacking**: Strictly prohibit overlapping or stacking two native modal components simultaneously (e.g., an alert or decision dialog opening on top of an existing modal view). This causes focus traps, backdrop flickering, and iOS/Android rendering glitches.
- **Decision Criteria**:
  - **Use a Navigation Page / Screen**: When a flow involves multi-step forms, complex data entry, deep scrolling, or may trigger secondary alerts, confirmations, or date pickers.
  - **Use a Modal / Bottom Sheet**: ONLY for atomic, single-step selections, quick actions, or bottom sheets that do not invoke nested dialogs.
  - **Global Alert Provider**: Alert and confirmation dialogs must be rendered through a single root dialog provider to prevent backdrop collisions.

---

## 6. Self-Review Checklist Before Emitting Code

Before outputting or approving any React Native code, verify:
- [ ] Is this screen repeating a gradient, background, safe area, or back button that exists elsewhere? -> Refactor to shared layout.
- [ ] Does this file exceed 250 lines (or screen exceed 100 lines)? -> Break into sub-components and custom hooks.
- [ ] Is children/slot composition used instead of inline layout duplication?
- [ ] Is business/query logic separated into a `use[Screen].ts` hook?
- [ ] Are safe area insets handled via `react-native-safe-area-context`?
