# Mode: diff — Review Staged Changes Before Commit

Runs Codex against the current git diff before committing. Catches mistakes in the changes you're about to commit.

## Usage

```
/codex-diff
```

Or to review a specific range:
```
/codex-diff HEAD~3..HEAD
```

## Execution Steps

### Step 1 — Get the Diff

```bash
git diff HEAD          # unstaged changes
git diff --cached      # staged changes
git diff HEAD~1        # last commit
```

Show the diff to the user so both Claude and the user can see what's being reviewed.

### Step 2 — Run Codex

```bash
codex --approval-policy suggest "
Review the following git diff critically before it gets committed. Find any problems.

$(git diff --cached 2>/dev/null || git diff HEAD)

CHECK FOR:
1. RULE VIOLATIONS:
   - Direct anthropic/openai/litellm imports outside agents/llm_client.py
   - os.environ['KEY'] instead of os.environ.get('KEY')
   - Hardcoded secrets, tokens, API keys
   - print() added to citadel/, core/, agents/, or projects/
   - New CrewAI agents
   - Stream 1 being routed through citadel harness

2. CORRECTNESS:
   - Logic errors in the changed code
   - Missing await on async calls
   - Variables used before assignment
   - Wrong return types

3. SECURITY:
   - Any secret or credential exposed
   - Input validation removed or weakened
   - Auth check removed

4. BROKEN TESTS:
   - Existing test assertions that will fail with this change
   - Test mocks that were added for Supabase (not allowed)

5. QUALITY:
   - Functions extended past 40 lines
   - Type annotations removed
   - Pydantic models replaced with raw dicts

For each issue:
- File + line number in the diff
- Severity: BLOCKER | WARNING | SUGGESTION
- What's wrong and how to fix it

BLOCKER = do not commit until fixed
WARNING = strong recommendation to fix
SUGGESTION = nice to have
"
```

### Step 3 — Report

List all findings. If any BLOCKERs exist, tell the user explicitly: **Do not commit until these are resolved.**
