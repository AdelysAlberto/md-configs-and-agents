---
name: mobile-native
description: iOS Human Interface Guidelines, Material Design 3, cross-platform patterns, gesture-driven interaction, safe areas, touch targets, native navigation, and battle-tested anti-patterns for React Native / Expo. Use when designing, building, or reviewing any mobile interface.
license: MIT
compatibility: opencode
metadata:
  domain: mobile
  version: "1.0.0"
---

# Mobile Native

Every mobile screen should feel like it belongs in a first-party platform app — calm, confident, native, and inevitable.

---

## Design Philosophy

- **Native over custom.** Use platform components and behaviors before inventing alternatives.
- **Subtle over expressive.** Confidence comes from restraint, not decoration.
- **"Feels obvious" over "looks fancy."** If a user has to think about how to interact, the design failed.
- **Remove before adding.** If something feels unnecessary, it is. Clarity and familiarity are the highest priorities.
- **When in doubt, follow system defaults.**

---

## iOS Human Interface Guidelines

### Visual Language

- **Typography**: System-first (SF Pro style). Clear hierarchy using size and weight, never color alone.
- **Color palette**: Neutral foundation (white/off-white backgrounds, system gray scale). Accent colors used sparingly and deliberately.
- **Depth**: Use translucency, blur, and layering where appropriate. No harsh borders — rely on spacing and grouping.

### Navigation Patterns

- **Navigation bars**: Standard UINavigationBar behavior. Back button ALWAYS on the left side.
- **Large titles**: Use when entering a top-level section. Collapse to inline on scroll.
- **Tab bars**: Bottom tab bar for primary navigation (max 5 items). Active state visually distinct.
- **Bottom sheets**: Preferred over full-screen modals for secondary content. Respect drag-to-dismiss.
- **Search**: Integrated in navigation bar or as a dedicated search interface.

### Component Behavior

- **Lists**: iOS-style grouped/inset grouped rhythm. Clear separators OR spacing, not both.
- **Buttons**: System button behavior. Clear primary vs secondary hierarchy. Every button must have visible affordance (fill, outline, or system-tinted text).
- **Alerts**: Native alert style for simple confirmations. Custom alert for complex content.
- **Action Sheets**: Bottom-anchored for destructive or multi-option choices.

### Layout Rules

- **Safe area awareness**: Mandatory. Content must respect notch, Dynamic Island, and home indicator.
- **Touch targets**: Minimum 44x44 points. No exceptions.
- **Vertical scroll**: Primary navigation pattern. Horizontal scroll only for media carousels or related content.

---

## Material Design 3 (Android)

### Visual Language

- **Material You / Dynamic Color**: Adapt to user's wallpaper-derived color scheme when available.
- **Typography**: Roboto or system font. Material type scale (Display, Headline, Title, Body, Label).
- **Elevation**: Real shadow system. Surfaces at different elevations have different tonal values.

### Navigation Patterns

- **Top app bar**: Standard toolbar. Back/Up button ALWAYS on the left.
- **Navigation drawer**: For apps with 5+ top-level destinations.
- **Bottom navigation**: 3-5 destinations. Active state with filled icon + label.
- **Navigation rail**: For tablets and large screens (side rail with icons + labels).
- **FAB**: Floating Action Button for the single most important action. Bottom-right default placement.

### Component Behavior

- **Cards**: Filled, elevated, or outlined variants. Consistent within a surface.
- **Chips**: For filters, selections, actions. Not as button replacements.
- **Dialogs**: Centered, not full-screen. Full-screen dialogs only for complex input.
- **Snackbars**: Temporary feedback at bottom. One at a time.

### Layout Rules

- **Touch targets**: Minimum 48x48dp. No exceptions.
- **Edge-to-edge**: Content extends behind system bars with appropriate insets.
- **Responsive**: Compact (phone), Medium (foldable/tablet), Expanded (desktop).

---

## Cross-Platform Patterns

### One-Handed Usability

- Primary actions in the thumb zone (bottom half of screen).
- Destructive or infrequent actions in harder-to-reach areas (top, corners).
- Bottom sheets and bottom navigation support one-handed use.
- Avoid requiring precise taps on small elements near screen edges.

### Gesture-Driven Interaction

| Gesture | Standard Use | Rule |
|---|---|---|
| **Swipe left** | Reveal delete/action buttons on list items | Always use for destructive list actions |
| **Swipe right** | Platform-specific (iOS: back navigation) | Do not override platform back gesture |
| **Pull down** | Refresh content | `isRefreshing` must bind ONLY to manual drag gesture, never background fetch |
| **Drag to dismiss** | Close bottom sheets and modals | Mandatory for bottom sheets |
| **Long press** | Context menu / selection mode | Use for secondary actions, never as only path |
| **Pinch** | Zoom maps/images | Standard for zoomable content |

### Cards & Containers

- Light and system-like. No heavy boxes with thick borders or aggressive shadows.
- Group related content by proximity and spacing, not by wrapping in bordered boxes.
- Avoid information-dense layouts. Clarity first, density second.

### Content Layout

- Vertical scroll as primary navigation. Never paginate what can scroll.
- Single-column layout for phones. Multi-column only on tablets/desktop.
- Progressive disclosure: show summary, reveal detail on interaction.

---

## Motion (Native Feel)

- **Easing**: Smooth, natural curves. No artificial bounce unless platform-standard.
- **Purpose**: Motion explains hierarchy and spatial relationships. Never decorative.
- **Vocabulary**: Fade, slide, and subtle scale. All transitions calm and intentional.
- **Duration**: 200-350ms for most transitions. Under 150ms for micro-interactions (button press, toggle).
- **Interruption**: All animations must be interruptible. User gesture always wins over animation.
- **Reduced motion**: Respect `prefers-reduced-motion` / `accessibilityReduceMotion`. Provide static alternatives.

---

## Anti-Patterns & Known Traps (React Native / Expo)

### Modal Stacking Trap

**Prohibido usar `<Modal>` nativo para pantallas completas.** Toda pantalla completa debe ser una ruta navegable dentro del `<Stack>` de Expo Router.

- Flujos conversacionales, formularios multi-step, mapas, onboarding = rutas de Stack. Nunca `<Modal>`.
- Si un modal nativo legitimo necesita mostrar un alert, debe montar su propio `<CustomAlert />` interno.
- Nunca invocar `showAlert()` del root view controller desde dentro de un `<Modal>`. El alert queda bloqueado detras, invisible hasta cerrar el modal padre.

### Back Button Placement

**El boton Back del header va SIEMPRE en el lado izquierdo.** Sin excepciones.

- Convencion universal iOS + Android.
- Colocarlo a la derecha viola la expectativa del usuario y rompe la navegabilidad.
- `headerLeft` en React Navigation / Expo Router. Nunca usar `headerRight` para back/close como accion primaria de retroceso.

### Navigation Anti-Patterns

- No usar `<Modal>` para simular navegacion entre pantallas.
- No crear custom back buttons que no respetan el gesto de swipe-back de iOS.
- No ocultar el header/navigation bar sin proporcionar una alternativa visible para regresar.
- No usar deep nesting de Stacks sin razon. Prefer flat navigation.

### Pull-to-Refresh Jank

- `isRefreshing` debe vincularse SOLO al gesto manual de drag del usuario (`isManualRefreshing`).
- Nunca vincular a background query fetching o `refetchInterval`.
- Violacion causa: spinner fantasma, layout shifts, flickering visual.

### Component Traps

- **ScrollView inside ScrollView**: Causa gestos conflictivos. Usar `FlatList` / `SectionList` con `ListHeaderComponent`.
- **TouchableOpacity sin feedback visual**: Todo elemento tocable debe tener respuesta visual (opacity, scale, highlight).
- **Absolute positioning para layout**: Usar Flexbox. Absolute solo para overlays y floating elements.
- **Hardcoded dimensions**: Usar Flexbox ratios y `Dimensions` API. Nunca hardcodear px para layout principal.
- **KeyboardAvoidingView inconsistente**: Configurar `behavior` segun plataforma (`padding` iOS, `height` Android).

### Performance Traps

- **Re-render masivo**: Aislar componentes que se actualizan frecuentemente con `React.memo` y selectores atomicos.
- **Imagenes sin cache**: Usar `expo-image` o `FastImage` con cache policy.
- **Listas sin virtualizacion**: Siempre `FlatList` / `FlashList` para listas dinamicas. Nunca `.map()` dentro de `ScrollView`.
- **HTTP polling para datos real-time**: Usar WebSockets o push events cuando esten disponibles. Prohibido `refetchInterval` como sustituto.

---

## Absolute Avoid List

- Over-designed custom components that don't feel native to the platform.
- Heavy gradients or neon colors on system surfaces.
- Harsh borders or outlines on cards and containers.
- Dense, cluttered information layouts (especially on phone screens).
- Non-standard navigation patterns that force users to learn new gestures.
- Trendy UI gimmicks or effects that add no functional value.
- Custom scrolling behavior that fights the platform's native scroll.

---

## Decision Rules

1. If it feels unnecessary, remove it.
2. If the platform has a standard way, use it.
3. If users have to think about how to interact, redesign.
4. Clarity and familiarity are always higher priority than visual expression.
5. Every screen should feel like it belongs in a first-party app.
