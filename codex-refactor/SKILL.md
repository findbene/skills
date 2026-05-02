---
name: codex-refactor
description: "Delegates a focused refactor to Codex CLI. Use for structural improvements within a module — not new features."
---

# /codex:refactor — Refactor Code via Codex

Delegates a focused refactor to Codex CLI. Use for structural improvements within a module — not new features.

## Usage

```
/codex:refactor <target> [goal]
```

Examples:
- `/codex:refactor agents/script_master.py split large functions`
- `/codex:refactor citadel/execution break out the dispatch logic`
- `/codex:refactor projects/media_pipeline/nodes.py add TypedDict state`

## Execution Steps

### Step 1 — Read the target
Read the target file(s) and identify:
- Functions longer than 40 lines
- Duplicate logic
- Missing type annotations
- Raw dicts that should be TypedDict

### Step 2 — Confirm scope with user
State what will be refactored and what will NOT change. Get confirmation before running Codex with `full-auto`.

### Step 3 — Run Codex

```bash
codex --approval-policy full-auto "
Refactor [TARGET] with the following goal: [GOAL]

RULES (non-negotiable):
- Do NOT change behavior — all existing tests must still pass
- Do NOT add features or handle new edge cases
- Do NOT add comments explaining the refactor
- Do NOT remove type annotations — add them where missing
- Keep functions under 40 lines; extract helpers for anything longer
- Use TypedDict for dict shapes that cross function boundaries
- Use Pydantic v2 models at module API boundaries
- No Any without an explanatory comment
- All LLM calls through agents/llm_client.py:call_llm() — do not change this
- Follow existing import ordering: stdlib → third-party → internal

WHAT TO DO:
1. Split oversized functions into focused helpers
2. Extract duplicate logic into shared functions
3. Add type annotations to all function signatures
4. Replace raw dict with TypedDict where dict shape is fixed
5. Remove dead code paths that can never execute
6. Improve variable names that don't convey intent

After refactoring, verify: python -m py_compile <file>.py
"
```

### Step 4 — Validate

```bash
git diff --stat
python -m pytest tests/ -v --tb=short
```

Report what changed and confirm tests pass.
