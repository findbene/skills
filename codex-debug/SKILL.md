---
name: codex-debug
description: "Give Codex a specific error or failure to diagnose and fix. Best for tricky bugs that don't have an obvious cause."
---

# /codex:debug — Debug an Error via Codex

Give Codex a specific error or failure to diagnose and fix. Best for tricky bugs that don't have an obvious cause.

## Usage

```
/codex:debug <error or symptom>
```

Examples:
- `/codex:debug KeyError 'niche' in pipeline.py at line 84`
- `/codex:debug pipeline hangs after script_master node with no error`
- `/codex:debug tests/test_pipeline.py::test_dry_run fails with AttributeError`

## Execution Steps

### Step 1 — Gather evidence
Before running Codex, collect:
- The full error message and stack trace
- The file and line number
- Recent git changes: `git log --oneline -5` + `git diff HEAD~1`
- Relevant test output: `python -m pytest <test> -v --tb=long 2>&1`

### Step 2 — Run Codex

```bash
codex --approval-policy full-auto "
Debug and fix the following error in this codebase:

ERROR:
[PASTE FULL ERROR AND STACK TRACE]

RECENT CHANGES:
[PASTE git diff output]

DEBUGGING APPROACH:
1. REPRODUCE — identify the exact code path that triggers this error
2. ISOLATE — find the root cause (not the symptom)
3. DIAGNOSE — explain WHY this happens
4. FIX — implement the minimal correct fix

RULES:
- Fix the root cause, not the symptom
- Do not add try/except to hide the error — fix what's actually wrong
- Do not change unrelated code
- Do not add workarounds or defensive checks for things that should not happen
- After fixing, verify the fix by reading the changed code

The fix must be minimal — change as few lines as possible while being correct.
"
```

### Step 3 — Validate

```bash
python -m pytest <failing_test> -v --tb=short
python scripts/run_pipeline.py --dry-run  # if pipeline-related
```

Report the root cause and what was changed.
