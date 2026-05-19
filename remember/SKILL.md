---
name: "remember"
description: "Explicitly save important knowledge to auto-memory with timestamp and context. Use when a discovery is too important to rely on auto-capture. Triggers: 'use remember', 'remember', 'remember task'."
allowed-tools: Bash, Glob, Grep, Read
---

# /si:remember — Save Knowledge Explicitly

Writes an explicit entry to auto-memory when something is important enough that you don't want to rely on Claude noticing it automatically.

## Usage

```
/si:remember <what to remember>
/si:remember "This project's CI requires Node 20 LTS — v22 breaks the build"
/si:remember "The /api/auth endpoint uses a custom JWT library, not passport"
/si:remember "Reza prefers explicit error handling over try-catch-all patterns"
```

## When to Use

| Situation | Example |
|-----------|---------|
| Hard-won debugging insight | "CORS errors on /api/upload are caused by the CDN, not the backend" |
| Project convention not in CLAUDE.md | "We use barrel exports in src/components/" |
| Tool-specific gotcha | "Jest needs `--forceExit` flag or it hangs on DB tests" |
| Architecture decision | "We chose Drizzle over Prisma for type-safe SQL" |
| Preference you want Claude to learn | "Don't add comments explaining obvious code" |

## Workflow

### Step 1: Parse the knowledge

Extract from the user's input:
- **What**: The concrete fact or pattern
- **Why it matters**: Context (if provided)
- **Scope**: Project-specific or global?

### Step 2: Check for duplicates

```bash
MEMORY_DIR="$HOME/.claude/projects/$(pwd | sed 's|/|%2F|g; s|%2F|/|; s|^/||')/memory"
grep -ni "<keywords>" "$MEMORY_DIR/MEMORY.md" 2>/dev/null
```

If a similar entry exists:
- Show it to the user
- Ask: "Update the existing entry or add a new one?"

### Step 3: Write to MEMORY.md

Append to the end of `MEMORY.md`:

```markdown
- {{concise fact or pattern}}
```

Keep entries concise — one line when possible. Auto-memory entries don't need timestamps, IDs, or metadata. They're notes, not database records.

If MEMORY.md is over 180 lines, warn the user:

```
⚠️ MEMORY.md is at {{n}}/200 lines. Consider running /si:review to free space.
```

### Step 4: Suggest promotion

If the knowledge sounds like a rule (imperative, always/never, convention):

```
💡 This sounds like it could be a CLAUDE.md rule rather than a memory entry.
   Rules are enforced with higher priority. Want to /si:promote it instead?
```

### Step 5: Confirm

```
✅ Saved to auto-memory

  "{{entry}}"

  MEMORY.md: {{n}}/200 lines
  Claude will see this at the start of every session in this project.
```

## What NOT to use /si:remember for

- **Temporary context**: Use session memory or just tell Claude in conversation
- **Enforced rules**: Use `/si:promote` to write directly to CLAUDE.md
- **Cross-project knowledge**: Use `~/.claude/CLAUDE.md` for global rules
- **Sensitive data**: Never store credentials, tokens, or secrets in memory files

## Tips

- Be concise — one line beats a paragraph
- Include the concrete command or value, not just the concept
  - ✅ "Build with `pnpm build`, tests with `pnpm test:e2e`"
  - ❌ "The project uses pnpm for building and testing"
- If you're remembering the same thing twice, promote it to CLAUDE.md

## When NOT to use

- Task is unrelated to remember — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Remember needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for remember
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving remember
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
