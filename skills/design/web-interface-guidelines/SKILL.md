---
name: web-interface-guidelines
description: Concrete MUST/SHOULD/NEVER rules for interaction, forms, animation, accessibility, and performance correctness on the web. Use as the technical-correctness pass on any interface — complements frontend-taste (aesthetic judgment) and web-visual-design/ux-ui-designer (process). Framework-agnostic, some items React/Next.js-specific.
source: adapted from https://github.com/vercel-labs/web-interface-guidelines (MIT)
---

# Web Interface Guidelines

Interfaces succeed or fail on hundreds of small decisions, most of them
invisible when done right and only noticed when done wrong. This is a
mechanical checklist for those decisions — load it as the **technical
correctness** pass on an interface, independent of whether it looks good.

## Interactions

- **Keyboard works everywhere.** All flows are keyboard-operable, following
  the [WAI-ARIA Authoring Patterns](https://www.w3.org/WAI/ARIA/apg/patterns/).
- **Visible, unobscured focus.** Prefer `:focus-visible` over `:focus`.
  Sticky headers/footers/overlays never cover the focused element.
- **Hit targets ≥24px** (≥44px on mobile); if the visual target is smaller,
  expand the hit area.
- **Mobile `<input>` font-size ≥16px** to prevent iOS auto-zoom on focus.
- **Never disable browser zoom** (`user-scalable=no`, `maximum-scale=1`).
- **Never block paste** in `<input>`/`<textarea>`.
- **Loading buttons** show a spinner and keep the original label; keep
  submit enabled until the request starts, then disable with the spinner.
- **Forms accept free text and validate after** — don't block typing, don't
  prevent an incomplete submit from surfacing validation. Errors inline next
  to the field; on submit, focus the first error.
- **`autocomplete` + meaningful `name` + correct `type`/`inputmode`** on
  every field; compatible with password managers and 2FA (allow pasting
  codes); trim values to strip text-expansion trailing spaces.
- **Warn on unsaved changes** before navigation away.
- **URL reflects state** — filters, tabs, pagination, expanded panels are
  deep-linkable. Back/Forward restores scroll position.
- **Use `<a>`/`<Link>` for navigation**, never `<div onClick>` — this is
  what makes Cmd/Ctrl/middle-click work.
- **Optimistic UI** where success is likely; reconcile on response, roll
  back or offer Undo on failure. Confirm destructive actions or provide an
  Undo window instead.
- **Polite `aria-live`** for toasts and inline validation.
- **Touch/drag:** generous hit targets, `overscroll-behavior: contain` in
  modals/drawers, disable text selection and set `inert` during drag, every
  drag/swipe/pinch gesture has a tap/click + keyboard alternative unless
  the gesture is essential.
- **If it looks clickable, it must be clickable.** No dead zones.

## Animation

- **Honor `prefers-reduced-motion`** — provide a reduced variant or disable.
- **Animate only compositor-friendly properties** (`transform`, `opacity`).
  Never animate layout properties (`top`, `left`, `width`, `height`), and
  never `transition: all` — list properties explicitly.
- **Animation must be motivated** — hierarchy, storytelling, feedback, or a
  state transition. "It looked cool" is not a reason.
- **Interruptible and input-driven.** Autoplay only for muted, non-essential
  loops; anything autoplaying longer than 5 seconds needs a pause/stop/hide
  control.
- **Correct `transform-origin`** so motion starts where it physically should.

## Layout

- **Optical alignment** — adjust ±1px when perception beats strict geometry.
- **Deliberate alignment to grid/baseline/edges**, never accidental
  placement.
- **Verify at mobile, laptop, and ultra-wide** (simulate ultra-wide at 50%
  zoom).
- **Respect safe areas** (`env(safe-area-inset-*)`).
- **No unwanted scrollbars** — fix overflows rather than hiding them.

## Content & accessibility

- **Skeletons mirror the final content shape** to avoid layout shift.
- **No dead ends** — every state offers a next step or a recovery path.
- **Design empty, sparse, dense, and error states explicitly**, not just the
  happy path.
- **Redundant status cues, not color-only** — icons carry text labels too.
- **Accessible names exist even when visuals omit labels.** Icon-only
  buttons have a descriptive `aria-label`.
- **Prefer native semantics** (`button`, `a`, `label`, `table`) before
  reaching for ARIA.
- **Resilient to real user content** — short, average, and very long strings
  all need to not break the layout (`truncate`, `line-clamp-*`,
  `break-words`; flex children need `min-w-0` to allow truncation).
- **Locale-aware dates/times/numbers** (`Intl.DateTimeFormat`,
  `Intl.NumberFormat`).
- **Media has captions/transcripts** as applicable; controls are
  keyboard-operable; decorative media is hidden from assistive tech.

## Performance

- **Measure reliably** — disable browser extensions that skew runtime,
  profile with CPU/network throttling.
- **Track and minimize re-renders.**
- **Mutations (`POST`/`PATCH`/`DELETE`) target <500ms.**
- **Virtualize lists over ~50 items.**
- **Preload above-fold images, lazy-load the rest**; set explicit image
  dimensions to prevent CLS.
- **Prefer video over animated GIF** for short non-essential loops
  (`<video autoplay muted loop playsinline>`), with a reduced-motion still
  fallback.

## Dark mode & theming

- **`color-scheme: dark` on `<html>`** for dark themes.
- **`<meta name="theme-color">` matches the page background.**
- **Native `<select>` needs explicit `background-color` and `color`**
  (a real Windows rendering bug otherwise).

## Hydration

- **Inputs with a `value` need an `onChange`**, or use `defaultValue`
  instead — an uncontrolled/controlled mismatch is a common source of lost
  input.
- **Guard date/time rendering against hydration mismatch** (server and
  client can disagree on "now").

## Design (visual-technical, not aesthetic)

- **Layered shadows** (ambient + direct) read as more physical than a
  single flat shadow.
- **Nested radii: child ≤ parent, concentric** — a rounded card with a
  square button inside it looks like a mistake.
- **Hue consistency** — tint borders/shadows/text toward the background hue
  rather than pure grey/black.
- **Prefer [APCA](https://apcacontrast.com/) over WCAG 2** for contrast
  where the tooling supports it; always increase contrast on
  `:hover`/`:active`/`:focus`, never decrease it.
- **Accessible charts** — color-blind-friendly palettes, not color-only
  encoding.

## Relationship to other skills

- [`frontend-taste`](../frontend-taste/SKILL.md) — the aesthetic-judgment
  counterpart. Run both as independent passes on the same interface: this
  skill asks "does it actually work right," that one asks "does it look
  designed."
- [`web-visual-design`](../web-visual-design/SKILL.md) and
  [`ux-ui-designer`](../ux-ui-designer/SKILL.md) — the process skills this
  one supplements. Neither of those two owns the full technical-correctness
  list; check this skill's items regardless of which process skill is
  leading.
