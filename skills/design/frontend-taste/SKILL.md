---
name: frontend-taste
description: Deep aesthetic-judgment pass for landing pages, portfolios, and marketing sites — the "does this look AI-generated" arbiter. Use alongside web-visual-design (BLS process) and web-interface-guidelines (technical correctness) as the taste/quality specialist in a design review. Not for dashboards, data tables, or multi-step product UI.
source: condensed and adapted from https://github.com/Leonxlnx/taste-skill (MIT), with additions from https://github.com/pbakaus/impeccable (Apache 2.0) — see "A note on Impeccable" below
---

# Frontend Taste

A mechanical checklist for the thing that's normally subjective: does this page
read as deliberately designed for *this* client, or as the median output any
model would produce for anyone. Load this as the **taste specialist** in a
design review — a second, independent pass after `web-visual-design` or
`ux-ui-designer` has done the process work, checking the result against a much
longer, harder-won list of "AI tells" than either of those skills carries on
its own.

**Scope:** landing pages, portfolios, marketing sites, redesigns. **Not for**
dashboards, dense product UI, data tables, multi-step forms/wizards, code
editors, or native mobile — those have their own design systems (Fluent,
Carbon, Polaris, Atlassian) and this skill's rules don't transfer.

## 1. Read the brief before generating anything

State one line before writing code: *"Reading this as: \<page kind> for
\<audience>, with a \<vibe> language, leaning toward \<design system or
aesthetic family>."* Pull the page kind, vibe words, reference URLs/screenshots,
audience, and existing brand assets from the brief — don't default to a generic
aesthetic because the brief under-specifies. If the read is genuinely
ambiguous, ask **one** question, never a list. If you can confidently infer,
don't ask.

Do not default to: purple/blue AI gradients, a centered hero over a dark mesh
background, three identical feature cards, glassmorphism on everything,
infinite-loop micro-animations everywhere, or Inter + slate-900 as the
unquestioned typography choice.

Name the **mode** the surface is actually in — it decides how much this
skill's rules even apply, before the dials do:

| Mode | The visitor's job | Design is... |
|---|---|---|
| **Persuade** | decide and act | the product — landing pages, pricing, campaigns |
| **Operate** | complete a task | secondary to scanability and consistency — app UI, dashboards, settings |
| **Read** | understand something | structured for comprehension first — docs, articles, changelogs |
| **Experience** | be inside the work | receding in favor of the artifact — portfolios, galleries, showcases |

This skill's rules (dials, bias-correction, AI tells) are written for
**Persuade** and **Experience** surfaces. A docs page for a design tool is
still **Read** — don't apply the hero/CTA/eyebrow rules to it. A SaaS
product's own dashboard is **Operate** — defer to the design system in
Section 3, not this skill's aesthetic judgment.

## 2. Set the three dials

Every layout, motion, and density decision downstream is gated by these.
State the values and *why*, don't silently use the baseline.

| Dial | 1 | Baseline | 10 |
|---|---|---|---|
| `DESIGN_VARIANCE` | Perfect symmetry | 8 | Artsy chaos |
| `MOTION_INTENSITY` | Static | 6 | Cinematic / physics |
| `VISUAL_DENSITY` | Art gallery / airy | 4 | Cockpit / packed data |

Rough presets: minimalist/editorial/Linear-style → variance 5-6, motion 3-4,
density 2-3. Premium consumer/Apple-y/luxury → variance 7-8, motion 5-7,
density 3-4. Playful/Awwwards/experimental agency → variance 9-10, motion
8-10, density 3-4. Trust-first/public-sector/regulated → variance 3-4, motion
2-3, density 4-5.

## 3. Real design system vs. honest aesthetic

Don't invent CSS for something that has an official package, and don't
pretend an aesthetic trend is an official system.

- Enterprise/Microsoft-ish → `@fluentui/react-components`. Material-flavored →
  `@material/web`. IBM-style B2B → `@carbon/react`. Shopify admin →
  Polaris. Atlassian/Jira-style → `@atlaskit/*`. GitHub-ish devtool →
  `@primer/react-brand`. UK public sector → `govuk-frontend`. US public
  sector → `uswds`. Modern SaaS you own the components for → shadcn/ui,
  never shipped in its default, unthemed state.
- **One system per project** — never mix Fluent with Carbon, or shadcn with
  Material in the same tree.
- Aesthetics with no official package (glassmorphism, bento, brutalism,
  editorial/magazine, dark-tech, aurora/mesh gradients, kinetic typography,
  Apple "Liquid Glass" on the web) are built with native CSS/Tailwind — label
  them honestly as an approximation, not as the official thing.

## 4. Bias-correction rules (where models default to cliché)

**Typography.** Discourage Inter as the unexamined default — reach for
Geist, Outfit, Cabinet Grotesk, or Satoshi first (Inter is fine when the brief
explicitly wants neutral/standard, or is public-sector/accessibility-first).
Serif is *very* discouraged as a default — "feels premium/editorial" is not a
reason; only use it when the brand brief names a serif or the aesthetic is
genuinely editorial/luxury/heritage, and never reuse the same serif across
consecutive client projects. `Fraunces` and `Instrument Serif` are specifically
banned as defaults — they're the two fonts every model reaches for. Emphasize
a word within a headline with italic/bold of the *same* family, never by
injecting a random serif word into a sans headline.

**Color.** Max one accent color, saturation under 80% by default. No
automatic purple/blue glow buttons or neon gradients (override when the brand
itself is purple). One accent, used identically everywhere on the page — a
warm-grey site does not get a random blue CTA three sections down. For
premium-consumer briefs (cookware, wellness, artisan, DTC home goods), the
banned default is warm beige/cream + brass/clay/oxblood + espresso-dark text
— every model reaches for this exact family and it makes every premium brand
look the same. Rotate instead: cold luxury (silver-grey/chrome), forest (deep
green + bone + amber), black-and-tan, cobalt + cream, terracotta + slate, or
pure monochrome + one saturated pop. Don't ship the same rotation twice in a
row for the same client type.

**Layout.** Avoid a centered hero once variance is above ~4 — prefer split
screen, left-aligned content with a right-aligned asset, or asymmetric
white space (centered is fine for editorial/manifesto briefs where the
message itself is the design). Pick one corner-radius scale for the whole
page and don't mix it. Cards only when elevation communicates real hierarchy;
otherwise group with a top border or negative space.

**Interactive states.** Implement the full cycle — loading (skeletons that
match the final shape, not a generic spinner), empty, and error states, not
just the happy path. Every CTA must pass a contrast check against its
background (WCAG AA, 4.5:1) — white-on-white or a transparent button with no
border on a busy background is a hard fail. CTA labels must fit on one line
at desktop; if it wraps, shorten the label or widen the button, never
constrain `max-width`.

## 5. Layout hard rules (failing any of these is shipping broken work)

- **Hero fits the initial viewport.** Headline max 2 lines, subtext max 20
  words and 3-4 lines, CTA visible with no scroll. If it doesn't fit, cut
  copy or reduce font scale — never let the hero force a scroll to find the
  CTA. Top padding caps at `pt-24` desktop; more than that reads as a bug.
- **Hero stack is at most 4 text elements**: an eyebrow OR a brand strip
  (pick zero or one), headline, subtext, CTAs. No trust micro-strip, no
  pricing teaser, no feature bullets inside the hero — those live in a
  section below it.
- **Navigation renders on one line at desktop**, max 80px tall. A two-line
  nav at desktop is broken, not "dense."
- **No layout family repeats more than needed.** A landing page with 8
  sections should use at least 4 different layout families. Max 2
  consecutive left-image/right-text zigzag sections before something breaks
  the pattern.
- **Eyebrow restraint (the most-violated rule in practice):** max 1 eyebrow
  per 3 sections. If section A has one, the next two can't. Most sections
  don't need one at all — the headline alone is enough.
- **Bento grids have exactly as many cells as there is content for** — no
  blank filler tile because the grid math didn't work out.
- **Testimonial quotes: max 3 lines of body**, real attribution (name +
  role, never just a first name).
- **One theme for the whole page.** If it's dark mode, every section is dark
  mode — no light-mode section sandwiched in the middle.

## 6. AI tells (forbidden by default, unless the brief explicitly asks)

These are the patterns that read as templated regardless of how polished the
execution is:

- **Content:** generic names ("John Doe", "Sarah Chan"), generic startup
  names ("Acme", "Nexus", "Cloudly"), filler verbs ("Elevate", "Seamless",
  "Unleash", "Revolutionize"), fake-perfect numbers (`99.99%`, `50%`) instead
  of organic ones (`47.2%`).
- **Visual:** pure black backgrounds (`#000000` — use off-black/zinc-950),
  neon outer glows, hand-rolled SVG icons instead of a real icon library,
  div-based fake product screenshots (fake terminal/dashboard built from
  styled `<div>`s — the single most common tell), broken/generic Unsplash
  links instead of real or generated imagery.
- **Structural clutter that shows up in real production tests:** version
  labels in the hero (`v0.6`, `BETA`) unless the brief is literally about a
  launch; section-number eyebrows (`00 / INDEX`, `002 · Featured`); decorative
  colored status dots with no real semantic state; scroll cues (`↓ scroll`,
  animated mouse wheel — if they haven't scrolled yet, they're looking at the
  hero, they know what scrolling is); rotated vertical text as agency
  decoration; a floating unaligned paragraph in the corner of a section
  header; a spec table with a hairline under every single row (group into 2-3
  clusters instead); locale/time/weather strips in the nav or footer unless
  the brand is genuinely about place or timezone-distributed work.
- **Duplicate CTA intent.** "Get in touch", "Contact us", "Let's talk", and
  "Start a project" on the same page are all the same intent — pick one label
  and reuse it in nav, hero, and footer.
- **Em-dash ban (the single most-violated tell).** No `—` or `–`-as-separator
  anywhere visible — headlines, eyebrows, body copy, captions, button text,
  alt text, quote attribution. Restructure with a period, comma, or regular
  hyphen. This one has no "sparingly is fine" exception.

### 6.A Additional tells (from Impeccable's "craft floor")

A stricter, complementary set — apply these as hard bans, not "use
sparingly":

- **The browser surfaces you didn't draw still carry the design.** Text
  selection color, the caret, custom scrollbars, focus rings, underline
  offset, and tabular-number alignment in data all ship with unthemed
  browser defaults by default. Theme them from the palette — this is the
  cheapest signal a page was actually designed rather than assembled, and
  the one most commonly skipped.
- **Kicker/eyebrow labels are a stricter ban here than Section 5's "max 1
  per 3 sections."** If the brief doesn't specifically call for one, default
  to none — delete the label and let the heading carry its own weight.
- **Type measure specifics:** body line length 65-75ch, display size caps
  around `6rem`, tracking floor around `-0.04em` for large display type.
- **No geometric masks standing in for photographic cutouts.** A circle,
  polygon, or radial-gradient crop approximating a subject's silhouette
  reads as the cheap version — derive a real alpha matte or use an actual
  cutout asset.
- **No monospace as a costume for "feels technical.**" Reserve monospace for
  actual code, data, or measurement — not as a decorative signal.
- **No system display face** (Impact, Arial Black, the platform default
  sans) as the display voice of a page with its own established visual
  world — source and self-host a face that actually matches; the closest
  installed font is a failure, not an acceptable fallback.
- **No hard-offset block shadows** (`box-shadow: 4px 4px 0`) outside an
  actual neobrutalist direction — it's a costume, not a depth system.

## 7. Redesign protocol

Misclassifying "redesign" as "greenfield" is the most common source of bad
redesign output.

1. **Detect the mode first**: preserve-and-modernize vs. full overhaul vs.
   effectively greenfield (brand itself is changing). Ask once if genuinely
   ambiguous.
2. **Audit before touching anything**: current brand tokens (colors, type,
   logo, radii), information architecture, what content is doing real work
   vs. filler, what patterns to preserve vs. retire, and the SEO baseline
   (current ranking pages, meta titles, structured data) — SEO regression is
   the single biggest redesign risk.
3. **Never silently change**: URL/route slugs, primary nav labels, form
   field names (breaks analytics and autofill), the logo/wordmark, existing
   legal/consent copy.
4. **Apply modernization levers in order, stop when the brief is satisfied**:
   typography refresh → spacing/rhythm → color recalibration (desaturate,
   keep the brand accent) → motion layer on existing components → hero/key
   section recomposition → full block replacement (last resort).

## 8. Pre-flight check (run before calling it done)

- [ ] Design read declared and dial values stated with reasoning, not silent
      defaults
- [ ] Zero em-dashes anywhere on the page
- [ ] One theme for the whole page, one accent color used identically
      everywhere, one corner-radius system
- [ ] Every CTA and form field passes WCAG AA contrast against its
      background
- [ ] No CTA label wraps to 2+ lines at desktop
- [ ] Hero fits the viewport with no forced scroll; hero stack ≤ 4 text
      elements
- [ ] No duplicate-intent CTAs on the page
- [ ] Section 6's tell list re-checked against the actual output, not just
      against intent

## Relationship to other skills

- [`web-visual-design`](../web-visual-design/SKILL.md) — BLS's own process
  for code-first client sites (discovery, palette, footer requirements,
  the mozao incident). Run that skill's process first; use this skill as the
  independent taste/quality pass on the result.
- [`ux-ui-designer`](../ux-ui-designer/SKILL.md) — product work with a
  Figma/dev-handoff pipeline. This skill's tells list still applies to any
  marketing surface inside that product (landing pages, pricing pages).
- [`web-interface-guidelines`](../web-interface-guidelines/SKILL.md) —
  the technical-correctness counterpart to this skill's aesthetic-judgment
  focus. Run both as independent passes: this skill asks "does it look
  designed," that one asks "does it actually work right" (keyboard,
  forms, performance, ARIA).
- [`brand-designer`](../brand-designer/SKILL.md) — upstream of this skill;
  provides the locked palette/typeface/voice this skill's rules get applied
  against, instead of this skill inventing one.
- [`playwright-mcp`](../../tools/playwright-mcp/SKILL.md) — use to actually
  see the rendered result (screenshots at desktop/tablet/mobile) before
  running this skill's pre-flight check; don't sign off from reading the
  code alone.

## A note on Impeccable

[pbakaus/impeccable](https://github.com/pbakaus/impeccable) (69k+ stars) is
a real, more ambitious product than this skill: a versioned CLI engine, a
browser extension for live visual iteration, 61 deterministic detector
rules, and 24 slash-style commands (`audit`, `critique`, `polish`, `bolder`,
`harden`, and more), installed via `npx impeccable install`. That's a
different shape of tool than this repo's skills — every skill here is a
self-contained markdown file with no runtime dependency, while Impeccable
requires installing and running its own binary per project.

Rather than build a shallow imitation of a tool we can't actually run, this
skill adopted Impeccable's portable ideas directly (the Modes framework in
Section 1, the craft-floor additions in Section 6.A) with credit, and left
the rest alone. If a project already has Impeccable installed, defer to its
live commands over this skill's static checklist — its detector rules run
against the actual rendered page, this skill's don't.
