# Memory Management & Context Recovery

## Signs of Context Pressure
- Responses becoming shorter or less detailed
- Claude "forgetting" earlier decisions
- Repeated questions about previously-explained topics
- Compressed message counts in conversation

## Prevention: The 30-Turn Rule
At 30 exchanges, run `/memory` proactively:
```
"I'm at the 30-turn mark — extracting session memory."
→ Run /memory or scripts/extract_session_memory.py
→ Update .agentic/progress.md
→ Update global memory if preferences changed
→ Continue working with preserved context
```

## What to Extract Before Compaction
```markdown
# Session Memory — {date}

## Work Completed
- [task]: [outcome]

## Key Decisions
- [decision]: [rationale] — [alternatives considered]

## Current State
- Currently working on: [file/module]
- Partially complete: [what and what's next]
- Open blockers: [any blockers]

## Context That Would Be Lost
- Specific error messages or debugging context
- Intermediate data structures discovered
- API responses or test results

## Next Steps
1. [specific next action]
2. [following action]
```

## Session Recovery Sequence
```
New session after compaction/restart:
1. Read .agentic/progress.md     → current state
2. Read CLAUDE.md                → project context
3. Read global MEMORY.md         → user preferences
4. Read session memory files     → recent decisions
5. Run git log --oneline -5      → what was committed
6. Produce orientation brief     → "here's where we are"
```

## Checkpoint Writing Pattern
```python
from functools import wraps
from datetime import datetime

def with_checkpoint(task_name: str):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            _write_progress(task_name, "in_progress")
            try:
                result = func(*args, **kwargs)
                _write_progress(task_name, "completed", str(result)[:100])
                return result
            except Exception as e:
                _write_progress(task_name, "failed", str(e))
                raise
        return wrapper
    return decorator

def _write_progress(task: str, status: str, details: str = ""):
    entry = f"\n- [{datetime.now().strftime('%Y-%m-%d %H:%M')}] {task}: {status.upper()}"
    if details:
        entry += f" — {details}"
    with open(".agentic/progress.md", "a") as f:
        f.write(entry)
```

## n8n Daily Memory Backup
Use an n8n schedule trigger (10pm daily) to:
1. Read all `.agentic/*.md` files
2. Upsert to Supabase `session_memories` table with date key
3. Provides cloud backup of session state
