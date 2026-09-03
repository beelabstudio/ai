# BEELABSTUDIO — AI Agent Instructions

> **Global Rule — Language**: All documentation, comments, commit messages, README files,
> PR descriptions, and any other written artifact must be in English — regardless of the
> language used in the request.

## Project Context

This is the central repository for BEELABSTUDIO organisation standards, containing:

- **Shared AI skills** (`skills/`) — reusable skill definitions for any AI assistant
- **Agent configuration** (`.claude/`) — orchestrator, commands, and rules
- **GitHub templates** (`.github/`) — PR template and org-level defaults

## Quick Start

1. Use the **Orchestrator** as the entry point for any complex or multi-step task
2. Run `/project:review` before submitting PRs
3. Check `.claude/rules/` for coding standards

## Available Resources

### Shared AI Skills

The complete and up-to-date skills catalog is the single source of truth:

[INDEX.md](./INDEX.md)

### Agents

| Agent | Path | Use When |
|-------|------|----------|
| Orchestrator | `.claude/agents/orchestrator.md` | Entry point for any complex task — routes to the right skill |
| Code Reviewer | `.claude/agents/code-reviewer.md` | Code quality and standards review |
| Security Auditor | `.claude/agents/security-auditor.md` | Security vulnerability assessment |

### Commands

| Command | Path | Usage |
|---------|------|-------|
| `/project:review` | `.claude/commands/review.md` | Pre-PR review checklist |
| `/project:fix-issue` | `.claude/commands/fix-issue.md` | GitHub issue resolution workflow |
| `/project:deploy` | `.claude/commands/deploy.md` | Deployment workflow |

### Rules (always in effect)

| Rule | Path | Covers |
|------|------|--------|
| Code Style | `.claude/rules/code-style.md` | Naming, formatting, TypeScript standards |
| Testing | `.claude/rules/testing.md` | Test structure, coverage requirements |
| API Conventions | `.claude/rules/api-conventions.md` | REST API standards, versioning |

## Standards

### Code Style

- Files: `kebab-case` (`my-component.tsx`)
- Components: `PascalCase` (`MyComponent`)
- Functions: `camelCase` (`myFunction`)
- Constants: `UPPER_SNAKE_CASE` (`API_BASE_URL`)

### Git Workflow

- Branch from `main`
- Branch naming: `feature/`, `fix/`, `hotfix/`, `chore/`
- Conventional Commits: `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`
- Squash merge via PR — no direct push to `main`

### Commit Message Format

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

Types: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `perf`, `ci`

### No AI Attribution

- Commit messages and PR descriptions must **not** contain any AI-attribution content — no
  `Co-Authored-By: <AI name>`, no `Generated with <AI tool>` footers, no session/task links,
  no "🤖" markers, and no mention that the change was authored or assisted by an AI tool.
- Commits should read as if authored by the human contributor alone.
- If a tool defaults to adding this kind of footer, strip it before committing/opening the PR.

### Testing Requirements

- Minimum 80% code coverage
- 100% coverage for critical paths
- All public APIs must have tests
- Unit, integration, and minimal E2E tests

## Guidelines for AI Assistants

When working in this repository:

1. Use the **Orchestrator** as the entry point for complex or multi-step tasks
2. Check relevant skills from `skills/` based on the domain
3. Follow the rules in `.claude/rules/` at all times
4. Ensure test coverage for every new code change
5. Use conventional commit format for all commits
6. Maintain English for all written artifacts without exception
7. Do not add AI-attribution lines (Co-Authored-By, Generated with, session links, etc.) to commits or PRs
