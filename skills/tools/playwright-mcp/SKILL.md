---
name: playwright-mcp
description: Set up and use the Playwright MCP server for stateful, exploratory browser sessions — visual design review, self-healing/ad-hoc test exploration, long-running autonomous browser workflows. Use playwright-cli instead for routine coding-agent browser automation and writing Playwright tests; use this skill specifically when the task needs persistent browser state across many turns or an actual rendered screenshot to judge.
source: adapted from https://github.com/microsoft/playwright-mcp (official Microsoft docs) and https://github.com/AslanMazhidov/design-review-skill (MIT, workflow structure)
---

# Playwright MCP

## Read this first: MCP vs CLI

This repo already has [`playwright-cli`](../playwright-cli/SKILL.md), and
for most coding-agent browser automation **it's the right default, not this
skill** — this is Microsoft's own stated guidance, not a house preference:

> CLI invocations are more token-efficient: they avoid loading large tool
> schemas and verbose accessibility trees into the model context... MCP
> remains relevant for specialized agentic loops that benefit from
> persistent state, rich introspection, and iterative reasoning over page
> structure, such as exploratory automation, self-healing tests, or
> long-running autonomous workflows.

Use **`playwright-cli`** for: writing/running Playwright tests, routine
navigation-and-assert automation, anything where the task is well-defined
upfront.

Use **this skill (MCP)** for: **visual design review** (the primary use
case that connects to this repo's design skills — see below), open-ended
exploration of an unfamiliar page's structure, or a long autonomous session
where holding browser state across many tool calls matters more than token
cost.

## Setup

```bash
claude mcp add playwright npx @playwright/mcp@latest
```

Requires Node.js 18+. Verify the connection by checking for tools prefixed
`mcp__playwright__*` (`browser_navigate`, `browser_resize`,
`browser_take_screenshot`, `browser_snapshot`, and others). If they're not
available, tell the user and offer the setup command above rather than
falling back to guessing about the page.

## Primary use case: visual design review

This is the concrete link to `frontend-taste`, `web-interface-guidelines`,
and `image-to-code` — those skills define *what* to check; this skill is
*how to actually see it* instead of judging from source code alone.

1. **Navigate** to the file or URL under review (`browser_navigate`, using a
   `file://` URL for a local file).
2. **Capture screenshots at three widths** (`browser_resize` then
   `browser_take_screenshot`), matching this repo's standard breakpoints:
   - Desktop: 1440×900
   - Tablet: 768×1024 (or 375×812 for a mobile-first breakpoint check)
   - Mobile: 375×812
3. **Read the screenshots back** through the Read tool. This step is not
   optional — capturing an image without reading it back is not "seeing"
   the design, it's producing a file nobody looked at.
4. **Snapshot the accessibility tree** (`browser_snapshot`) to check
   semantics and hierarchy independent of visual noise — this is also how
   [`web-interface-guidelines`](../../design/web-interface-guidelines/SKILL.md)'s
   ARIA/semantics items actually get verified rather than assumed from the
   markup.
5. **Audit against this repo's existing checklists**, not an ad-hoc list:
   [`frontend-taste`](../../design/frontend-taste/SKILL.md)'s pre-flight
   check for aesthetic/AI-tells, and
   [`web-interface-guidelines`](../../design/web-interface-guidelines/SKILL.md)
   for technical correctness. Don't invent a third, parallel checklist —
   these two already cover typography, contrast, spacing, hierarchy,
   responsive behavior, and interaction correctness in more depth than a
   generic review pass would.
6. **Report concretely, not vaguely.** "The hero H1 is 32px against an 18px
   body — the hierarchy reads flat, bump the H1 to at least 56px," not "the
   typography feels off." Cite the specific check that failed.
7. **Fix, then re-verify.** After applying a fix, re-run steps 1-4 on the
   same page. One pass rarely gets everything; don't declare done from
   reading the diff, confirm it against a fresh screenshot.

## Other use cases

- **Exploratory automation on an unfamiliar page** — when the DOM structure
  isn't known upfront and the agent needs to reason interactively about
  what's on the page turn by turn, the accessibility-tree-based approach
  here is more forgiving than CLI's terser snapshots.
- **Long-running autonomous workflows** where persistent browser state
  (staying logged in, keeping a multi-step flow open) across many tool calls
  outweighs the token cost of MCP's larger schemas.

## Relationship to other skills

- [`playwright-cli`](../playwright-cli/SKILL.md) — the default for
  everything that isn't the use cases above; read the "MCP vs CLI" section
  first before reaching for this skill.
- [`frontend-taste`](../../design/frontend-taste/SKILL.md) and
  [`web-interface-guidelines`](../../design/web-interface-guidelines/SKILL.md)
  — the checklists this skill's review workflow audits against. This skill
  doesn't carry its own separate design checklist by design.
- [`image-to-code`](../../design/image-to-code/SKILL.md) — that skill's
  "verify, don't assume" step is this skill's screenshot-and-compare
  workflow in practice.
- [`figma-mcp`](../figma-mcp/SKILL.md) — use after generating code from a
  Figma frame to confirm the rendered result actually matches, rather than
  trusting the structured-data generation was correct.
