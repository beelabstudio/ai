# Copilot Instructions for BEELABSTUDIO

> **Global Rule — Language**: All documentation, comments, commit messages, README files, PR descriptions, and any other written artifact must be in English — regardless of the language used in the request.

## Project Context

This is the central repository for BEELABSTUDIO organization standards, containing:
- Shared AI skills for team members
- GitHub templates and workflows
- Claude Code configuration (`.claude/`)

## Available Resources

### AI Skills (for reference and inclusion)

Reference these skills when they match the user's context:

**C-Level Skills** (`skills/c-level/`)
- `ceo`: Strategic trade-offs and executive decision-making
- `cfo`: Financial governance and unit economics
- `ciso`: Security posture, risk governance and compliance
- `coo`: Operational governance and scaling
- `cpo`: Product vision and roadmap prioritisation
- `cto`: Technology strategy and architecture decisions

**Domain Skills** (`skills/domain/`)
- `dbt-specialist`: DBT modeling and data transformation
- `github-copilot-agent-engineer`: Copilot agent development and customization
- `legacy-system-migration-specialist`: Migration planning and execution
- `mysql-specialist`: MySQL optimization and management
- `nutricionista-clinica`: Clinical nutrition domain knowledge

**Enabling Skills** (`skills/enabling/`)
- `devops-engineer`: Infrastructure and deployment
- `documentation-writer`: Technical documentation standards
- `especialista-seo-saude`: SEO for health/wellness
- `especialista-ux-ui-saude`: UX/UI for health applications
- `integration-specialist`: System integration patterns
- `project-orchestrator`: Project coordination
- `research-synthesis`: Research analysis
- `ux-ui-designer`: General UX/UI design

**Foundation Skills** (`skills/foundation/`)
- `data-engineer`: Data engineering practices
- `dba`: Database administration
- `product-manager`: Product management
- `qa-engineer`: Quality assurance
- `software-architect`: Architecture decisions
- `software-engineer`: Development standards

**Process Skills** (`skills/process/`)
- `business-analyst`: Business analysis
- `fiscal-consultant`: Fiscal/compliance matters
- `gdpr-privacy-specialist`: Privacy and GDPR compliance
- `git-workflow-coordinator`: Git and GitHub workflow
- `skill-coordinator`: AI skill management

### Claude Code Integration

This repository includes `.claude/` folder with:
- **Orchestrator Agent**: Central coordinator (`.claude/agents/orchestrator.md`)
- **Code Reviewer**: Quality-focused reviews (`.claude/agents/code-reviewer.md`)
- **Security Auditor**: Security checks (`.claude/agents/security-auditor.md`)
- **Slash Commands**: `/project:review`, `/project:fix-issue`, `/project:deploy`
- **Rules**: Code style, testing, API conventions

When working with Claude Code, the orchestrator manages coordination between these resources.

## Standards

### Code Style
- Files: kebab-case (`my-component.tsx`)
- Components: PascalCase (`MyComponent`)
- Functions: camelCase (`myFunction`)
- Constants: UPPER_SNAKE_CASE (`API_BASE_URL`)

### Git Workflow
- Branch from `main`
- Branch naming: `feature/`, `fix/`, `hotfix/`, `chore/`
- Conventional Commits: `feat:`, `fix:`, `chore:`, `docs:`
- Squash merge via PR (no direct push to main)

### Commit Message Format
```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

Types: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `perf`, `ci`

## Testing Requirements

- Minimum 80% code coverage
- 100% coverage for critical paths
- All public APIs must have tests
- Unit, integration, and minimal E2E tests

## Response Guidelines

When assisting users in this repository:
1. Check relevant skills based on the domain
2. Follow code style rules
3. Ensure test coverage for new code
4. Use conventional commit format
5. Maintain English for all artifacts
6. Reference appropriate `.claude/` resources when applicable
