# Memory Operations Reference

## File Locations

### Global Memory (all projects)
```
C:\Users\findb\.claude\memory\
  MEMORY.md          ← index (always in context)
  user_*.md          ← identity, role, background
  feedback_*.md      ← workflow corrections
```

### Project Memory (this project)
```
C:\Users\findb\.claude\projects\C--AI-Agency\memory\
  MEMORY.md          ← project memory index
  [type_topic.md]    ← individual memory files
```

### Session Recovery
```
C:\AI_Agency\.agentic\
  progress.md        ← current state (read every session)
  plan.md            ← implementation roadmap
```

## Memory File Format
```markdown
---
name: user_biniyam_background
description: Biniyam's technical background and learning style
type: user
---

[Memory content]

**Why:** [Reason this matters]
**How to apply:** [When to use this in responses]
```

## Write a New Memory

1. Determine type: `user` | `feedback` | `project` | `reference`
2. Check MEMORY.md — does a relevant file already exist?
3. If yes: update that file (no duplicates)
4. If no: write new file
5. Add/update pointer in MEMORY.md index

```
# MEMORY.md entry format:
- [filename]: one-line description
```

## Update Existing Memory

```
1. Read MEMORY.md index
2. Find relevant existing file
3. Read that file
4. Edit it (not create new)
5. Note date of update
```

## Session Start Sequence

```python
# Order of operations at session start:
files_to_read = [
    "C:\\Users\\findb\\.claude\\projects\\C--AI-Agency\\memory\\MEMORY.md",
    "C:\\AI_Agency\\.agentic\\progress.md",
    "C:\\AI_Agency\\CLAUDE.md",
]
# For each feedback_ file in MEMORY.md: read and apply
# For each user_ file: read and adjust communication style
# For each project_ file: understand current state
```

## What NOT to Store

- Session-specific ephemera (temp paths, debug output)
- Information already in CLAUDE.md or progress.md
- Speculative conclusions from reading a single file
- Actual credentials or passwords
- Negative judgments about the user
