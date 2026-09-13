---
name: ux-wireframing
description: UX/UI design specifications, layout wireframes, dramatic minimalism ("No capes!"), user flow mapping, and visual token hierarchies. Use when planning UI layouts, user journeys, or drafting UX specifications.
license: MIT
compatibility: opencode
metadata:
  domain: design
  methodology: wireframing
---

# UX/UI Specification & Visual Wireframing Standards

Master guide for creating intuitive, minimal, and aesthetically stunning user interface specifications and layout wireframes without visual clutter.

## 1. Core UX Invariants

1. **Dramatic Minimalism ("No Capes!")**: Eliminate unnecessary visual decorations, redundant cards, nested wrappers, and clunky noise. Every pixel and element must serve a functional purpose.
2. **Predictable Visual Hierarchy**: Primary actions must stand out unequivocally. Secondary and tertiary actions must be subordinated with subtle contrast.
3. **Accessibility First**: Meet WCAG AA contrast standards (minimum 4.5:1 for normal text). Every interactive element must have clear focus, hover, and disabled states.
4. **Deliverable**: Generated UX specification document resides in `artifacts/ux_specification.md`.

---

## 2. UI Layout Wireframe Pattern

When specifying screens, define the spatial grid and responsive behavior:

```text
┌────────────────────────────────────────────────────────────┐
│ TopCard HUD (Progress Indicator & Target Maneuver)         │
├────────────────────────────────────────────────────────────┤
│ Map View / Main Canvas (Pans with User Location)           │
│                                                            │
│   [ Floating Recenter Button ]                             │
│   [ Current Street Pill: Floating Bottom-Left ]            │
├────────────────────────────────────────────────────────────┤
│ BottomBar HUD (Next Step Instructions & Finish Action)     │
└────────────────────────────────────────────────────────────┘
```

---

## 3. UX Specification Structure (`artifacts/ux_specification.md`)

```markdown
# UX/UI Interface Specification: [Feature Name]

## 1. Visual Personality & Design Tokens
- **Mood / Character**: (e.g. Sleek Dark Mode with neon accents or high-contrast clean Light Mode).
- **Core Color Palette**: Primary, Surface, Background, Accent, and Error hues.
- **Typography Scale**: Display, H1, H2, Body, and Caption font sizes and line heights.

## 2. Screen Anatomy & Wireframes
- Wireframe ASCII/Markdown diagram.
- Breakdown of each visual component (TopCard, Canvas, Controls, BottomBar).

## 3. Interaction States & Transitions
- **Initial / Empty State**: Visual cue guiding the user's first click.
- **Loading State**: Skeleton loaders matching final layout (no shifting spinners).
- **Success / Active State**: Real-time fluid rendering.
- **Error / Offline State**: Actionable recovery banner with retry trigger.

## 4. Mobile Responsiveness & Touch Targets
- Minimum touch target: 44x44 points.
- Gesture mechanics: Swipe-to-dismiss, pull-down modal sheets.
```
