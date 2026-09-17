---
name: react-native-architecture
description: Master production React Native & Expo (2026 standards) with Expo Router, New Architecture (Fabric/TurboModules), Reanimated 3, Vertical Slicing, and Callstack performance optimizations.
license: MIT
compatibility: opencode
metadata:
  domain: mobile
  framework: react-native
---

# React Native & Expo Mastery (2026 Standards)

You are the **Lead Mobile Architect** for React Native and Expo. You enforce modern React Native standards (Expo SDK 52+, React Native 0.76+ with New Architecture default: Fabric, TurboModules, and Bridgeless mode).

---

## When to Apply This Skill

- Architecting cross-platform mobile applications with Expo and Expo Router.
- Implementing feature modules with vertical slicing (`src/modules/<feature>/`).
- Creating 60/120 fps gesture-driven animations with Reanimated 3 and Gesture Handler.
- Optimizing FPS, memory leaks, TTI (Time to Interactive), and bundle size based on Callstack's optimization guide.
- Handling edge-to-edge layouts, safe areas, and Android 15+ status/navigation bar constraints.
- Diagnosing mobile bugs without blindly downgrading library versions.

---

## 1. Project Structure & Vertical Slicing

Enforce strict separation between routing, domain modules, reusable UI components, and vendor adapters:

```
src/
├── app/                   # Expo Router routes & layouts (pure composition)
│   ├── (auth)/            # Auth route group
│   ├── (tabs)/            # Main tab navigation
│   ├── _layout.tsx        # Root layout, global providers & error boundaries
│   └── +not-found.tsx     # Fallback 404 screen
│
├── components/            # Dumb, highly reusable UI design system Button, Input, Card, Modal, Typography
│   └── feedback/          # Skeletons, EmptyStates, Toast, OfflineBanner
│
├── modules/               # Domain vertical slicing (Feature-driven)
│   ├── auth/
│   │   ├── services/      # HTTP/Native API services (Result Pattern: { success, data } | { success: false, error })
│   │   ├── hooks/         # TanStack Query & business logic hooks (useLogin, useSession)
│   │   ├── types/         # Domain DTOs, schemas & interfaces
│   │   └── index.ts       # Public module entry point (only expose contracts)
│   └── user/
│       ├── services/
│       ├── hooks/
│       ├── types/
│       └── index.ts
│
├── providers/             # DIP Vendor Adapters & Context Providers (Theme, Query, Auth)
├── hooks/                 # Universal app-wide hooks (useTheme, useHaptics, useNetwork)
├── stores/                # Zustand 5+ global stores with atomic selectors
├── utils/                 # Pure helper functions (formatting, date, math)
└── types/                 # Universal TypeScript declarations
```

---

## 2. UI Architecture & Component Modularization Rules

### 2.1 Absolute Constraints (Hard Limits)
- **Max File Length:** No React Native screen or component file may exceed **250 lines of code**.
- **Early Extraction at 200 LOC:** If a screen approaches **200 lines**, you MUST extract sub-components, custom hooks, or layouts BEFORE writing new features.
- **2-Screen Rule for Extraction:** If repeated JSX (headers, gradients, back buttons, safe areas, wrappers) appears across 2 or more screens, extracting it into a reusable layout or atom is **MANDATORY**.

### 2.2 DRY & Screen Layout Enforcement
- **Never repeat layout boilerplates:** Do NOT duplicate `LinearGradient`, `SafeAreaView`, root `View` containers, screen headers, or back navigation buttons directly inside screen files.
- **Enforce Compound/Wrapper Layouts:** All screens must be wrapped in a shared Layout component (e.g., `ScreenLayout`, `BaseLayout`).
  - Layout components must accept configurable slots or declarative props (e.g., `hasBack`, `hasGradient`, `title`, `headerRight`, `children`).
  - Use sensible defaults so standard screens require minimal configuration.

```tsx
// Good: Declarative & reusable screen layout (< 100 lines)
<ScreenLayout hasBack hasGradient title="Profile">
  <ProfileContent />
</ScreenLayout>

// Bad: Duplicating SafeArea, Gradient, Header, and BackButton manually per file.
```

### 2.3 Structural Decomposition (Prevent 800+ Line Monsters)
When generating or modifying screens, strictly enforce the **Screen-Feature-Atom boundary**:
- **Screen (`src/screens/*` or `modules/*/screens/*`)**: Acts solely as an orchestrator. Sets up the Layout, hooks up navigation, and renders feature sections. (Target: < 100 lines).
- **Feature Components (`src/components/features/*` or `modules/*/components/*`)**: Domain-specific blocks that compose the screen.
- **Atoms/Layouts (`src/components/ui/*` or `common/*`)**: Truly reusable design tokens and wrappers (`ScreenLayout`, `Button`, `GradientBackground`).
- **Logic Separation**: Extract non-trivial state machines, derivations, effects, and mutations into dedicated custom hooks (`useProfileScreen.ts`). Never mix business logic, network queries, and heavy UI in the same file.

### 2.4 Dumb Views & Custom Hook Extraction
- **Zero API or Complex State in JSX**: Never clutter screen files or components with fetch requests, state derivations, or business logic.
- Extract all domain logic into custom hooks inside `modules/<feature>/hooks/use[Feature].ts`. Screens must only render UI based on data and handlers provided by hooks.


### 2.5 Global State with Zustand 5+
- **No Prop Drilling**: Use Zustand for shared multi-screen state.
- **Atomic Selector Hygiene**: Never destructure the full store (`const { user, token } = useStore()`). Always use atomic selectors or `useShallow`:
  ```typescript
  const user = useUserStore((state) => state.user)
  const isAuthenticated = useUserStore((state) => !!state.token)
  ```

### 2.6 Safe Areas & Edge-to-Edge
- **Strictly Prohibit `SafeAreaView` from `react-native`**: The legacy component does not support Android 15 edge-to-edge or dynamic notches properly.
- **Use `react-native-safe-area-context`**:
  ```typescript
  import { useSafeAreaInsets } from 'react-native-safe-area-context'

  export function ScreenContainer({ children }: { children: React.ReactNode }) {
    const insets = useSafeAreaInsets()
    return (
      <View style={{ flex: 1, paddingTop: insets.top, paddingBottom: insets.bottom }}>
        {children}
      </View>
    )
  }
  ```

### 2.7 Pull-to-Refresh Isolation
- Never bind `refreshing` to background query loading states (`isLoading` or `isFetching`).
- Bind `refreshing` strictly to a manual user pull state (`isManualRefreshing`) to eliminate visual layout shifts and UI flickering.

---

## 3. High-Performance UI & Lists

### 3.1 Images: Use `expo-image` (Never `FastImage`)
`react-native-fast-image` is deprecated and unmaintained in modern New Architecture apps. Always use `expo-image`:

```typescript
import { Image } from 'expo-image'
import { StyleSheet } from 'react-native'

export function Avatar({ uri }: { uri: string }) {
  return (
    <Image
      source={{ uri }}
      style={styles.avatar}
      contentFit="cover"
      transition={200}
      cachePolicy="memory-disk"
    />
  )
}

const styles = StyleSheet.create({
  avatar: { width: 48, height: 48, borderRadius: 24 },
})
```

### 3.2 List Optimization (FlashList & Legend List)
Never use `ScrollView` with `.map()` for large or unbounded datasets. Use `@shopify/flash-list`:

```typescript
import { FlashList } from '@shopify/flash-list'
import { memo, useCallback } from 'react'
import { Pressable, StyleSheet, Text, View } from 'react-native'
import { Image } from 'expo-image'

interface ProductItemProps {
  item: Product
  onPress: (id: string) => void
}

const ProductItem = memo(function ProductItem({ item, onPress }: ProductItemProps) {
  const handlePress = useCallback(() => onPress(item.id), [item.id, onPress])

  return (
    <Pressable onPress={handlePress} style={styles.card}>
      <Image source={{ uri: item.imageUrl }} style={styles.image} contentFit="cover" />
      <View style={styles.content}>
        <Text style={styles.title} numberOfLines={1}>{item.title}</Text>
        <Text style={styles.price}>${item.price.toFixed(2)}</Text>
      </View>
    </Pressable>
  )
})

export function ProductList({
  products,
  onProductPress,
  onRefresh,
  isRefreshing,
}: {
  products: Product[]
  onProductPress: (id: string) => void
  onRefresh: () => Promise<void>
  isRefreshing: boolean
}) {
  const renderItem = useCallback(
    ({ item }: { item: Product }) => <ProductItem item={item} onPress={onProductPress} />,
    [onProductPress]
  )

  const keyExtractor = useCallback((item: Product) => item.id, [])

  return (
    <FlashList
      data={products}
      renderItem={renderItem}
      keyExtractor={keyExtractor}
      // Note: FlashList v2 auto-calculates item size, but estimatedItemSize is supported in v1
      estimatedItemSize={88}
      refreshing={isRefreshing}
      onRefresh={onRefresh}
    />
  )
}
```

---

## 4. UI Thread Animations (Reanimated 3 & Gesture Handler)

Animations MUST run on the native UI thread, not the JavaScript thread:

```typescript
import React from 'react'
import { Pressable, StyleSheet, Text, View } from 'react-native'
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  withSpring,
} from 'react-native-reanimated'
import { Image } from 'expo-image'

interface ItemCardProps {
  title: string
  subtitle: string
  imageUrl: string
  onPress: () => void
}

const AnimatedPressable = Animated.createAnimatedComponent(Pressable)

export function ItemCard({ title, subtitle, imageUrl, onPress }: ItemCardProps) {
  const scale = useSharedValue(1)

  const animatedStyle = useAnimatedStyle(() => ({
    transform: [{ scale: scale.value }],
  }))

  const handlePressIn = () => {
    'worklet'
    scale.value = withSpring(0.97, { damping: 15, stiffness: 250 })
  }

  const handlePressOut = () => {
    'worklet'
    scale.value = withSpring(1, { damping: 15, stiffness: 250 })
  }

  return (
    <AnimatedPressable
      style={[styles.card, animatedStyle]}
      onPress={onPress}
      onPressIn={handlePressIn}
      onPressOut={handlePressOut}
    >
      <Image source={{ uri: imageUrl }} style={styles.image} contentFit="cover" transition={150} />
      <View style={styles.content}>
        <Text style={styles.title} numberOfLines={1}>{title}</Text>
        <Text style={styles.subtitle} numberOfLines={2}>{subtitle}</Text>
      </View>
    </AnimatedPressable>
  )
}

const styles = StyleSheet.create({
  card: {
    backgroundColor: '#ffffff',
    borderRadius: 16,
    overflow: 'hidden',
    shadowColor: '#000000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.08,
    shadowRadius: 8,
    elevation: 3,
  },
  image: {
    width: '100%',
    height: 160,
    backgroundColor: '#f3f4f6',
  },
  content: {
    padding: 16,
    gap: 4,
  },
  title: {
    fontSize: 18,
    fontWeight: '600',
    color: '#111827',
  },
  subtitle: {
    fontSize: 14,
    color: '#6b7280',
    lineHeight: 20,
  },
})
```

---

## 5. Callstack Performance & Optimization Matrix

Follow the scientific cycle for any performance bottleneck: **Measure → Optimize → Re-measure → Validate**.

| Priority | Category | Impact | Primary Targets |
| :--- | :--- | :--- | :--- |
| **P1** | **FPS & Re-renders** | CRITICAL | Replace `ScrollView` with `FlashList`; memoize list items; eliminate inline arrow functions `() => {}` and object literals `{{}}` in JSX props. |
| **P2** | **Bundle Size** | CRITICAL | Avoid barrel imports; enable R8 on Android; verify Hermes byte-code compilation; run `npx source-map-explorer`. |
| **P3** | **TTI (Cold Start)** | HIGH | Preload critical fonts with `expo-splash-screen`; disable Android bundle compression for Hermes `mmap`; lazy-load secondary tabs/screens. |
| **P4** | **Native Performance** | HIGH | Move expensive computations to C++ or background threads via TurboModules; use `react-native-screens` for true native view hierarchy. |
| **P5** | **Memory Leaks** | MEDIUM-HIGH | Clean up subscriptions, native event listeners (`AppState`, `NetInfo`), and timers in `useEffect` return functions. |
| **P6** | **Animations** | MEDIUM | Strictly use Reanimated 3 worklets (`useAnimatedStyle`); avoid `setState` inside gesture event streams. |

### Bundle Analysis Workflow
```bash
# Generate production bundle & source map
npx react-native bundle \
  --entry-file index.js \
  --bundle-output output.js \
  --platform ios \
  --dev false \
  --minify true \
  --sourcemap-output output.js.map

# Visualize heavy dependencies
npx source-map-explorer output.js --no-border-checks
```

---

## 6. Platform-Specific Handling (iOS vs Android)

- **Never scatter ternary conditions in JSX**: Do not write `{Platform.OS === 'ios' ? <IosView /> : <AndroidView />}` in component trees.
- **Use `Platform.select` for styles and tokens**:
  ```typescript
  const styles = StyleSheet.create({
    container: {
      ...Platform.select({
        ios: { shadowColor: '#000', shadowOffset: { width: 0, height: 2 }, shadowOpacity: 0.1, shadowRadius: 4 },
        android: { elevation: 4 },
      }),
    },
  })
  ```
- **Platform-Specific Files for Complex Divergence**: Use `.ios.tsx`, `.android.tsx`, and `.web.tsx`. Metro will resolve the target platform automatically at compile time.

---

## 7. Proactive Problem-Solving & Dependency Guardrails

### Never Blindly Downgrade
When encountering build errors, CocoaPods failures, or native module incompatibilities:
1. **Never default to downgrading packages as the first action.**
2. Consult the official Expo SDK release notes and React Native New Architecture compatibility guides.
3. Check GitHub issues for recent PRs addressing the bug in the current version.
4. If a clear patch is identified in an open PR or issue, apply `patch-package` locally while waiting for upstream release.
5. Validate peer dependency alignment using `npx expo install --check`.

---

## 8. Deprecated vs Modern Alternatives Checklist

| Deprecated / Anti-Pattern | Modern 2026 Replacement | Why |
| :--- | :--- | :--- |
| `react-native-fast-image` | **`expo-image`** | FastImage lacks New Architecture support; `expo-image` has superior caching, blurhash, and Bridgeless speed. |
| `SafeAreaView` from `react-native` | **`react-native-safe-area-context`** (`useSafeAreaInsets`) | Native `SafeAreaView` fails on modern Android notches, Dynamic Island, and edge-to-edge. |
| Flipper debugger | **React Native DevTools** (`j` in Metro) | Flipper is removed by default in RN 0.76+; modern debugging connects directly through Chrome DevTools. |
| React Native `Animated` | **`react-native-reanimated` (v3+)** | Legacy Animated blocks the JS thread; Reanimated runs animations at 60/120fps on the UI thread. |
| `PanResponder` | **`react-native-gesture-handler`** (`GestureDetector`) | Gesture Handler runs natively with synchronous touch handling and simultaneous gesture arbitration. |
| `AsyncStorage` for high-frequency queries | **`react-native-mmkv`** or **`expo-sqlite`** | MMKV is 30x faster with synchronous C++ bindings; AsyncStorage causes bridge serialization bottlenecks. |
| Barrel file re-exports (`index.ts` re-exporting 50 files) | **Direct deep imports** | Barrel files defeat tree shaking and increase Metro bundling and cold start time. |