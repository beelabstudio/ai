---
name: security-scan
description: >
  Scan a Claude Code configuration (.claude/ directory) for security vulnerabilities,
  misconfigurations, and injection risks using AgentShield. Checks CLAUDE.md, settings.json,
  MCP servers, hooks, and agent definitions. Use when auditing a .claude/ directory — CLAUDE.md,
  settings.json, MCP servers, hooks, or agent definitions — before committing config changes or
  onboarding to a repo with existing agent configs.
source: https://github.com/affaan-m/agentshield
---

# Security Scan

Audit your Claude Code configuration for security issues using [AgentShield](https://github.com/affaan-m/agentshield).

## Description

AgentShield is a security scanner and supply-chain defense layer for AI-agent configurations. It scans `.claude/` (and other harness) surfaces for hardcoded secrets, overly permissive permissions, prompt-injection vectors, risky MCP servers, and command-injection hooks.

## When to Use

- Setting up a new Claude Code project
- After modifying `.claude/settings.json`, `AGENTS.md`/`CLAUDE.md`, or MCP configs
- Before committing configuration changes
- When onboarding to a repository with existing Claude Code configs
- Periodic security-hygiene checks

## What It Scans

| File | Checks |
|------|--------|
| `CLAUDE.md` / `AGENTS.md` | Hardcoded secrets, auto-run instructions, prompt-injection patterns |
| `settings.json` | Overly permissive allow lists, missing deny lists, dangerous bypass flags |
| `mcp.json` | Risky MCP servers, hardcoded env secrets, `npx` supply-chain risks |
| `hooks/` | Command injection via interpolation, data exfiltration, silent error suppression |
| `agents/*.md` | Unrestricted tool access, prompt-injection surface, missing model specs |

## Prerequisites

```bash
# Check if installed
npx ecc-agentshield --version

# Install globally (recommended)
npm install -g ecc-agentshield

# Or run directly via npx (no install needed)
npx ecc-agentshield scan .
```

## Usage

### Basic Scan

```bash
npx ecc-agentshield scan                # scan current project's .claude/
npx ecc-agentshield scan --path /path/to/.claude
npx ecc-agentshield scan --min-severity medium
```

### Output Formats

```bash
npx ecc-agentshield scan                          # terminal — colored report with grade
npx ecc-agentshield scan --format json            # JSON for CI/CD
npx ecc-agentshield scan --format markdown        # for documentation
npx ecc-agentshield scan --format html > security-report.html
```

### Auto-Fix

```bash
npx ecc-agentshield scan --fix
```

Applies only fixes marked auto-fixable: replaces hardcoded secrets with environment-variable references and tightens wildcard permissions. Never modifies manual-only suggestions.

### Initialize Secure Config

```bash
npx ecc-agentshield init
```

Scaffolds a secure `.claude/` from scratch: scoped permissions and a deny list in `settings.json`, a `CLAUDE.md` with security best practices, and an `mcp.json` placeholder.

### GitHub Action

```yaml
- uses: affaan-m/agentshield@v1
  with:
    path: '.'
    min-severity: 'medium'
    fail-on-findings: true
```

## Severity Levels

| Grade | Score | Meaning |
|-------|-------|---------|
| A | 90-100 | Secure configuration |
| B | 75-89 | Minor issues |
| C | 60-74 | Needs attention |
| D | 40-59 | Significant risks |
| F | 0-39 | Critical vulnerabilities |

## Interpreting Results

### Critical (fix immediately)
- Hardcoded API keys or tokens in config files
- `Bash(*)` in the allow list (unrestricted shell access)
- Command injection in hooks via `${file}` interpolation
- Shell-running MCP servers

### High (fix before production)
- Auto-run instructions in `CLAUDE.md` (prompt-injection vector)
- Missing deny lists in permissions
- Agents with unnecessary shell access

### Medium (recommended)
- Silent error suppression in hooks (`2>/dev/null`, `|| true`)
- Missing `PreToolUse` security hooks
- `npx -y` auto-install in MCP server configs

### Info (awareness)
- Missing descriptions on MCP servers
- Prohibitive instructions correctly flagged as good practice

## Links

- GitHub: [github.com/affaan-m/agentshield](https://github.com/affaan-m/agentshield)
- npm: [npmjs.com/package/ecc-agentshield](https://www.npmjs.com/package/ecc-agentshield)
