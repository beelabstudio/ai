---
name: figma-mcp
description: Set up and use the official Figma MCP server to pull real design context (components, variables, layout) from a Figma file into code, instead of guessing from a screenshot or a verbal description. Use when a Figma link is shared, when a client hands over a Figma file, or when generated code needs to match an existing design system exactly.
source: adapted from https://github.com/figma/mcp-server-guide (official Figma docs)
---

# Figma MCP

Figma maintains an official remote MCP server that exposes structured design
data — components, variables, layout, tokens — directly to a coding agent, so
code generation works from the actual design system instead of a screenshot
guess or a verbal description of what a frame looks like.

## When to use this over other design skills

- A Figma link (or a specific frame/layer URL) is provided → use this skill.
- Only a screenshot/export image is available, no Figma access → use
  [`image-to-code`](../../design/image-to-code/SKILL.md) instead.
- No design exists yet and one needs to be established → use
  [`brand-designer`](../../design/brand-designer/SKILL.md) first.

## Setup

The server is remote — no local install, no binary to manage. Connect once
per client:

**Claude Code (CLI):**
```bash
claude mcp add --transport http figma https://mcp.figma.com/mcp
```
Or install the bundled plugin (includes the MCP config plus Figma's own
implementation/Code-Connect skills):
```bash
claude plugin install figma@claude-plugins-official
```

**VS Code / Cursor / other Streamable-HTTP clients:** point the client's MCP
config at `https://mcp.figma.com/mcp` (see the client's own MCP settings —
the URL is the same everywhere).

Check the connection by confirming `get_design_context` shows up as an
available tool; if it doesn't, restart the client.

**Rate limits apply** on read tools — free/View/Collab seats are capped at 6
calls/month; paid Dev/Full seats follow Figma's REST API tier-1 per-minute
limits. Write-to-canvas tools are exempt. Budget calls accordingly on a
capped account — don't re-fetch the same frame repeatedly across a session.

## Getting a frame into context

1. Copy the link to the specific frame or layer in Figma (not just the file
   link — the node ID in the URL is what the server needs to resolve).
2. Prompt with that URL: "implement the design at this link." The client
   can't navigate to the URL itself, but extracts the node ID for the MCP
   server.

## Key tools

- **`get_design_context`** — a structured React + Tailwind representation of
  the selected frame. Treat this as a starting point to translate into
  whatever framework/style the project actually uses, not as final code to
  paste in — the prompt controls the translation, the tool just supplies
  structure.
- **`get_variable_defs`** — extracts the variables/styles used in the
  selection (color, spacing, typography). Reach for this explicitly when the
  agent is emitting raw hex/px values instead of referencing the project's
  design tokens: "get the variable names and values used in this frame."

## Getting good output

**On the Figma file side** (can't control this if the file is a client's,
but flag it when advising a client on Figma hygiene):
- Real components for anything reused (buttons, cards, inputs) — ungrouped
  layers produce guesswork.
- Code Connect linking components to the actual codebase — without it, the
  model is guessing at how a component maps to code; with it, output reuses
  real project components.
- Variables for spacing/color/radius/typography, not hardcoded values on
  each layer.
- Semantic layer names (`CardContainer`, not `Group 5`).
- Auto layout used to express responsive intent — a fixed-position layer
  gives the server nothing to reason about for different viewport sizes.

**On the prompt side:**
- Name the target framework/styling system explicitly ("Use Chakra UI",
  "Generate SwiftUI").
- Name the target file path when adding to an existing project
  (`src/components/marketing/PricingCard.tsx`) — otherwise expect a new file
  instead of an edit to the right place.
- Name the project's existing layout primitives if it has them ("use our
  `Stack` component").
- If results come back as raw values instead of design tokens, explicitly
  ask for `get_variable_defs` output.

## Relationship to other skills

- [`image-to-code`](../../design/image-to-code/SKILL.md) — same
  exact-replication mandate, different source of truth. Use this skill when
  Figma access exists (structured data, exact tokens); fall back to
  `image-to-code` when only a flat image is available.
- [`brand-designer`](../../design/brand-designer/SKILL.md) and
  [`ux-ui-designer`](../../design/ux-ui-designer/SKILL.md) — when the Figma
  file *is* the design system (not just one page to implement), pull tokens
  via `get_variable_defs` into those skills' palette/typography
  documentation instead of re-deriving them by eye.
- [`playwright-mcp`](../playwright-mcp/SKILL.md) — after generating code
  from a Figma frame, use that skill to screenshot the actual rendered
  result and verify it matches, rather than trusting the generation was
  correct from the structured data alone.
