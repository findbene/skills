---
name: codex-test
description: "Delegates test writing to Codex CLI. Use when you need comprehensive tests for a module, feature, or after fixing a bug."
---

# /codex:test — Generate or Improve Tests via Codex

Delegates test writing to Codex CLI. Use when you need comprehensive tests for a module, feature, or after fixing a bug.

## Usage

```
/codex:test <target>
```

Examples:
- `/codex:test agents/script_master.py`
- `/codex:test projects/media_pipeline/pipeline.py`
- `/codex:test all agents/ that lack coverage`

## Execution Steps

### Step 1 — Assess Coverage
Run before Codex to understand current state:

```bash
python -m pytest tests/ --co -q 2>/dev/null | grep -i "<target>" | head -30
```

### Step 2 — Run Codex

```bash
codex --approval-policy full-auto "
Write comprehensive pytest tests for [TARGET] in this codebase.

PROJECT-SPECIFIC RULES (non-negotiable):
- NEVER mock the Supabase client. Use test schema with SUPABASE_TEST_SCHEMA=test
- Every test that writes to Supabase must clean up after itself using fixtures with teardown
- No time.sleep() — use event-driven waits or mock time
- Seed random generators: random.seed(42)
- Tag external network tests with @pytest.mark.integration
- Name regression tests: test_regression_<issue_description>
- Tests go in tests/ directory, matching the module structure

TEST REQUIREMENTS:
1. Happy path test for every public function
2. Edge cases: empty inputs, None, boundary values
3. Error cases: invalid inputs, missing dependencies
4. Integration tests for anything touching the pipeline or Supabase (mark with @pytest.mark.integration)
5. For every bug-prone area, write a regression test

WHAT TO TEST:
- Return values are correct
- Side effects happen (files written, DB rows created)
- Errors are raised with correct type and message
- State transitions are correct (LangGraph state dicts)

DO NOT:
- Write tests that only test Python builtins
- Write tests that mock business logic away entirely
- Test implementation details — test behavior and contracts
- Write duplicate tests that cover the same branch

After writing tests, verify they are syntactically valid Python.
"
```

### Step 3 — Validate

```bash
python -m pytest tests/ -v -k "<new test names>" --tb=short
```

Report pass/fail and any fixes needed.
