---
name: codex-fix
description: "Delegates a targeted fix to Codex CLI with full-auto approval. Use when you have a clear, specific issue to resolve and want Codex (o3) to implement it independently."
---

# /codex:fix — Fix a Specific Issue via Codex

Delegates a targeted fix to Codex CLI with full-auto approval. Use when you have a clear, specific issue to resolve and want Codex (o3) to implement it independently.

## Usage

```
/codex:fix <description of the issue>
```

Examples:
- `/codex:fix the direct anthropic import in agents/script_master.py line 12`
- `/codex:fix missing tenant_id filter in src/api/jobs.py`
- `/codex:fix os.environ["KEY"] calls throughout the codebase`

## Execution Steps

### Step 1 — Confirm Scope
Before running, tell the user:
- What file(s) will be modified
- What the fix will do
- Confirm before proceeding if the change touches more than 3 files

### Step 2 — Run Codex

```bash
codex --approval-policy full-auto "
Fix the following issue in this codebase:

ISSUE: [INSERT USER'S DESCRIPTION]

CONSTRAINTS:
- Follow existing code style and patterns
- Do not refactor surrounding code — fix only the stated issue
- Do not add comments explaining the fix
- Do not add error handling for cases that cannot happen
- Use os.environ.get('KEY') not os.environ['KEY']
- All LLM calls must go through agents/llm_client.py:call_llm()
- No direct anthropic/openai/litellm imports
- Use Pydantic v2 models at API boundaries
- Type annotations required on all modified function signatures

After making the fix, verify it by checking the modified file for correctness.
"
```

### Step 3 — Validate

After Codex completes:
1. Run `git diff` to review what changed
2. Run `python -m py_compile <modified_file>.py` for syntax check
3. Run `python -m pytest tests/ -v -k "<relevant test>"` if applicable
4. Report what was changed

## Notes
- Uses `full-auto` approval — Codex will write files directly.
- Review the git diff before committing.
- For multi-file fixes, consider `/codex:refactor` instead.
