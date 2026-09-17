---
name: visual-craft
description: Visual design craft knowledge base. Color psychology, intentional typography, concentric radii, optical alignment, shadows for elevation, GPU-only animations, scale-on-press, icon stroke matching, system-fit decisions, and anti-AI-cliche detection. Platform-agnostic. Use when designing, building, or reviewing any UI surface.
license: MIT
compatibility: opencode
metadata:
  domain: design
  version: "1.0.0"
---

# Visual Craft

Production-grade visual design knowledge. Every decision serves the brief, the user, and the platform — never the template.

## Design Philosophy

- **Bold with restraint.** Spend boldness in one place. Let the signature element be the one memorable thing; keep everything around it quiet and disciplined.
- **The brief wins.** Honor pinned aesthetics, eras, materials, fonts, and palettes even when they conflict with personal taste. Redirecting a clear brief is failure.
- **Refinement preserves; redesign replaces.** Refinement keeps the incumbent identity, behavior, and copy. Redesign keeps product truth, content, and function but treats the old look as evidence and anti-reference.
- **Subject-first.** If the brief does not pin the subject, pin it yourself: name the product, its audience, and the page's single job. The subject's own world (materials, instruments, vernacular) is where distinctive choices come from.

---

## Color Psychology & Palette Design

Never pick colors generically. Every hue carries meaning shaped by context, culture, and adjacency.

### Meaning by Context

| Intent | Hue direction | Notes |
|---|---|---|
| Trust / stability | Deep blue, navy, slate | Banking, insurance, enterprise |
| Urgency / action | Warm red, coral, amber | CTAs, alerts, time-sensitive |
| Calm / health | Teal, sage, soft green | Wellness, travel, onboarding |
| Premium / luxury | Black, gold, deep purple | High-end, exclusive, fashion |
| Energy / youth | Electric blue, magenta, lime | Consumer, social, gaming |
| Neutrality / clarity | Gray scale, off-whites | System UI, settings, data-dense |

### Palette Construction Rules

- Define 4-6 named hex values: primary, secondary, surface, background, accent, error.
- Test every combination for WCAG AA contrast (4.5:1 normal text, 3:1 large text).
- Dark mode is not inverted light mode. Redesign surfaces, elevation, and accent intensity independently.
- Accent colors used sparingly — if everything is accent, nothing is.
- Never use pure black (#000000) on pure white (#FFFFFF) for body text. Use near-black on near-white for comfortable reading.

### Anti-Generic Palette Rules

- No random purple-to-blue gradient as default "modern" look.
- No cream #F4F1EA + high-contrast serif + terracotta accent (AI cliche #1).
- No near-black + single acid-green or vermilion accent (AI cliche #2).
- No broadsheet layout with hairline rules and dense newspaper columns (AI cliche #3).
- If the palette you chose matches any of these defaults, revise before proceeding.

---

## Typography

Typography carries personality. It is not a neutral delivery vehicle.

### Pairing Rules

- Pair a **characterful display face** (used with restraint) with a **complementary body face** and optionally a **utility/mono face** for captions or data.
- Never use the same family for every role unless the brief demands it.
- Set a clear type scale with intentional weights, widths, and spacing.

### Hierarchy by Size & Weight

- Use size and weight to create hierarchy, not color.
- Display: bold statement, largest size, minimal use.
- H1-H3: descending size, consistent weight progression.
- Body: comfortable reading size (16px base on web, system default on mobile).
- Caption: smaller, muted, utility.

### Icon Stroke = Text Weight

- `1.5px` stroke beside regular (400) text.
- `2px` stroke beside semibold (600) text.
- One stroke weight per icon set. Never mix icon libraries on one surface.
- Icons use `currentColor` and get states from CSS color/opacity, never separate assets.
- Outline = default state. Fill = active/selected state.

---

## Surfaces & Depth

### Concentric Border Radius (Most Common Visual Bug)

**Outer radius = inner radius + padding.** Mismatched radii on nested elements is the #1 thing that makes interfaces feel off.

```
Bad:  parent rounded-xl + child rounded-xl (both 12px)
Good: parent rounded-2xl (16px) + child rounded-lg (8px) with 8px padding
      16 = 8 + 8 ✓
```

### Optical Over Geometric Alignment

When geometric centering looks off, align optically. Buttons with icons, play triangles, and asymmetric glyphs all need manual adjustment. Trust the eye over the pixel grid.

### Shadows for Elevation, Borders for Structure

- For buttons, cards, and containers whose border exists only to create depth: use layered transparent `box-shadow` values.
- Keep borders that communicate **structure or state**: dividers, layout separators, selected/focus states.
- Never use both shadow AND border for the same elevation purpose.

### Image Outlines

- Add `1px` outline with low opacity for consistent depth.
- Light mode: pure black `oklch(0 0 0 / 0.1)`. Dark mode: pure white `oklch(1 0 0 / 0.1)`.
- Never use tinted neutrals (slate, zinc) — they pick up the surface color and read as dirt.

---

## Components: Mandatory Completeness

### Buttons (Zero Tolerance)

Every button MUST have:

- **Visible background or explicit border.** A text-only element without visual boundary is not a button — it is a link. If it behaves as a button, it must look like one.
- **Clear hierarchy**: primary (filled, prominent), secondary (outlined or toned), tertiary (text-only, explicitly styled as link-like).
- **Complete states**: default, hover, focus (visible outline), active/pressed, disabled (opacity + pointer-events none), loading (spinner or skeleton replacing label).

### All Interactive Components

Must define: default, hover, focus, active, disabled states minimum. Loading state when the component triggers async work.

### Cards & Containers

- Light and system-like. Not heavy boxes with thick borders.
- Rely on spacing and grouping, not borders, to separate content.
- Avoid dense information layouts. Clarity first.

---

## Animations & Motion

### GPU-Only Rule

Only animate `transform` and `opacity`. Never animate layout properties (`top`, `left`, `width`, `height`, `margin`, `padding`).

### Scale on Press

`scale(0.96)` on click for tactile feedback. Always `0.96`. Never below `0.95` (feels exaggerated). Add a `static` prop to disable when motion would be distracting.

### Interruptible Animations

Use CSS transitions for interactive state changes (they can be interrupted mid-animation). Reserve `@keyframes` for staged sequences that run once.

### Enter/Exit

- Stagger infrequent staged entrances by ~100ms between semantic chunks. Do NOT stagger routine, high-frequency interactions.
- Exits: small fixed `translateY` instead of full height. Softer than enters. `ease-out` for both.
- Skip entrance animation on page load: `initial={false}` on `AnimatePresence`.

### Icon Transitions

Animate with `opacity`, `scale`, and `blur`:
- Scale: `0.25` to `1`
- Opacity: `0` to `1`
- Blur: `4px` to `0px`
- Spring: `{ type: "spring", duration: 0.3, bounce: 0 }` (bounce always 0).

### Reduced Motion (Non-Negotiable)

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

### Never

- `transition: all` — always specify exact properties.
- `will-change: all` — only use for `transform`, `opacity`, `filter` when first-frame stutter is observed.
- Motion as the only feedback channel — every animated state also needs a static cue (color, icon, label).
- Custom animation on high-frequency interactions (the attention cost repeats on every trigger).

---

## System-Fit Decision Framework

Before creating a new component, classify the need:

1. **Reuse** — an existing component or pattern already solves it.
2. **Compose** — existing primitives combined in an established way.
3. **Extend** — existing component needs a new state, behavior, or variant.
4. **Create** — the need is meaningfully distinct and reusable enough for a new system asset.
5. **Feature-local** — too specific to become shared.

Do NOT create a new component merely because the exact visual arrangement is new. Identify the actual missing behavior (state, accessibility, data scale, responsive, content rule, or API).

---

## Design Modes

Choose the mode from the requested surface:

| Mode | Surface | Priority |
|---|---|---|
| **Persuade** | Landing pages, marketing, pricing | Earn attention and action. Ship real imagery. |
| **Operate** | App UI, dashboards, admin, tools | Scanability, consistency, native expectations. Brand in precise details. |
| **Read** | Docs, articles, guides, changelogs | Structure for comprehension, then make reading worth staying. |
| **Experience** | Portfolios, galleries, showcases | Let the artifact lead. Interface recedes. |

---

## Two-Pass Design Process

### Pass 1: Brainstorm

Create a compact token system:
- **Color**: 4-6 named hex values with rationale.
- **Type**: 2+ typeface roles (display, body, utility) with specific families.
- **Layout**: one-sentence prose + ASCII wireframe.
- **Signature**: the single unique element this page will be remembered by.

### Pass 2: Anti-Cliche Review

- Does any part read like the generic default for any similar page?
- Does the palette match AI cliche #1, #2, or #3?
- Is the signature element actually distinctive or just "big gradient hero"?
- Revise what fails. State what changed and why.
- Only after confirming: start building.

---

## Common Mistakes (Quick Reference)

| Mistake | Fix |
|---|---|
| Same border radius on nested parent + child | `outerRadius = innerRadius + padding` |
| Icons look off-center | Adjust optically, not geometrically |
| Border used only for fake elevation | Use layered `box-shadow` |
| Button without visible background | Add background or explicit border. Always. |
| `transition: all` | Specify exact properties |
| First-frame animation stutter | Add `will-change: transform` (sparingly) |
| Hairline icon beside bold text | Match stroke width to text weight |
| Separate icon assets per state | One `currentColor` SVG, states via CSS |
| Filled icons everywhere | Outline default, fill only for active |
| Animation on every hover/keystroke | Instant feedback or ≤150ms opacity/color |
| Generic AI color palette | Verify against the 3 known cliches |
