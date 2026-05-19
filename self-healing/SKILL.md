---
name: self-healing
description: "Self-healing AI agent system patterns for automatic error recovery, retry strategies, circuit breakers, and autonomous failure resolu. Triggers: 'use self-healing', 'self healing', 'self-healing task."
allowed-tools: Glob, Grep, Read
version: 1.0.0
triggers:
  - self-healing
  - self-improvement
  - error loop
  - stuck in a loop
  - auto-fix
  - automatic recovery
  - context compaction
  - extract memory
  - session recovery
  - prevent context loss
  - agent failure pattern
  - repeated errors
references:
  - references/memory-management.md
  - references/pattern-recognition.md
  - references/skill-creation-guide.md
---

# Self-Healing Skill

## Purpose
Build AI agent systems that recover from failures automatically, preserve context before it's lost, recognize recurring error patterns, and improve over time.

## Activation
Load this skill when:
- An agent is stuck in a failure loop (same error repeating)
- Context window approaching limits (> 30 turns)
- Setting up automatic error recovery for agent pipelines
- Building the self-improvement loop (Auditor → Optimizer → Script Master)
- Creating new skills to address recurring problem patterns
- Setting up Claude Code hooks for automatic session management

## Self-Healing Hierarchy

```
Level 1: Retry (same approach)
Level 2: Retry with adjustment (modified parameters)
Level 3: Alternative strategy (different approach)
Level 4: Degraded mode (partial success, flag for human)
Level 5: Escalation (alert Biniyam, stop)
```

Never jump to Level 5 without trying Levels 1–4 first.

## Error Classification

| Error Class | Examples | Strategy |
|---|---|---|
| Transient | Network timeout, API 429, DB connection | Retry with backoff |
| Configuration | Missing env var, wrong key format | Fail fast, alert |
| Logic | Wrong output schema, assertion failure | Retry with adjusted prompt |
| Resource | OOM, disk full, quota exceeded | Reduce batch size, alert |
| External | Third-party API down | Fallback, circuit breaker |
| Data | Corrupted input, unexpected format | Log + skip, continue |

## Self-Improvement Loop (This Project)

```
Agent generates output
    ↓
Auditor Agent scores (1-10)
    ↓
If score < 7: log failure to Supabase (negative example)
    ↓
Optimizer Agent reads failures, analyzes patterns
    ↓
Optimizer generates improved params for Script Master
    ↓
Script Master loads improvements before next run
    ↓
Quality improves each cycle
```

## Automatic Retry Wrapper

```python
import time
import logging
from functools import wraps

def self_healing_retry(max_attempts=3, base_delay=2.0):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            last_error = None
            for attempt in range(1, max_attempts + 1):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    last_error = e
                    error_class = classify_error(e)
                    if error_class == "configuration":
                        raise  # don't retry config errors
                    if attempt < max_attempts:
                        delay = base_delay * (2 ** (attempt - 1))
                        logging.warning(f"Attempt {attempt}/{max_attempts} failed: {e}. Retry in {delay:.1f}s")
                        time.sleep(delay)
            raise last_error
        return wrapper
    return decorator

def classify_error(e: Exception) -> str:
    s = str(e).lower()
    if any(k in s for k in ["429", "rate limit", "too many"]):  return "transient"
    if any(k in s for k in ["timeout", "connection", "network"]): return "transient"
    if any(k in s for k in ["api key", "unauthorized", "credentials"]): return "configuration"
    if any(k in s for k in ["memory", "disk", "quota"]): return "resource"
    return "unknown"
```

## The 30-Turn Rule

At 30 back-and-forth exchanges, proactively extract memory:
```
Turn 28-30: "I'm at the 30-turn mark — extracting session memory."
→ Run /memory
→ Update .agentic/progress.md
→ Update global memory if needed
→ Continue work with preserved context
```

## Claude Code Hooks

```json
// ~/.claude/settings.json
{
  "hooks": {
    "PreCompact": [{
      "type": "command",
      "command": "python C:\\AI_Agency\\scripts\\extract_session_memory.py"
    }],
    "Stop": [{
      "type": "command",
      "command": "python C:\\AI_Agency\\scripts\\session_end.py"
    }]
  }
}
```

## Triggers

\\\"self-healing\\\", \\\"error recovery\\\", \\\"retry logic\\\", \\\"circuit breaker\\\", \\\"automatic recovery\\\", \\\"agent resilience\\\", \\\"failure handling\\\", \\\"fallback\\\", \\\"health check\\\", \\\"self-repairing\\\", \\\"a...

## When NOT to use

- Task is unrelated to self healing — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Self Healing needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for self healing
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving self healing
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
