# Skills Index

Complete reference of all AI skills and resources available in this repository.

## Quick Navigation

| Resource | Path | For |
|----------|------|-----|
| Copilot Instructions | `.github/copilot-instructions.md` | GitHub Copilot |
| Claude Orchestrator | `.claude/agents/orchestrator.md` | Claude Code entry point |
| Claude Commands | `.claude/commands/` | Claude slash commands |
| Claude Rules | `.claude/rules/` | Claude standards |
| Shared Skills | `skills/` | Both Copilot and Claude |

## Shared Skills Catalog

### Domain Skills

| Skill | Path | Description |
|-------|------|-------------|
| DBT Specialist | `skills/domain/dbt-specialist/SKILL.md` | Data transformation with DBT |
| Legacy Migration | `skills/domain/legacy-system-migration-specialist/SKILL.md` | System migration planning |
| MySQL Specialist | `skills/domain/mysql-specialist/SKILL.md` | MySQL optimization |
| Clinical Nutrition | `skills/domain/nutricionista-clinica/SKILL.md` | Nutrition domain knowledge |

### Enabling Skills

| Skill | Path | Description |
|-------|------|-------------|
| DevOps Engineer | `skills/enabling/devops-engineer/SKILL.md` | Infrastructure and CI/CD |
| Documentation Writer | `skills/enabling/documentation-writer/SKILL.md` | Technical writing |
| SEO Health Specialist | `skills/enabling/especialista-seo-saude/SKILL.md` | SEO for health sector |
| UX/UI Health | `skills/enabling/especialista-ux-ui-saude/SKILL.md` | Health app UX/UI |
| Integration Specialist | `skills/enabling/integration-specialist/SKILL.md` | System integration |
| Project Orchestrator | `skills/enabling/project-orchestrator/SKILL.md` | Project coordination |
| Research Synthesis | `skills/enabling/research-synthesis/SKILL.md` | Research analysis |
| UX/UI Designer | `skills/enabling/ux-ui-designer/SKILL.md` | Design standards |

### Foundation Skills

| Skill | Path | Description |
|-------|------|-------------|
| Data Engineer | `skills/foundation/data-engineer/SKILL.md` | Data engineering |
| DBA | `skills/foundation/dba/SKILL.md` | Database administration |
| Product Manager | `skills/foundation/product-manager/SKILL.md` | Product management |
| QA Engineer | `skills/foundation/qa-engineer/SKILL.md` | Quality assurance |
| Software Architect | `skills/foundation/software-architect/SKILL.md` | Architecture |
| Software Engineer | `skills/foundation/software-engineer/SKILL.md` | Development standards |

### Process Skills

| Skill | Path | Description |
|-------|------|-------------|
| Business Analyst | `skills/process/business-analyst/SKILL.md` | Business analysis |
| Fiscal Consultant | `skills/process/fiscal-consultant/SKILL.md` | Fiscal compliance |
| GDPR/Privacy | `skills/process/gdpr-privacy-specialist/SKILL.md` | Privacy compliance |
| Git Workflow | `skills/process/git-workflow-coordinator/SKILL.md` | Git/GitHub workflow |
| Skill Coordinator | `skills/process/skill-coordinator/SKILL.md` | AI skill management |

## Claude Code Resources

### Agents

| Agent | File | Use When |
|-------|------|----------|
| Orchestrator | `.claude/agents/orchestrator.md` | Any complex or unclear request |
| Code Reviewer | `.claude/agents/code-reviewer.md` | Code quality review |
| Security Auditor | `.claude/agents/security-auditor.md` | Security assessment |

### Commands

| Command | File | Usage |
|---------|------|-------|
| `/project:review` | `.claude/commands/review.md` | Pre-PR review |
| `/project:fix-issue` | `.claude/commands/fix-issue.md` | Fix GitHub issues |
| `/project:deploy` | `.claude/commands/deploy.md` | Deployment workflow |

### Rules

| Rule | File | Covers |
|------|------|--------|
| Code Style | `.claude/rules/code-style.md` | Naming, formatting, TypeScript |
| Testing | `.claude/rules/testing.md` | Test structure, coverage |
| API Conventions | `.claude/rules/api-conventions.md` | REST standards, versioning |

### Skills (Auto-invoked)

| Skill | File | Triggers On |
|-------|------|-------------|
| Orchestrator | `.claude/skills/orchestrator/SKILL.md` | Generic/complex requests |
| Security Review | `.claude/skills/security-review/SKILL.md` | Security-sensitive code |
| Deploy | `.claude/skills/deploy/SKILL.md` | Deployment context |

## Usage Patterns

### For Copilot

Reference skills in your project's `copilot-instructions.md`:

```markdown
## Domain Context
Include: ~/repos/.github/skills/foundation/software-engineer/SKILL.md
Include: ~/repos/.github/skills/domain/mysql-specialist/SKILL.md
```

### For Claude Code

1. Use the **Orchestrator** as entry point:
   - "Help me review this code" → routes to code-reviewer
   - "Is this secure?" → routes to security-auditor
   - "Deploy this" → runs deploy workflow

2. Use **slash commands** directly:
   - `/project:review` for pre-PR checks
   - `/project:fix-issue` for bug fixes
   - `/project:deploy` for deployment

3. Reference **rules** automatically applied

## Contributing

To add a new skill:

1. Create `skills/<category>/<skill-name>/SKILL.md`
2. Follow the existing SKILL.md format
3. Update this INDEX.md
4. Submit PR following the template
