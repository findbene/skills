---
name: "promote"
description: "Graduate a proven pattern from auto-memory (MEMORY.md) to CLAUDE.md or .claude/rules/ for permanent enforcement. Triggers: 'use promote', 'promote', 'promote task'."
allowed-tools: Bash, Glob, Grep, Read
---

# /si:promote — Graduate Learnings to Rules

Moves a proven pattern from Claude's auto-memory into the project's rule system, where it becomes an enforced instruction rather than a background note.

## Usage

```
/si:promote <pattern description>                    # Auto-detect best target
/si:promote <pattern> --target claude.md             # Promote to CLAUDE.md
/si:promote <pattern> --target rules/testing.md      # Promote to scoped rule
/si:promote <pattern> --target rules/api.md --paths "src/api/**/*.ts"  # Scoped with paths
```

## Workflow

### Step 1: Understand the pattern

Parse the user's description. If vague, ask one clarifying question:
- "What specific behavior should Claude follow?"
- "Does this apply to all files or specific paths?"

### Step 2: Find the pattern in auto-memory

```bash
# Search MEMORY.md for related entries
MEMORY_DIR="$HOME/.claude/projects/$(pwd | sed 's|/|%2F|g; s|%2F|/|; s|^/||')/memory"
grep -ni "<keywords>" "$MEMORY_DIR/MEMORY.md"
```

Show the matching entries and confirm they're what the user means.

### Step 3: Determine the right target

| Pattern scope | Target | Example |
|---|---|---|
| Applies to entire project | `./CLAUDE.md` | "Use pnpm, not npm" |
| Applies to specific file types | `.claude/rules/<topic>.md` | "API handlers need validation" |
| Applies to all your projects | `~/.claude/CLAUDE.md` | "Prefer explicit error handling" |

If the user didn't specify a target, recommend one based on scope.

### Step 4: Distill into a concise rule

Transform the learning from auto-memory's note format into CLAUDE.md's instruction format:

**Before** (MEMORY.md — descriptive):
> The project uses pnpm workspaces. When I tried npm install it failed. The lock file is pnpm-lock.yaml. Must use pnpm install for dependencies.

**After** (CLAUDE.md — prescriptive):
```markdown
## Build & Dependencies
- Package manager: pnpm (not npm). Use `pnpm install`.
```

**Rules for distillation:**
- One line per rule when possible
- Imperative voice ("Use X", "Always Y", "Never Z")
- Include the command or example, not just the concept
- No backstory — just the instruction

### Step 5: Write to target

**For CLAUDE.md:**
1. Read existing CLAUDE.md → verify: file content matches expected shape
2. Find the appropriate section (or create one) → verify: output exists + parses without error
3. Append the new rule under the right heading → verify: step output matches expected outcome
4. If file would exceed 200 lines, suggest using `.claude/rules/` instead → verify: step output matches expected outcome

**For `.claude/rules/`:**
1. Create the file if it doesn't exist → verify: output exists + parses without error
2. Add YAML frontmatter with `paths` if scoped → verify: dependency resolves + import works
3. Write the rule content → verify: output exists + parses without error

```markdown
---
paths:
  - "src/api/**/*.ts"
  - "tests/api/**/*"
---

# API Development Rules

- All endpoints must validate input with Zod schemas
- Use `ApiError` class for error responses (not raw Error)
- Include OpenAPI JSDoc comments on handler functions
```

### Step 6: Clean up auto-memory

After promoting, remove or mark the original entry in MEMORY.md:

```bash
# Show what will be removed
grep -n "<pattern>" "$MEMORY_DIR/MEMORY.md"
```

Ask the user to confirm removal. Then edit MEMORY.md to remove the promoted entry. This frees space for new learnings.

### Step 7: Confirm

```
✅ Promoted to {{target}}

Rule: "{{distilled rule}}"
Source: MEMORY.md line {{n}} (removed)
MEMORY.md: {{lines}}/200 lines remaining

The pattern is now an enforced instruction. Claude will follow it in all future sessions.
```

## Promotion Decision Guide

### Promote when:
- Pattern appeared 3+ times in auto-memory
- You corrected Claude about it more than once
- It's a project convention that any contributor should know
- It prevents a recurring mistake

### Don't promote when:
- It's a one-time debugging note (leave in auto-memory)
- It's session-specific context (session memory handles this)
- It might change soon (e.g., during a migration)
- It's already covered by existing rules

### CLAUDE.md vs .claude/rules/

| Use CLAUDE.md for | Use .claude/rules/ for |
|---|---|
| Global project rules | File-type-specific patterns |
| Build commands | Testing conventions |
| Architecture decisions | API design rules |
| Team conventions | Framework-specific gotchas |

## Tips

- Keep CLAUDE.md under 200 lines — use rules/ for overflow
- One rule per line is easier to maintain than paragraphs
- Include the concrete command, not just the concept
- Review promoted rules quarterly — remove what's no longer relevant

## When NOT to use

- Task is unrelated to promote — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Promote needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for promote
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving promote
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
