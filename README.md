# BEELABSTUDIO — Organisation Standards

Central repository for **community health files**, templates, and quality standards across all BEELABSTUDIO projects.

## ⚠️ Global Rule — Language

> **All documentation, comments, commit messages, README files, PR descriptions, and any other written artifact must be in English — regardless of the language used in the request.**

## 📁 Structure

```
.github/
├── PULL_REQUEST_TEMPLATE.md        # Generic Pull Request template
└── copilot-instructions.md         # GitHub Copilot instructions
.claude/                            # Claude Code configuration
├── agents/                         # Subagent personas
│   ├── orchestrator.md             # Central coordinator
│   ├── code-reviewer.md            # Code quality reviewer
│   └── security-auditor.md         # Security auditor
├── commands/                       # Slash commands
│   ├── review.md                   # → /project:review
│   ├── fix-issue.md                # → /project:fix-issue
│   └── deploy.md                   # → /project:deploy
├── rules/                          # Modular instruction files
│   ├── code-style.md               # Naming and formatting standards
│   ├── testing.md                  # Test requirements
│   └── api-conventions.md          # REST API standards
├── skills/                         # Auto-invoked skills
│   ├── orchestrator/SKILL.md       # Coordination skill
│   ├── security-review/SKILL.md    # Security checks
│   └── deploy/SKILL.md             # Deployment guidance
└── settings.json                   # Shared configuration
skills/
├── domain/                         # Domain-specific AI skills
├── enabling/                       # Enabling / cross-cutting AI skills
├── foundation/                     # Foundation engineering role skills
└── process/                        # Workflow and process skills
CLAUDE.md                           # Team instructions for Claude
CLAUDE.local.md                     # Personal overrides (gitignored)
```

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
| `domain/` | Specialised domain knowledge (nutrition, databases, migrations…) |
| `enabling/` | Cross-cutting capabilities (SEO, UX/UI, integrations, research…) |
| `foundation/` | Core engineering roles (software engineer, QA, DBA, PM…) |
| `process/` | Workflow and process skills (git, business analysis, fiscal…) |

## 🤖 Using with AI Assistants

### GitHub Copilot

Copy `.github/copilot-instructions.md` to your project or reference skills from this repository.

### Claude Code

This repository includes a complete `.claude/` setup:

1. **Copy the structure** to your project:
   ```bash
   cp -r ~/repos/.github/.claude ./
   cp ~/repos/.github/CLAUDE.md ./
   ```

2. **Customize** `CLAUDE.md` with your project description

3. **Use the Orchestrator** for any complex task - it will route to the right resources

### Setup

Clone this repository to use shared skills locally:

```bash
git clone git@github.com:beelabstudio/.github.git ~/repos/.github
```

Then reference skills from your project's AI configuration:

**For Copilot** (`copilot-instructions.md`):
```
~/repos/.github/skills/<category>/<skill-name>/SKILL.md
```

**For Claude Code** (copy and adapt from `.claude/` folder)

## 🏗️ Default project stack

- HTML5 + CSS3 + Vanilla JavaScript (ES6+)
- GitHub Actions for CI/CD
- Conventional Commits (`feat:`, `fix:`, `chore:`, `docs:`)
- Branches: `feature/`, `fix/`, `hotfix/`, `chore/`
