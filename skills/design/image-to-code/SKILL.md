---
name: image-to-code
description: Reconstruct working code from a reference screenshot, mockup, or design export with pixel-level fidelity, instead of a loose visual interpretation. Use when the user pastes a screenshot/image and asks for it to be built, matched, or cloned — a client's existing site, a Figma export, a competitor page, a design file.
source: prompting approach distilled from https://github.com/abi/screenshot-to-code (MIT)
---

# Image to Code

Building "something inspired by" a reference image is a different task from
building "this exact page." Default to the exact-replication mandate below
whenever a reference image is provided — only relax it if the user explicitly
asks for a loose interpretation, a different framework's idioms, or a
"redesign inspired by."

## The replication mandate

- **The output must look exactly like the reference**, not an
  approximation of its vibe. Match spacing, proportions, type scale, and
  color exactly where the image resolution allows it.
- **Use the exact text from the image.** Don't paraphrase copy, "improve"
  headlines, or fill gaps with placeholder copy when the real text is
  legible in the reference.
- **Real assets over invented ones.** Extract actual image assets from the
  reference where possible (logos, photography, icons cropped out of the
  screenshot) rather than generating replacements from scratch. Only
  generate a replacement asset when the original is genuinely
  unextractable — occluded by another element, or itself a background
  texture with no clean crop.
- **Inspect every extracted asset before using it.** A crop that grabbed
  the wrong bounding box (half a logo, a sliver of an adjacent element) is
  worse than a generated placeholder.
- **Upscale, don't stretch.** If an extracted asset is visibly low-resolution
  and needs to render larger than its source, upscale it properly (an
  image tool, not CSS `width`/`height` stretching, which visibly blurs).

## Multiple images in one request

- If the images are clearly different pages of one site, build them as
  distinct, linked pages.
- If they're different tabs or views of one app, connect them with the
  matching navigation.
- If they look unrelated, scaffold clearly separated sections ("View 1",
  "View 2") rather than forcing a single flow.
- For mobile screenshots specifically: exclude the device frame and browser
  chrome from the build — reconstruct only the actual UI content.

## When a design system doc is available

If the project has a `DESIGN.md`, a locked palette/typography from
[`brand-designer`](../brand-designer/SKILL.md), or another explicit design
system reference, that reference **takes priority over what the image
visually implies** when the two conflict — e.g. the image shows a color that
doesn't exist in the locked palette because of screen calibration or JPEG
compression; use the documented hex, not the pixel-sampled one. Curated
real-brand `DESIGN.md` examples for calibrating tone/structure of such a
doc are available at
[VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md)
(MIT) — useful reference material, not something to vendor into a client
project directly.

Without a design system doc, the image is the source of truth for visual
decisions; don't substitute your own stylistic preferences for what's
actually shown.

## Verify, don't assume

Building from a static image is error-prone in ways that are easy to miss
without checking: a flex layout that looks right in isolation but doesn't
match the reference's actual proportions, a font-weight that's visually
close but not identical, a color read wrong because of image compression.

- After building, compare the rendered output against the reference image
  side by side (a screenshot tool, browser preview, or asking the user to
  confirm) rather than declaring done from reading the code.
- If you spot a mismatch — broken layout, wrong spacing, wrong color, a
  missing element — fix it and re-compare. Don't ship the first pass
  unchecked.
- Flag genuine ambiguity rather than guessing silently: a cropped element at
  the image edge, a color that could plausibly be one of two close hex
  values, text that's illegible at the given resolution.

## Stack defaults

Match whatever the project already uses. If there's no existing project to
match and the user hasn't specified a stack, default to plain HTML + Tailwind
(via CDN for a single-file prototype, or the project's existing build for a
real page) — it reproduces arbitrary visual layouts with the least
translation loss between "what the image shows" and "what the code says."

## Relationship to other skills

- [`brand-designer`](../brand-designer/SKILL.md) — if the reference image
  represents a brand's identity being established for the first time
  (not just a page to clone), that skill's process comes first; this skill
  is for reconstructing a *specific* image, not inventing a system from one.
- [`frontend-taste`](../frontend-taste/SKILL.md) — does not apply while
  matching a reference exactly (the reference is the taste authority, not
  the model's own judgment). Load it afterward only if the user asks to
  *improve* on the reference, not just reproduce it.
- [`web-visual-design`](../web-visual-design/SKILL.md) — that skill's
  "AI-generated tells" checklist assumes the model is inventing the design;
  it doesn't apply to a faithful reconstruction task.
- [`figma-mcp`](../../tools/figma-mcp/SKILL.md) — if the reference is
  actually a Figma file (not a flat screenshot), use that skill instead —
  structured component/variable data beats reading pixels every time.
- [`playwright-mcp`](../../tools/playwright-mcp/SKILL.md) — for the
  "verify, don't assume" step above: screenshot the rendered result and
  compare it against the reference image directly, rather than reading the
  code and declaring it correct.
