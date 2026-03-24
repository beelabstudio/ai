# Orchestrator Skill

## Description

Central coordination skill that routes requests to appropriate specialized agents and workflows. Activates when the user makes broad requests that require coordination of multiple resources.

## Activation

This skill is auto-invoked when:
- User asks for "help" or "what can you do"
- Request involves multiple domains (code + security + deploy)
- Complex multi-step tasks without clear single-agent ownership
- "Review everything" or "check all" type requests
- User is unsure which specific agent/command to use

## Behavior

When activated:
1. Load the `orchestrator` agent persona
2. Analyze the request against available resources
3. Delegate to appropriate specialized agents
4. Coordinate workflow execution
5. Return consolidated results

## Coordination Patterns

### Multi-Agent Tasks
- Break work into logical components
- Assign each component to best-fit agent
- Execute in optimal order (dependencies first)
- Aggregate results

### Resource Discovery
- Check available commands in `.claude/commands/`
- Review applicable rules in `.claude/rules/`
- Identify relevant skills in `.claude/skills/`
- Select matching agents in `.claude/agents/`

## Example Triggers

| User Input | Orchestrator Action |
|-----------|---------------------|
| "I need help" | Show available commands and capabilities |
| "Check this PR" | Route to code-reviewer + security-auditor |
| "Fix and deploy" | Coordinate fix-issue → deploy workflow |
| "Is this ready?" | Run full review: code, tests, security |
| "What agents do we have?" | List all available agents and skills |
