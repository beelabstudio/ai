# Bee Lab Studio — AI Standards

This is the repository developers actually clone and reference day to day:
`git clone git@github.com:beelabstudio/ai.git ~/repos/ai`. Every project's
`AGENTS.md` points here (`~/repos/ai/skills/...`).

**Correction (2026-09-15):** an earlier version of this note described
`beelabstudio/.github` as a separate repository kept in manual sync with
this one. That was wrong — `git push` to `beelabstudio/.github` returns
`This repository moved. Please use the new location: beelabstudio/ai`, and
`gh repo view beelabstudio/.github` resolves to this same repo. **They are
the same repository; `.github` was this repo's name before it was renamed
to `ai`.** GitHub keeps old-name pushes/clones working as a redirect, which
is why a stale local clone at `~/repos/.github` kept appearing to work.

One real consequence of the rename: GitHub's org-wide defaults (a PR
template that auto-applies to repos without their own, community health
file fallbacks) require a repo **literally named** `.github` in the org.
Since this repo is now named `ai`, no such repo currently exists, so that
auto-fallback behaviour is inactive — repos without their own
`PULL_REQUEST_TEMPLATE.md` no longer get one automatically. Whether that
matters is a call for whoever owns the GitHub org: fine to ignore if every
repo defines its own PR template anyway, or worth creating a small,
dedicated `beelabstudio/.github` repo (holding only
`PULL_REQUEST_TEMPLATE.md`) if the auto-fallback is wanted back.

## ⚠️ Global Rule — Language

> **All documentation, comments, commit messages, README files, PR descriptions, and any other written artifact must be in English — regardless of the language used in the request.**

## 📁 Structure

```
.github/
├── PULL_REQUEST_TEMPLATE.md        # Generic Pull Request template
└── copilot-instructions.md         # GitHub Copilot instructions
.claude/                            # Claude Code native config
└── settings.json                   # Permissions, hooks, model, theme
agents/                             # Specialized agents
├── orchestrator.md                 # Central coordinator
├── code-reviewer.md                # Code quality reviewer
└── security-auditor.md             # Security auditor
commands/                           # Slash commands
├── review.md                       # → /project:review
├── fix-issue.md                    # → /project:fix-issue
└── deploy.md                       # → /project:deploy
rules/                              # Coding standards (code style, testing, API, security)
├── code-style.md                   # Naming and formatting standards
├── testing.md                      # Test requirements
├── api-conventions.md              # REST API standards
└── security.md                     # Prompt defense, secrets, input validation
skills/
├── c-level/                        # Executive leadership AI skills
├── content/                        # Technical writing & research skills
├── design/                         # Interface, UX/UI, visual design & motion skills
├── domain/                         # Domain-specific AI skills
├── foundation/                     # Engineering roles & practices
├── marketing/                      # SEO & growth skills
├── process/                        # Workflow and process skills
├── security/                       # Security review & auditing
└── tools/                          # Internet access, browser automation & local scraping infra
contexts/                           # Reusable working modes — reference only, not
├── dev.md                          # auto-loaded by Claude Code; the Orchestrator
├── research.md                     # reads the matching file when a request calls
└── review.md                       # for a mode switch (see agents/orchestrator.md)
SOUL.md                             # Core identity and principles
CLAUDE.md                           # Team instructions for Claude
CLAUDE.local.md                     # Personal overrides (gitignored)
```

> **Symlinks:** the canonical `agents/`, `commands/`, and `rules/` content lives at the
> top level; every file under `.claude/agents/`, `.claude/commands/`, and
> `.claude/rules/` is a per-file symlink back to it, so editing the top-level file is
> enough — Claude Code discovers them from its native `.claude/` paths and there's
> nothing to keep in sync. `skills/` is different: it's a reference catalog, not
> mirrored into `.claude/` wholesale — only [Task Observer](skills/process/task-observer/SKILL.md)
> is individually symlinked into `.claude/skills/`, for the reason explained in
> "Activating Task Observer in another project" below.

## 📋 Pull Request Template

The `.github/PULL_REQUEST_TEMPLATE.md` file is automatically applied to all organisation repositories that **do not have** their own template.

Includes:
- Type of change (feat, fix, hotfix, refactor, style, docs, chore, i18n)
- References to issues, design and staging
- Code quality checklist
- Deploy checklist
- Space for visual evidence and reviewer notes

## 🤖 Shared AI Skills

The `skills/` directory contains reusable AI skill definitions (`SKILL.md` files) organised by category. Clone this repository locally and reference skills from your project's `copilot-instructions.md`.

| Category | Description |
|----------|-------------|
| `c-level/` | Executive leadership skills (CEO, CTO, CFO, CPO, COO, CISO) |
| `content/` | Content and knowledge work (technical writing, research synthesis) |
| `design/` | Interface, UX/UI, visual design & motion/animation skills (Apple-style design, animation, transitions, prototyping, UI libraries) |
| `domain/` | Specialised domain knowledge (nutrition, databases, migrations…) |
| `foundation/` | Core engineering roles and practices (software engineer, architect, QA, DBA, API design, TDD, onboarding…) |
| `marketing/` | Marketing and growth (SEO) |
| `process/` | Workflow and process skills (git, business analysis, fiscal…) |
| `security/` | Security review and configuration auditing |
| `tools/` | External-access tooling (internet access, browser automation, local scraping infra) |

## 🤖 Using with AI Assistants

### GitHub Copilot

Copy `.github/copilot-instructions.md` to your project or reference skills from this repository.

### Claude Code

New projects should start from the AGENTS.md template in the
`beelabstudio-brain` second brain
(`99-meta/templates/AGENTS.md`), not from a blank file — it bakes in the
infra/git/language conventions so a fresh session doesn't have to
rediscover them. This repo is "the shared brain that every project's
`AGENTS.md` points to" (see `SOUL.md`) — **point at it, don't copy it.**
Claude Code only auto-discovers subagents, commands, and rules from inside
a project's own `.claude/` folder (`.claude/agents/`, `.claude/commands/`,
`.claude/rules/`) — a plain top-level `agents/`, `commands/`, or `rules/`
folder is invisible to it, no matter what's inside. Symlinking `.claude/`
straight at this repo's copies means every consuming project always runs
the current org standard, with nothing to keep in sync by hand:

1. **Wire this repo into your project's `.claude/`:**
   ```bash
   mkdir -p .claude
   ln -s ~/repos/ai/agents .claude/agents
   ln -s ~/repos/ai/commands .claude/commands
   ln -s ~/repos/ai/rules .claude/rules
   ln -s ~/repos/ai/contexts ./contexts   # top-level, not .claude/ — see "Contexts" note below
   cp ~/repos/ai/.claude/settings.json .claude/settings.json
   cp "~/repos/beelabstudio-brain/Bee Lab Studio - Brain/99-meta/templates/AGENTS.md" ./AGENTS.md
   ```
   `settings.json` and `AGENTS.md` are **copied**, not symlinked, because
   they're meant to be adapted per project (permissions, model, project
   description) — `agents/`, `commands/`, `rules/`, and `contexts/` are
   **symlinked**, because they're meant to always match this repo exactly.
   If a project genuinely needs to override one specific agent, command, or
   rule, replace just that symlinked entry with a real local file — don't
   turn the whole folder into a copy to do it.

   `contexts/` needs its own top-level symlink (not `.claude/contexts/`):
   Claude Code has no native discovery for it at all (see "Contexts" in
   `AGENTS.md`) — `agents/orchestrator.md`'s routing table reads it as a
   plain relative path, `contexts/dev.md`, resolved against your project's
   working directory, not against `.claude/`.

2. **Customize** `AGENTS.md` with your project description — fill in every
   `{{placeholder}}` left by the template copied above (project name, one-paragraph
   overview naming the actual stack — see "Default project stack" below if it isn't
   decided yet —, domain/DNS status, git-workflow tier, project status). The template's
   own header comment has the full instructions, including the two symlinks it
   requires (`CLAUDE.md` and `.github/copilot-instructions.md`) — follow them, then
   delete that comment block once done.

3. **Use the Orchestrator** for any complex task - it will route to the right resources.
   No setup action is needed beyond step 1: `.claude/settings.json` already sets
   `"agent": "orchestrator"` as the default (a real, documented setting —
   see [settings reference](https://code.claude.com/docs/en/settings-reference.md)),
   so once `.claude/agents/orchestrator.md` resolves through the symlink above,
   any Claude Code session there opens with the Orchestrator active and
   routes multi-step requests to the right skill, agent, or command on its own.

4. **Restart the Claude Code session** after step 1 — subagents, commands,
   and rules are only picked up at session start, not mid-session.

### Activating Task Observer in another project

Unlike `agents/`, `commands/`, and `rules/`, **skills are not wired into
`.claude/` by default** — `skills/` is a much larger reference catalog
(60+ skills), and loading all of it into every project's context would be
wasteful. Being cataloged there doesn't make a skill active anywhere; it's
opt-in per skill. One skill in this catalog,
[Task Observer](skills/process/task-observer/SKILL.md) (vendored from
[rebelytics/one-skill-to-rule-them-all](https://github.com/rebelytics/one-skill-to-rule-them-all),
CC BY 4.0), watches work sessions for recurring patterns and corrections
worth turning into new or improved skills. It's wired live in this repo's
own `.claude/skills/task-observer` (a symlink to the source under
`skills/process/`, kept in sync automatically) — that only makes it active
for sessions working *on this `ai` repo itself*, not anywhere else. To
activate it in another project too:

1. Symlink it in — do not copy the files, so the project always tracks the
   latest version in `~/repos/ai`:
   ```bash
   mkdir -p .claude/skills
   ln -s ~/repos/ai/skills/process/task-observer .claude/skills/task-observer
   ```
2. Reference it explicitly in that project's own `AGENTS.md` — its
   frontmatter states that description-matching alone isn't a reliable
   trigger. If `AGENTS.md` already has a "Shared skills" table (from the
   `beelabstudio-brain` template), add a row pointing at
   `.claude/skills/task-observer/SKILL.md`; otherwise add a short section
   saying its Session Start Protocol should run at the start of any
   multi-step session.
3. Restart the Claude Code session in that project — new skills are only
   picked up at session start.

`rules/security.md` (this repo's and, if linked per step 1 above, the
target project's) takes precedence over the skill's own instruction to
retry a denied or failed tool call through an alternate interface — that
override is already written inline in the vendored `SKILL.md`, right under
its attribution block, so nothing else needs to be added for it.

### Activating Google Maps Scraper in another project

[Google Maps Scraper](skills/tools/google-maps-scraper/SKILL.md) (vendored
from
[Mahanaicoach/google-maps-scraper-kit](https://github.com/Mahanaicoach/google-maps-scraper-kit),
MIT) follows the same symlink-in pattern as Task Observer above, but needs
one extra step because it drives a **local Docker container**, not just
instructions: after symlinking the skill into `.claude/skills/`, copy (not
symlink) its `docker-compose.yml` and `.env.example` into the project root
so `docker compose up -d` has a real file to read, and optionally copy its
four slash commands and merge its permission allowlist. The full sequence
— with the reasoning for copy-vs-symlink — is in that skill's own
"Activating it in a project" section, right after its attribution block.

### Setup

Clone this repository to use shared skills locally:

```bash
git clone git@github.com:beelabstudio/ai.git ~/repos/ai
```

Then reference skills from your project's AI configuration:

**For Copilot** (`copilot-instructions.md`):
```
~/repos/ai/skills/<category>/<skill-name>/SKILL.md
```

**For Claude Code**: symlink `.claude/agents`, `.claude/commands`, and `.claude/rules`
to this repo's copies, and top-level `contexts` too — see "Claude Code" above for the
full command and why a copy doesn't work the way a symlink does.

## 🏗️ Default project stack

If a project's own `AGENTS.md` doesn't specify a stack, **don't default to the list
below silently.** Invoke the [`software-architect`](skills/foundation/software-architect/SKILL.md)
skill (or [`cto`](skills/c-level/cto/SKILL.md), which routes to it for stack decisions —
see its "Skills to Activate" table) to ask about the project's actual requirements —
expected scale, team size, whether it needs a backend/database, hosting target,
content model — and propose the best-fit stack from that. Record the decision as an
ADR (`docs/adr/`, per `software-architect`'s ADR format) so the choice and its
trade-offs are on record, not just assumed.

The list below is the **fallback for the simplest recurring case** this org actually
builds — a static institutional or marketing site with no dynamic backend needs — not
a default to reach for before that conversation happens:

- HTML5 + CSS3 + Vanilla JavaScript (ES6+)
- GitHub Actions for CI/CD
- Conventional Commits (`feat:`, `fix:`, `chore:`, `docs:`)
- Branches: `feature/`, `fix/`, `hotfix/`, `chore/`
