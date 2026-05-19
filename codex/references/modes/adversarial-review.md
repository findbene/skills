# Mode: adversarial-review — Full Adversarial Codebase Audit

Runs OpenAI Codex CLI (o3 model) against the entire codebase with an adversarial, critical prompt. Codex operates independently of Claude Code, so it can catch mistakes Claude may have introduced or missed.

## Execution Steps

### Step 1 — Orient
Read these files before launching Codex:
- `SESSION_HANDOFF.md` — current state
- `CLAUDE.md` — hard rules Codex should verify against
- `.claude/rules/` — all rule files

Extract the hard rules and constraints to include in the Codex prompt.

### Step 2 — Run Codex

```bash
codex --approval-policy suggest "
You are an adversarial code reviewer tasked with finding every mistake, bug, violation, and poor decision in this codebase. Be critical, thorough, and aggressive. Assume the previous developer made errors. Your job is to find them all.

HARD RULES TO VERIFY (from project CLAUDE.md):
- ALL LLM calls must go through agents/llm_client.py:call_llm() only
- Stream 1 (media_pipeline) must NEVER route through citadel/execution/harness/runner.py
- Every Supabase table must have tenant_id column + RLS policy + index
- SUPABASE_SERVICE_ROLE_KEY must never appear in dashboard/ frontend code
- Never os.environ['KEY'] — must use os.environ.get('KEY')
- No hardcoded secrets, API keys, or tokens anywhere
- No direct anthropic/openai/litellm imports outside agents/llm_client.py
- No new CrewAI agents
- No print() in production modules (citadel/, core/, agents/, projects/)

AUDIT CHECKLIST:

CORRECTNESS:
- Logic errors, off-by-one bugs, wrong conditionals
- Incorrect async/await usage (missing await, blocking calls in async context)
- Race conditions in concurrent code
- Silent error swallowing (bare except, pass in except blocks)
- Dead code paths that will never execute

SECURITY:
- Hardcoded secrets, tokens, API keys
- SQL injection vectors
- Input validation gaps at API boundaries
- JWT verification skipped or weakened
- SUPABASE_SERVICE_ROLE_KEY in frontend code

ARCHITECTURE VIOLATIONS:
- Direct LLM library imports outside agents/llm_client.py
- Stream 1 pipeline routed through citadel harness
- New CrewAI agents added
- print() in production modules
- Raw dicts crossing module boundaries instead of Pydantic models
- Missing tenant_id/RLS on Supabase tables

CODE QUALITY:
- Functions longer than 40 lines without justification
- Duplicate logic that should be shared
- Missing type annotations on function signatures
- Circular imports, star imports, unused imports

TESTING GAPS:
- Business logic with no test coverage
- Mocked Supabase in tests (rule violation)
- Tests with time.sleep()
- Missing regression tests for known bug areas

DASHBOARD (Next.js):
- 'use client' added unnecessarily to Server Components
- getSession() used instead of getUser() in server context
- SUPABASE_SERVICE_ROLE_KEY in any dashboard/ file

For each issue found:
1. File path and line number
2. Classify: CRITICAL | HIGH | MEDIUM | LOW
3. Describe the exact problem
4. Show the offending code snippet
5. Suggest the correct fix

Be exhaustive. Do not skip files. Do not be polite. This is an adversarial audit.
"
```

### Step 3 — Synthesize Report

```
## Adversarial Audit Report
**Date:** YYYY-MM-DD
**Model:** o3 (Codex CLI)
**Scope:** Full codebase

### CRITICAL (must fix before next commit)
### HIGH (fix this session)
### MEDIUM (fix this sprint)
### LOW (fix when touching the file)
### Rule Violations (zero tolerance)
### Summary: Total N | Critical N | High N | Medium N | Low N | Rule violations N
```

### Step 4 — Prioritize

After the report, recommend the top 5 issues to fix immediately, ranked by risk/impact.

## Notes
- Read-only by default (`--approval-policy suggest`). Codex will NOT make changes.
- To fix issues found, use `/codex-fix <issue description>` or fix manually.
- Run after every major Claude Code session.
- If Codex times out on the full codebase, re-run with `--model o4-mini` for speed.
