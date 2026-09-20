---
name: codebase-onboarding
description: >
  Analyze an unfamiliar codebase and generate a structured onboarding guide with an
  architecture map, key entry points, conventions, and a starter AGENTS.md/CLAUDE.md. Use when
  joining a new project or setting up an AI assistant in a repo for the first time.
source: https://github.com/affaan-m/ECC
---

# Codebase Onboarding

Systematically analyze an unfamiliar codebase and produce a structured onboarding guide.

## When to Use

- First time opening a project with an AI assistant
- Joining a new team or repository
- "Help me understand this codebase"
- Generating an `AGENTS.md` / `CLAUDE.md` for a project
- "Onboard me" or "walk me through this repo"

## How It Works

### Phase 1: Reconnaissance

Gather raw signals without reading every file. Run these checks in parallel:

1. **Package manifest detection** — `package.json`, `go.mod`, `Cargo.toml`, `pyproject.toml`, `pom.xml`, `build.gradle`, `Gemfile`, `composer.json`, `pubspec.yaml`
2. **Framework fingerprinting** — `next.config.*`, `nuxt.config.*`, `vite.config.*`, Django settings, FastAPI main, Rails config
3. **Entry point identification** — `main.*`, `index.*`, `app.*`, `server.*`, `cmd/`, `src/main/`
4. **Directory structure snapshot** — top 2 levels, ignoring `node_modules`, `vendor`, `.git`, `dist`, `build`, `__pycache__`
5. **Config and tooling detection** — `.eslintrc*`, `tsconfig.json`, `Makefile`, `Dockerfile`, `.github/workflows/`, `.env.example`
6. **Test structure detection** — `tests/`, `test/`, `__tests__/`, `*.spec.ts`, `*.test.js`

### Phase 2: Architecture Mapping

**Tech stack** — language(s), framework(s), database(s), build tools, CI/CD.

**Architecture pattern** — monolith, monorepo, microservices, serverless; frontend/backend split; API style (REST, GraphQL, gRPC, tRPC).

**Key directories** — map top-level directories to purpose.

**Data flow** — trace one request from entry to response: router → middleware/schema → service/model → repository/DB.

### Phase 3: Convention Detection

- **Naming** — file naming (kebab-case, camelCase, PascalCase, snake_case), component/class naming, test file naming
- **Code patterns** — error handling style, dependency injection vs direct imports, async patterns
- **Git conventions** — branch naming, commit style, PR workflow (squash/merge/rebase)

If the repo has no commits or only shallow history (`git clone --depth 1`), note "Git history unavailable or too shallow to detect conventions".

### Phase 4: Generate Onboarding Artifacts

#### Output 1: Onboarding Guide

```markdown
# Onboarding Guide: [Project Name]

## Overview
[2-3 sentences: what this project does and who it serves]

## Tech Stack
| Layer | Technology | Version |
|-------|-----------|---------|
| Language | TypeScript | 5.x |

## Architecture
[How components connect]

## Key Entry Points
- **API routes**: `src/app/api/`
- **Database**: `prisma/schema.prisma`

## Directory Map
[Top-level directory → purpose]

## Request Lifecycle
[Trace one request from entry to response]

## Conventions
- [file naming] [error handling] [testing] [git workflow]

## Common Tasks
- **Run dev server**: `npm run dev`
- **Run tests**: `npm test`

## Where to Look
| I want to... | Look at... |
|--------------|-----------|
| Add an endpoint | `src/app/api/` |
| Add a table | `prisma/schema.prisma` |
```

#### Output 2: Starter AGENTS.md / CLAUDE.md

Generate or enhance a project-specific instruction file based on detected conventions. If one already exists, read it first and enhance it — preserve existing instructions and call out what was added or changed.

## Best Practices

1. **Don't read everything** — use Glob and Grep, not Read on every file.
2. **Verify, don't guess** — if config says one framework but code uses another, trust the code.
3. **Respect existing instruction files** — enhance, don't replace. Call out what's new vs existing.
4. **Stay concise** — the guide should be scannable in 2 minutes.
5. **Flag unknowns** — "could not determine test runner" beats a wrong answer.

## Anti-Patterns to Avoid

- Generating an instruction file longer than ~100 lines
- Listing every dependency — highlight only the ones that shape how code is written
- Describing obvious directory names (`src/` needs no explanation)
- Copying the README — the guide adds structural insight the README lacks
