# CLAUDE.md

Team instructions for Claude Code in this repository.

This file contains shared instructions that apply to all team members working with Claude Code in this project.

## Project Overview

[Brief description of the project]

## Team Standards

- **Use the Orchestrator** (`.claude/agents/orchestrator.md`) as the entry point for complex or unclear requests
- Follow the conventions defined in `.claude/rules/`
- Use slash commands from `.claude/commands/` for common tasks
- Skills in `.claude/skills/` are auto-invoked based on context

## Quick Start

1. **Start with the Orchestrator** for any multi-step or unclear task
2. Run `/project:review` before submitting PRs
3. Check `.claude/rules/code-style.md` for coding standards
4. See `.claude/rules/testing.md` for testing requirements

## Available Resources

| Resource | Location | Purpose |
|----------|----------|---------|
| Orchestrator Agent | `.claude/agents/orchestrator.md` | Central coordinator for all requests |
| Code Reviewer | `.claude/agents/code-reviewer.md` | Quality-focused code review |
| Security Auditor | `.claude/agents/security-auditor.md` | Security vulnerability assessment |
| Review Command | `.claude/commands/review.md` | Pre-PR review checklist |
| Fix Issue Command | `.claude/commands/fix-issue.md` | GitHub issue resolution workflow |
| Deploy Command | `.claude/commands/deploy.md` | Deployment workflow |
| Code Style Rules | `.claude/rules/code-style.md` | Naming and formatting standards |
| Testing Rules | `.claude/rules/testing.md` | Test coverage requirements |
| API Conventions | `.claude/rules/api-conventions.md` | REST API standards |
| Security Review | `.claude/skills/security-review/SKILL.md` | Auto security checks |
| Deploy Skill | `.claude/skills/deploy/SKILL.md` | Auto deployment guidance |
| Orchestrator Skill | `.claude/skills/orchestrator/SKILL.md` | Auto coordination |

## Skills Catalog

The complete and up-to-date skills catalog is the single source of truth — always read it before suggesting or using any skill:

@INDEX.md

## Links

- [Repository README](./README.md)
- [Contributing Guidelines](./CONTRIBUTING.md)

## Multi-Assistant Support

This project is configured for both **Claude Code** and **GitHub Copilot**:
- Claude uses `.claude/` folder and `CLAUDE.md`
- Copilot uses `.github/copilot-instructions.md`
- Shared skills are in `skills/` folder
