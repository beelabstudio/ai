# BEELABSTUDIO — AI Standards

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
└── tools/                          # Internet access & browser automation
contexts/                           # Reusable working modes
├── dev.md                          # Active development mode
├── research.md                     # Exploration mode
└── review.md                       # Code review mode
SOUL.md                             # Core identity and principles
CLAUDE.md                           # Team instructions for Claude
CLAUDE.local.md                     # Personal overrides (gitignored)
```

> **Symlinks:** the canonical `agents/`, `commands/`, `rules/`, and `skills/` content lives
> at the top level. `.claude/` contains `settings.json` plus symlinks back to those folders
> so Claude Code discovers them from its native `.claude/` paths.

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
| `tools/` | External-access tooling (internet access, browser automation) |

## 🤖 Using with AI Assistants

### GitHub Copilot

Copy `.github/copilot-instructions.md` to your project or reference skills from this repository.

### Claude Code

New projects should start from the AGENTS.md template in the
`beelabstudio-brain` second brain
(`99-meta/templates/AGENTS.md`), not from a blank file — it bakes in the
infra/git/language conventions so a fresh session doesn't have to
rediscover them. This repo includes the full reusable structure
(`agents/`, `commands/`, `rules/`, plus `.claude/settings.json`) for
reference:

1. **Copy the structure** to your project:
   ```bash
   cp -r ~/repos/ai/agents ~/repos/ai/commands ~/repos/ai/rules ./
   mkdir -p .claude && cp ~/repos/ai/.claude/settings.json .claude/
   cp ~/repos/ai/AGENTS.md ./  # then adapt via the brain template above
   ```

2. **Customize** `AGENTS.md` with your project description

3. **Use the Orchestrator** for any complex task - it will route to the right resources

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

**For Claude Code** (copy and adapt `agents/`, `commands/`, `rules/`, and `.claude/settings.json`)

## 🏗️ Default project stack

- HTML5 + CSS3 + Vanilla JavaScript (ES6+)
- GitHub Actions for CI/CD
- Conventional Commits (`feat:`, `fix:`, `chore:`, `docs:`)
- Branches: `feature/`, `fix/`, `hotfix/`, `chore/`
