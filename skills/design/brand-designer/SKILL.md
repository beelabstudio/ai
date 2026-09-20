---
name: brand-designer
description: Use when a client needs a visual identity built from scratch (logo direction, color palette, typography system, voice/tone) before any site or product gets designed or coded, or when auditing an existing brand for consistency. Produces the brand foundation that web-visual-design and ux-ui-designer apply downstream.
source: framework adapted from https://github.com/alirezarezvani/claude-skills (marketing-skill/brand-guidelines, MIT) and https://github.com/anthropics/skills (canvas-design's design-direction approach)
---

# Brand Designer

## When this applies

Use this **before** `web-visual-design` or `ux-ui-designer`, whenever a client
has no real visual identity yet — no locked palette, no typeface pairing, no
name for how the brand should feel. Building a site or product on top of a
palette invented on the spot (or on the model's defaults) is how a project
ends up needing a visual rebuild later; this skill is the step that prevents
that.

Two distinct jobs live under this skill — identify which one you're doing
before starting:

1. **Creating a new identity from scratch.** No brand exists yet, or what
   exists is inconsistent enough that it isn't usable as a foundation. Follow
   the full process below.
2. **Auditing an existing identity.** A real brand already exists (logo
   files, a style guide, a live site) and the task is to check new material
   against it, or to reverse-engineer an undocumented identity into a written
   one. Skip straight to the [Audit checklist](#audit-checklist).

If the second brain already has research or a locked palette/typeface for
this project (check the project's ficha first), that supersedes anything
this skill would invent — don't re-run discovery on a decision that's
already made.

## What this skill does not do

This produces the **written identity and, where useful, first-pass concept
sketches** — not final production-ready vector logo artwork. Claude can draw
a geometric wordmark or mark as SVG to pin down a direction (proportions,
weight, a mark concept), but treat that as a brief for a human designer or a
proper vector tool (Illustrator, Figma), not as the delivered logo file. Say
this explicitly to the client/team rather than letting a rough SVG pass as
finished artwork.

## Process — building an identity from scratch

### 1. Brand foundation

Nothing visual gets decided before this exists, in writing:

| Element | Definition |
|---------|-----------|
| **Mission** | Why the business exists, beyond revenue |
| **Positioning** | What it is, for whom, against which alternative |
| **Values** | 3–5 principles that actually drive decisions, not wall art |
| **Personality** | Adjectives that describe how the brand behaves and talks |

Pull these from the client brief / second-brain research if they exist. If
they don't, ask — don't invent a mission statement to fill the slot. A
palette and typeface chosen without this foundation is decoration, not
identity, and it won't survive the first "why did we pick this" question.

### 2. Competitive and category scan

Look at 3–5 real competitors or category peers before proposing anything.
The goal isn't imitation, it's knowing what to avoid — if every competitor
already uses a blue-to-purple gradient and rounded sans-serif, that's now
the generic look for the category, not a differentiator. This is also where
the [`frontend-taste`](../frontend-taste/SKILL.md) and
[`web-visual-design`](../web-visual-design/SKILL.md) skills' "AI-generated"
tells checklists apply: none of those defaults (emoji-as-icons, blob
gradients, pill buttons everywhere, warm-cream + terracotta editorial
cliché, Inter/Space Grotesk as the safe pick) should survive into a real
identity unless a specific reason ties them to this client.

For grounding against real, structured examples rather than vague
recollection, [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md)
(MIT) is a curated collection of `DESIGN.md` analyses of real brand design
systems (Stripe, Linear, Airbnb, Apple, and more) — useful to see how a
specific real brand actually structures its palette/type/spacing decisions,
not something to copy from directly.

### 3. Direction exploration (name it, then justify it)

Propose **2–3 named directions**, not one default. For each, in a short
paragraph: name the direction (two or three words, e.g. "Quiet Precision" or
"Warm Utility" — a name the team can refer back to in feedback rounds), then
state in plain terms how it shows up in color, type, imagery, and voice, and
*why* it fits this client's positioning and audience specifically. This is
the one useful habit worth borrowing from purely artistic direction-setting:
naming the direction up front keeps feedback anchored ("this doesn't feel
precise enough") instead of vague ("I don't like it").

Reject a direction that could apply to any client in the category with the
name swapped out — if the paragraph reads the same with a different company
name dropped in, it isn't specific enough yet.

### 4. Color system

- **Primary palette (2–3 colors):** one dominant neutral, one strong brand
  color carrying most of the recognition, one supporting color.
- **Accent palette (1–3 colors):** used sparingly — CTAs, states, emphasis —
  never as a large fill.
- Every foreground/background pairing that will actually be used must pass
  WCAG AA: ≥4.5:1 for normal text, ≥3:1 for large text (18pt+) and UI
  components. Check pairs before presenting them, not after a client signs
  off on a palette that fails contrast in production.
- Document explicitly: which color is for primary CTAs vs. plain links,
  which background combinations are approved, which colors must never sit
  next to each other, and the dark-mode equivalents (every BLS deliverable
  needs to render in `prefers-color-scheme: dark`).
- Give named hex values (not "a warm orange") — a contractor with no context
  should be able to reproduce the palette exactly from the doc.

### 5. Typography system

- Max two typeface families: one for display/headings with actual
  character, one for body optimized for readability. A monospace/utility
  face only if the content needs one (pricing, code, data).
- State in one line *why* each face was picked for this client — "safe" or
  "what everyone uses" is not a reason.
- Define the full type scale (display, H1–H3, body, caption/label) with
  size range, weight, and line-height for each role, plus letter-spacing for
  large display sizes.
- Confirm license coverage for every intended use (web embedding, print,
  app bundling) and availability on Google Fonts or equivalent before
  locking a choice — don't spec a typeface the project can't actually
  license.

### 6. Logo direction

Even without producing final artwork, the identity doc must specify:

- **Variations needed:** primary (full color on light), inverted (on dark),
  single-color/monochrome, mark-only (for favicons, small sizes).
- **Clear space:** a formula (e.g. "clear space on all sides equal to the
  wordmark's cap-height"), not a fixed pixel value that breaks on rescale.
- **Minimum size:** for digital and, if relevant, print.
- **Prohibited uses:** stretching, drop shadows/gradients/outlines, placing
  on busy photography without a color block, recoloring with an accent,
  rotating.

If a first-pass concept sketch is useful to align the client before
commissioning real artwork, draw a simple geometric mark/wordmark as SVG —
label it clearly as a direction sketch, not final art.

### 7. Imagery and iconography

- Photography: lighting style, color treatment (does it get tinted/
  desaturated to match the palette?), what subjects are on-brand vs.
  generic stock to avoid.
- Illustration: flat vs. 3D, line vs. filled, palette limit.
- Icons: stroke-based or filled, weight, corner radius — and always a real
  icon set (Lucide, Heroicons, Phosphor) or custom SVG, never emoji
  standing in for icons in shipped UI (same rule `web-visual-design`
  enforces on the build side).

### 8. Voice and tone

- 4–6 voice attributes, each with what it means and what it is *not*
  (e.g. "Direct: says what it means, no filler — not blunt or dismissive").
  The "what it's not" column is what actually keeps a copywriter from
  drifting.
- A tone matrix: how the voice dial shifts by context (marketing headline
  vs. error message vs. support content vs. legal) — voice stays constant,
  tone adapts.
- A short words-to-use / words-to-avoid list, specific to this brand, not a
  generic "avoid jargon" note.
- Remember the global language split: this doc and all internal notes are
  in English; example copy is written in whatever language the project's
  own `AGENTS.md` sets for that audience (usually European Portuguese for
  BLS client work) — write voice examples in that language, not English,
  or they won't actually test the voice.

### 9. Deliverable

Ship a single **Brand Guidelines Mini-Doc** covering all eight dimensions
above, concrete enough that a contractor who has never seen the brand could
produce on-brand work from the doc alone — exact hex codes, exact type
specs, exact pixel/mm measurements, not subjective descriptions. This doc is
what `web-visual-design` and `ux-ui-designer` consume as their starting
palette/typography/voice input instead of inventing one.

## Audit checklist

Use this to check any asset (new or existing) against a locked identity,
without re-running the full creation process:

- [ ] Colors match the approved palette — no off-brand variations
- [ ] Every text/background pair in use passes WCAG AA contrast
- [ ] Fonts are the correct family and weight for their role
- [ ] Logo is an approved variation with correct clear space and minimum
      size
- [ ] Imagery/icon style matches the documented guidelines (no emoji icons,
      no off-palette illustration)
- [ ] Copy tone matches the voice attributes for its context
- [ ] No prohibited logo/color uses present (gradients on the logo, wrong
      accent as a fill, stretched proportions)
- [ ] Co-branded material follows partner logo sizing/spacing rules, if any

Be specific when flagging a deviation — name the exact color code, font
weight, or measurement that's wrong, not "this feels off."

## Relationship to other skills

- [`web-visual-design`](../web-visual-design/SKILL.md) — consumes this
  skill's palette/typography/voice output when building a code-first
  client site. Load this skill first when no identity exists yet; if one
  already does, go straight to `web-visual-design`.
- [`ux-ui-designer`](../ux-ui-designer/SKILL.md) — same relationship for
  product work with a Figma/dev-handoff pipeline; borrow its WCAG 2.1 AA
  floor regardless of which skill leads.
- [`frontend-taste`](../frontend-taste/SKILL.md) and
  [`web-interface-guidelines`](../web-interface-guidelines/SKILL.md) — run
  downstream of this skill, as the taste and technical-correctness passes
  once `web-visual-design`/`ux-ui-designer` have applied this skill's
  output.
- [`image-to-code`](../image-to-code/SKILL.md) — for reconstructing a
  specific reference image/screenshot rather than establishing a new
  identity; load this skill instead when the task is "match this exactly,"
  not "invent a system."
- `anthropic-skills:canvas-design` — useful once a direction is locked and
  the team wants a polished visual asset (poster, mood board, social
  graphic) expressing it; don't use it to *choose* the direction, that's
  this skill's job.
- `anthropic-skills:brand-guidelines` — applies Anthropic's own official
  brand identity to artifacts; unrelated to client identity work, only
  relevant for BLS's own Anthropic-facing materials.
