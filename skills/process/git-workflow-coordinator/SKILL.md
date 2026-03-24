# Git Workflow Coordinator

## Description

Defines and enforces the version control strategy for all Bee Lab Studio projects. The workflow is based on **GitHub Flow** — simple, continuous, and compatible with Kanban + CI/CD. No one works directly on `main`. Every change goes through a feature branch and a pull request.

This skill is activated when:
- Setting up a new repository
- Creating a new feature, fix, or chore branch
- Opening or reviewing a pull request
- Resolving a merge conflict
- Planning a release or hotfix
- Configuring branch protection rules

---

## Core Principle: Main is Always Deployable

`main` is the production branch. It must always be in a state that can be deployed safely. This is enforced by:
- Branch protection rules (no direct push to `main`)
- Required CI passing before merge
- Required pull request review

---

## Branching Model: GitHub Flow

```
main ──────────────────────────────────────────────────────► (always deployable)
  │
  ├── feature/add-whatsapp-payment-reminder  ──► PR ──► squash merge ──► main
  │
  ├── fix/billing-grace-period-off-by-one    ──► PR ──► squash merge ──► main
  │
  ├── hotfix/jwt-secret-exposure             ──► PR ──► squash merge ──► main (fast-track)
  │
  └── chore/update-prisma-schema             ──► PR ──► squash merge ──► main
```

### Branch naming convention

| Prefix | When to use | Example |
|--------|-------------|---------|
| `feature/` | New functionality | `feature/student-belt-promotion` |
| `fix/` | Bug fix (non-urgent) | `fix/attendance-count-wrong` |
| `hotfix/` | Urgent production fix | `hotfix/stripe-webhook-signature` |
| `chore/` | Tooling, config, deps, docs | `chore/upgrade-prisma-7` |
| `refactor/` | Code restructure, no behaviour change | `refactor/extract-notification-service` |
| `test/` | Adding or fixing tests only | `test/billing-integration-coverage` |

**Rules:**
- All lowercase, words separated by hyphens
- Short and descriptive — no more than 5 words after the prefix
- Include ticket/issue number when it exists: `feature/UMAT-42-qr-checkin`
- Never use: `my-branch`, `test`, `dev`, `wip`, `temp`

### Creating a branch

Always branch from the latest `main`:

```bash
git switch main
git pull origin main
git switch -c feature/your-feature-name
```

---

## Pull Request Process

### Before opening a PR

- [ ] Branch is up to date with `main` (`git rebase main` or `git merge main`)
- [ ] All tests pass locally (`npm test`)
- [ ] TypeScript compiles (`tsc --noEmit`)
- [ ] Linter passes (`npm run lint`)
- [ ] No `console.log` left in production code
- [ ] No `.env` secrets or API keys committed
- [ ] Self-reviewed the diff — read every line as if reviewing someone else's code

### PR title format

Follow Conventional Commits:

```
feat: add WhatsApp payment due reminder
fix: correct belt stripe count validation
chore: upgrade Prisma to 7.5
docs: add ADR-005 mobile strategy
refactor: extract billing dunning logic to service
test: add integration tests for auth endpoints
```

### PR description template

Every PR must include:

```markdown
## What
[One paragraph describing what changed and why]

## How to test
1. [Step 1]
2. [Step 2]
3. Expected: [what should happen]

## Checklist
- [ ] Tests written and passing
- [ ] TypeScript clean
- [ ] Linter clean
- [ ] QA reviewed acceptance criteria (link story)
- [ ] PO validated on staging (if applicable)
- [ ] No secrets committed
- [ ] Migration included if schema changed
```

### Review rules

- **Minimum 1 approval** before merge — no self-merge
- **CI must pass** — no exceptions (lint + type check + unit + integration tests)
- Reviewer focuses on: logic correctness, security, test coverage, readability
- Author responds to every comment — either fix or explain why not
- **Do not merge a PR with unresolved conversations**
- If a requested change is out of scope → open a new ticket, don't expand the PR

### Merge strategy: Squash and Merge

All PRs are squashed into a single commit on `main`. This keeps `main` history clean and readable.

**After merge:**
- Delete the feature branch immediately (GitHub does this automatically if configured)
- The squash commit message should follow Conventional Commits format

---

## Branch Protection Rules (configure on GitHub)

For `main`:

```
✅ Require a pull request before merging
  ✅ Require approvals: 1
  ✅ Dismiss stale pull request approvals when new commits are pushed
✅ Require status checks to pass before merging
  ✅ Require branches to be up to date before merging
  Required checks: lint, typecheck, test-unit, test-integration
✅ Require conversation resolution before merging
✅ Do not allow bypassing the above settings (including admins)
❌ Allow force pushes → DISABLED
❌ Allow deletions → DISABLED
```

---

## Commit Conventions (Conventional Commits)

Every commit must follow this format:

```
<type>(<scope>): <description>

[optional body]

[optional footer: Co-Authored-By, Fixes #123]
```

### Types

| Type | When to use |
|------|-------------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `chore` | Dependency updates, build config, tooling |
| `docs` | Documentation only |
| `refactor` | Code restructure — no behaviour change |
| `test` | Adding or fixing tests |
| `perf` | Performance improvement |
| `ci` | CI/CD pipeline changes |
| `revert` | Reverting a previous commit |

### Scope (optional but encouraged)

Use the module or area of the codebase: `auth`, `billing`, `whatsapp`, `graduation`, `mobile`, `api`, `db`

```
feat(billing): add MB Way recurring payment support
fix(auth): prevent role self-assignment on registration
chore(deps): upgrade Prisma from 6 to 7
test(graduation): add belt promotion eligibility unit tests
```

### Rules
- Description in **imperative mood**: "add" not "added" or "adds"
- No capital letter at start of description
- No period at end
- Keep under 72 characters
- Body explains *why*, not *what*

---

## Hotfix Process

For urgent production bugs (P0 — billing broken, security breach, data loss):

```bash
# 1. Branch from main immediately
git switch main && git pull
git switch -c hotfix/brief-description

# 2. Fix the issue — write a regression test first (TDD)
# 3. Open PR — mark as hotfix in title
# 4. Fast-track review — 1 reviewer, CI must pass
# 5. Squash merge to main
# 6. Deploy immediately
# 7. Open a follow-up ticket for root cause analysis
```

Hotfixes skip the staging queue but **never** skip CI or code review.

---

## Release Tagging

After a group of features is deployed and validated:

```bash
# Semantic versioning: MAJOR.MINOR.PATCH
# MAJOR: breaking change (new billing model, auth overhaul)
# MINOR: new feature (new module, new endpoint)
# PATCH: bug fix or chore

git tag -a v1.2.0 -m "feat: add WhatsApp notification module"
git push origin v1.2.0
```

Tags are created on `main` after deployment, not before. GitHub Releases are created from tags with a changelog generated from squash commit messages.

---

## Common Mistakes to Avoid

| Mistake | Consequence | Prevention |
|---------|------------|------------|
| Working directly on `main` | No review, broken main | Branch protection blocks direct push |
| Long-lived feature branches (>3 days) | Merge conflicts, integration risk | Break work into smaller PRs |
| Merging without CI passing | Broken tests in main | Branch protection requires green CI |
| Committing `.env` or secrets | Security breach | Pre-commit hook + secret scanning |
| Rewriting public history (`rebase -f`, `push --force` on `main`) | Destroys team's work | Branch protection blocks force push |
| PRs with 50+ files changed | Unreviable, risky | Split into multiple focused PRs |
| Vague branch names (`test`, `wip`) | No context | Branch naming conventions above |

---

## Git Hooks (recommended for all projects)

Install with Husky or configure via `.claude/settings.json` hooks:

### Pre-commit (runs before every commit)
```bash
# Prevent secrets from being committed
npx gitleaks protect --staged

# Run linter on staged files
npx lint-staged
```

### Commit-msg (validates commit message format)
```bash
npx commitlint --edit $1
```

### Pre-push (runs before pushing)
```bash
npm run typecheck
npm test -- --passWithNoTests
```

---

## Repository Setup Checklist (for new projects)

- [ ] `main` branch created and set as default
- [ ] Branch protection rules configured (see above)
- [ ] `.gitignore` includes: `node_modules`, `.env`, `dist`, `*.local`
- [ ] `.env.example` committed with all required keys (no values)
- [ ] `CLAUDE.md` with skill references
- [ ] PR description template at `.github/pull_request_template.md`
- [ ] GitHub Actions CI workflow at `.github/workflows/ci.yml`
- [ ] Conventional Commits enforced (commitlint or equivalent)
- [ ] Secret scanning enabled (GitHub → Settings → Security → Secret scanning)
