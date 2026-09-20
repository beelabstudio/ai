---
name: web-visual-design
description: Use before writing any HTML/CSS for a real client website (landing page, institutional site) built directly in code, without a Figma handoff. Defines Bee Lab Studio's bar for "doesn't look AI-generated" and the process to get there.
---

# Web Visual Design (code-first sites)

## When this applies

Most Bee Lab Studio client work is a **small institutional or marketing
site** (Astro, plain HTML/CSS/JS, or similar) built **directly in code** by
an agent — no designer, no Figma file, no handoff. This skill is for that
case.

If the project has an actual product team, personas, and a Figma pipeline
(e.g. UFlowApp), use `~/repos/ai/skills/design/ux-ui-designer/SKILL.md`
instead — that one assumes a design/dev split this skill doesn't.

**Load this before writing the first line of layout/CSS**, not after. A
redesign pass costs more than getting the direction right once — this
skill exists because that already happened once (see the `mozao` project
history in the second brain: a first version shipped with a visibly
AI-generated look, and had to be rebuilt).

## The problem this solves

Left to its own defaults, a coding agent reaches for the same handful of
looks every time, because they're the median of its training data, not
because they suit the client. The result is instantly recognizable as
"AI-made" to anyone who's seen a few dozen of these — which, increasingly,
is every client. Shipping that look undermines the actual pitch: that Bee
Lab Studio designs something specific to the client, not a template.

## Tells to avoid (the "AI-generated" checklist)

If you catch yourself doing any of these **without a specific reason tied to
the client's brand**, stop and reconsider:

- **Emoji as icons** in a real interface (service cards, feature lists, nav)
  — emoji render inconsistently across platforms and read as a placeholder,
  not a finished icon system. Use real SVG icons (self-host or a proper
  library — Lucide, Heroicons, Phosphor) or nothing.
- **Blurred gradient "blob" shapes** behind a hero section.
- **`rounded-2xl` + soft drop-shadow on every card**, uniformly, regardless
  of whether that element actually needs to look "lifted."
- **Pill-shaped buttons as the unquestioned default.**
- **A purple-to-blue (or similar) gradient hero on white.**
- **Inter or Space Grotesk chosen because they're "safe"** — pick a typeface
  pairing that says something about *this* client, not the default any
  model would reach for.
- **Centering everything.** Most content reads better left-aligned with a
  real grid.
- **Warm cream background (`#F4F1EA`-ish) + serif display + terracotta
  accent** — this exact combination is its own cliché; using terracotta
  because the client's brand palette calls for it is fine, using it because
  it's the "editorial-looking" default is not.
- **Accent-color vertical bar/rail as decoration on every card** with no
  structural reason for it.
- **Broadsheet hairline rules + dense multi-column text** used as a generic
  "serious" register, unrelated to the client's actual content.

None of these are permanently forbidden — the point is that each choice
should trace back to something true about *this* client, not be the
reflexive first idea.

## Process

1. **Ground it in the subject.** Before touching layout, write down: who is
   this for, what's the one thing this page needs someone to do or believe,
   and what's specific to this business (its actual services, its actual
   tone, a real detail only this client has — not a generic stand-in). Pull
   this from the second brain's research/ficha for the project, not from
   guessing.
2. **Pick a real palette.** 4–6 named hex values, derived from the client's
   brand direction (if the second brain's research already recommends one —
   e.g. a specific palette for a specific reason — start there instead of
   inventing a new one). A neutral should be chosen (a grey with a slight
   hue bias toward the accent), not defaulted to.
3. **Pair typefaces deliberately.** One display face with actual character,
   one body face for readability, a monospace/utility face only if the
   content needs one (data, prices, code). State *why* each was picked for
   this client in one line before writing any CSS.
4. **Use real content everywhere**, including in early drafts — real service
   names, real prices (or the client's actual placeholders, clearly marked
   as such), never `lorem ipsum` or "Service 1 / Service 2."
5. **Match treatment to the ask.** A one-page institutional site with a
   contact form is closer to a well-composed document than to an app — real
   typographic hierarchy and considered spacing, not a landing-page-builder
   template with a giant animated hero. Reserve a bolder editorial treatment
   (a real hero moment, more animation, a visual risk) for projects that are
   actually pitching something visually — a product launch, a campaign page.
6. **Build both light and dark rendering if the stack supports it**
   (`prefers-color-scheme`), same as any other Bee Lab Studio deliverable —
   don't ship a page that's unreadable in a visitor's dark-mode browser.
7. **Self-check against the tells list above before calling it done.** If
   more than one or two are present, that's the signal to revise, not ship
   and iterate later — a client-facing site is not a draft.

## Icons specifically

Since this comes up constantly: for a real site, use an actual icon set
(Lucide, Heroicons, Phosphor — all have permissive licenses and SVG/React
exports) inlined as SVG or imported as components. Never emoji glyphs
(`🔧`, `🏠`, `💡`) standing in for icons in a shipped interface — they're
fine in this skill file, in chat, or in an internal doc, not in client-facing
UI.

## Footer requirements (every client site)

Two rules that apply regardless of visual direction, added after a real
incident on the `mozao` site — its footer shipped with "Mozão está em fase
de lançamento — nome e identidade visual em validação" visible to any
visitor:

1. **Never put internal project-status language in visible copy.** Things
   like "in launch phase", "name/identity under validation", "content
   pending", or any other note meant for the team belong in `AGENTS.md`, the
   second brain, or a code comment — never in a `<p>` a visitor can read.
   This kind of text is exactly the sort of thing that quietly ships to
   production because nobody treats it as "real" copy requiring review. If a
   fact truly isn't confirmed yet (a number, a claim, a legal status), leave
   it out of the page rather than narrating the uncertainty to visitors.
2. **Every site's footer must include:**
   - **Social icons** for the channels that actually exist (WhatsApp,
     Instagram, Facebook, etc.) as real inline SVG — never emoji. For a
     channel that doesn't have a real profile yet, **hide that icon
     entirely** rather than linking it to `#` or a placeholder — a dead
     link is worse than no icon.
   - **A copyright line + Bee Lab Studio credit**, matching the pattern
     already used on `jp2-solucoes-construtivas`:
     `© {year} <Client Name>. Todos os direitos reservados.` plus a
     `Desenvolvido por Bee Lab Studio` link to `https://beelabstudio.com`.

## Relationship to other skills

- `~/repos/ai/skills/design/brand-designer/SKILL.md` — load this *first*
  when the client has no locked palette/typeface/voice yet. This skill
  assumes that foundation already exists (from the second brain's research
  or from that skill's output); it doesn't invent one from nothing.
- `~/repos/ai/skills/marketing/seo-specialist/SKILL.md` — technical SEO,
  independent of visual direction, load alongside this one.
- `~/repos/ai/skills/design/ux-ui-designer/SKILL.md` — for actual product
  work with a design/dev split (personas, Figma, dev handoff). Don't apply
  its Figma-centric process to a one-page marketing site; do borrow its
  accessibility floor (WCAG 2.1 AA) regardless of which skill you're
  primarily following.
- `~/repos/ai/skills/design/frontend-taste/SKILL.md` — run this *after*
  the process above as an independent taste/AI-tells pass; it carries a
  much longer, more mechanical checklist than the "Tells to avoid" section
  here.
- `~/repos/ai/skills/design/web-interface-guidelines/SKILL.md` — the
  technical-correctness counterpart (keyboard, forms, performance, ARIA);
  run alongside `frontend-taste`, not instead of it.
- `~/repos/ai/skills/design/image-to-code/SKILL.md` — when the task is
  reconstructing a specific reference image/screenshot rather than
  designing from a brief, that skill's exact-replication mandate overrides
  this skill's "avoid AI tells" process — the reference is the authority.
