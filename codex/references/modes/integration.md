# Mode: integration — Delegation Patterns + Mode Selection

Delegation patterns for Codex-assisted coding. Covers when to delegate, which mode to use, and multi-turn session handling.

## Pre-Check

Before Codex operations:
1. Check for `.codex/config.toml` — if missing, ask user before bootstrapping.
2. Bootstrap using `references/modes/ecosystem.md` guidance after confirmation.

## Default Configuration

```toml
model = "gpt-5.2-codex"
model_reasoning_effort = "high"
sandbox = "workspace-write"
approval_policy = "on-request"
```

## When to Delegate

Delegate when tasks are multi-file, long-running, architecture-heavy, or require deep analysis.

## Interaction Modes

### MCP Mode
Use `mcp__codex-native__codex` and `mcp__codex-native__codex-reply` for interactive multi-turn work.

### Headless Mode
Use `codex exec` for one-off scripted tasks.

### Background Mode
Use background execution for long tasks and summarize results after completion.

## Session Handling

- Preserve `threadId` for follow-up calls.
- Session bookkeeping in `scripts/codex-interactive.py` is process-local and in-memory.

## Model Selection

| Complexity | Model | Reasoning |
|-----------|-------|-----------|
| Simple | `gpt-5.2-codex` | medium |
| Medium | `gpt-5.2-codex` | high |
| Complex | `gpt-5.2-codex` | xhigh |
| Critical/security | `gpt-5.1-codex-max` | xhigh |

## Best Practices

1. Keep prompts specific.
2. Validate outputs before applying.
3. Ask before enabling MCP servers or writing config files.
4. Enable only MCPs needed for the current task.
