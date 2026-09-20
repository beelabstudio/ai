---
name: architecture-decision-records
description: >
  Capture architectural decisions as structured ADRs (Architecture Decision Records). Detects
  decision moments, records context, alternatives considered, and rationale, and maintains an
  ADR index so future developers understand why the codebase is shaped the way it is. Use when
  a significant architectural choice is made, or when asked "why did we choose X?".
source: https://github.com/affaan-m/ECC
---

# Architecture Decision Records

Capture architectural decisions as they happen during coding sessions, so decisions don't live only in Slack threads, PR comments, or someone's memory.

## When to Activate

- User says "let's record this decision" or "ADR this"
- User chooses between significant alternatives (framework, library, pattern, database, API design)
- User says "we decided to..." or explains "we're doing X instead of Y because..."
- User asks "why did we choose X?" (read existing ADRs)
- During planning phases when architectural trade-offs are discussed

## ADR Format

Use the lightweight ADR format proposed by Michael Nygard, adapted for AI-assisted development:

```markdown
# ADR-NNNN: [Decision Title]

**Date**: YYYY-MM-DD
**Status**: proposed | accepted | deprecated | superseded by ADR-NNNN
**Deciders**: [who was involved]

## Context
[2-5 sentences describing the situation, constraints, and forces at play]

## Decision
[1-3 sentences stating the decision clearly]

## Alternatives Considered

### Alternative 1: [Name]
- **Pros**: ...
- **Cons**: ...
- **Why not**: [specific reason this was rejected]

## Consequences

### Positive
- ...

### Negative
- ...

### Risks
- [risk and mitigation]
```

## Workflow

### Capturing a New ADR

1. **Initialize (first time only)** — if `docs/adr/` does not exist, ask before creating the directory, a `README.md` index, and a `template.md`. Do not create files without explicit consent.
2. **Identify the decision** — extract the core architectural choice.
3. **Gather context** — what problem prompted this? What constraints exist?
4. **Document alternatives** — what else was considered and why rejected?
5. **State consequences** — what becomes easier or harder?
6. **Assign a number** — scan existing ADRs and increment.
7. **Confirm and write** — present the draft for review; only write after explicit approval.
8. **Update the index** — append to `docs/adr/README.md`.

### Reading Existing ADRs

- If `docs/adr/` doesn't exist: "No ADRs found in this project. Would you like to start recording architectural decisions?"
- If it exists, scan the index, read the matching ADR, and present the Context and Decision sections.

### Directory Structure

```
docs/
└── adr/
    ├── README.md              ← index of all ADRs
    ├── 0001-use-nextjs.md
    ├── 0002-postgres-over-mongo.md
    └── template.md            ← blank template for manual use
```

### Index Format

```markdown
# Architecture Decision Records

| ADR | Title | Status | Date |
|-----|-------|--------|------|
| [0001](0001-use-nextjs.md) | Use Next.js as frontend framework | accepted | 2026-01-15 |
```

## Decision Detection Signals

**Explicit**: "Let's go with X", "We should use X instead of Y", "Record this as an ADR".

**Implicit** (suggest an ADR — do not auto-create without confirmation):
- Comparing two frameworks/libraries and reaching a conclusion
- A database schema design choice with stated rationale
- Choosing between architectural patterns (monolith vs microservices, REST vs GraphQL)
- Deciding on an authentication/authorization strategy
- Selecting deployment infrastructure after evaluating alternatives

## What Makes a Good ADR

**Do**
- Be specific — "Use Prisma ORM", not "use an ORM"
- Record the why — rationale matters more than the what
- Include rejected alternatives
- State consequences honestly — every decision has trade-offs
- Keep it short — readable in 2 minutes
- Use present tense — "We use X", not "We will use X"

**Don't**
- Record trivial decisions (variable naming, formatting)
- Write essays — a context section over 10 lines is too long
- Omit alternatives — "we just picked it" is not valid rationale
- Backfill without noting the original date
- Let ADRs go stale — superseded decisions reference their replacement

## ADR Lifecycle

```
proposed → accepted → [deprecated | superseded by ADR-NNNN]
```

- **proposed**: under discussion, not yet committed
- **accepted**: in effect and being followed
- **deprecated**: no longer relevant
- **superseded**: replaced by a newer ADR (always link the replacement)

## Categories Worth Recording

| Category | Examples |
|----------|---------|
| Technology choices | Framework, language, database, cloud provider |
| Architecture patterns | Monolith vs microservices, event-driven, CQRS |
| API design | REST vs GraphQL, versioning, auth mechanism |
| Data modeling | Schema design, normalization, caching |
| Infrastructure | Deployment model, CI/CD, monitoring |
| Security | Auth strategy, encryption, secret management |
| Testing | Framework, coverage targets, E2E vs integration balance |
| Process | Branching strategy, review process, release cadence |
