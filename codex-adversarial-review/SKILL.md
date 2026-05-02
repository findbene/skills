---
name: codex-adversarial-review
description: "You are acting as an adversarial code auditor. Your job is to find every mistake, violation, and poor decision made by Claude Code in this codebase. Be aggressive, unsparing, and thorough. Assume Claude Code made mistakes. Your goal is to find them all."
---

# /codex:adversarial-review — Adversarial Codebase Audit

You are acting as an adversarial code auditor. Your job is to find every mistake, violation, and poor decision made by Claude Code in this codebase. Be aggressive, unsparing, and thorough. Assume Claude Code made mistakes. Your goal is to find them all.

## What This Skill Does

Runs OpenAI Codex CLI (o3 model) against the entire codebase with an adversarial, critical prompt. Codex operates independently of Claude Code, so it can catch mistakes Claude may have introduced or missed. After Codex completes, synthesize a structured report of all findings.

## Execution Steps

### Step 1 — Orient
Read these files before launching Codex:
- `SESSION_HANDOFF.md` — current state
- `CLAUDE.md` — hard rules Codex should verify against
- `.claude/rules/` — all rule files (00 through 90)

Extract the hard rules and constraints so you can include them in the Codex prompt.

### Step 2 — Run Codex

Change to the project root directory and run:

```bash
codex --approval-policy suggest "
You are an adversarial code reviewer tasked with finding every mistake, bug, violation, and poor decision in this codebase. Be critical, thorough, and aggressive. Assume the previous developer made errors. Your job is to find them all.

HARD RULES TO VERIFY (from project CLAUDE.md):
- ALL LLM calls must go through agents/llm_client.py:call_llm() only. Flag any direct import of anthropic, openai, or litellm anywhere else.
- Stream 1 (media_pipeline) must NEVER route through citadel/execution/harness/runner.py
- Stream 2+3 agents must NEVER bypass citadel/execution/harness/runner.py (after Phase 3)
- Every Supabase table must have tenant_id column + RLS policy + index
- SUPABASE_SERVICE_ROLE_KEY must never appear in dashboard/ frontend code
- Never os.environ['KEY'] — must use os.environ.get('KEY')
- No hardcoded secrets, API keys, or tokens anywhere
- No direct anthropic/openai/litellm imports outside agents/llm_client.py
- No new CrewAI agents
- No print() in production modules (citadel/, core/, agents/, projects/)

AUDIT CHECKLIST — review every file for:

CORRECTNESS:
- Logic errors, off-by-one bugs, wrong conditionals
- Incorrect async/await usage (missing await, blocking calls in async context)
- Race conditions in concurrent code
- Silent error swallowing (bare except, pass in except blocks)
- Functions that claim to do X but actually do Y
- Dead code paths that will never execute
- Variables that shadow outer scope unintentionally

SECURITY:
- Hardcoded secrets, tokens, API keys
- os.environ['KEY'] instead of os.environ.get('KEY')
- SQL injection vectors (string concatenation in queries)
- Input validation gaps at API boundaries
- JWT verification skipped or weakened
- SUPABASE_SERVICE_ROLE_KEY in frontend code
- Secrets in log statements
- Missing authentication checks on protected routes

ARCHITECTURE VIOLATIONS:
- Direct LLM library imports outside agents/llm_client.py
- Stream 1 pipeline routed through citadel harness
- New CrewAI agents added
- print() in production modules
- Raw dicts crossing module boundaries instead of Pydantic models
- tenant_id taken from request body instead of verified JWT claims
- Missing response_model on FastAPI endpoints
- Missing tenant_id/RLS on Supabase tables

CODE QUALITY:
- Functions longer than 40 lines without clear justification
- Duplicate logic that should be shared
- Missing type annotations on function signatures
- Any type usage without explanatory comment
- Circular imports
- Star imports
- Unused imports
- Missing error handling at system boundaries
- Over-engineered abstractions for one-time operations
- Speculative code for hypothetical future requirements

TESTING GAPS:
- Business logic in agents/, projects/, citadel/ with no test coverage
- Mocked Supabase in tests (rule violation)
- Tests with time.sleep()
- Tests with undeterministic randomness (no seed)
- Missing regression tests for known bug areas
- Tests that only test the happy path

LangGraph SPECIFIC:
- Untyped state dicts instead of TypedDict
- Generated media written to wrong directories (agents/, citadel/, src/, tests/, core/)
- Quality gate score thresholds not enforced (publish if score >= 7.0, retry 5.0-6.9, hard fail below 5.0)
- Missing token usage tracking in call_llm() callers

DASHBOARD (Next.js):
- 'use client' added unnecessarily to Server Components
- getSession() used instead of getUser() in server context
- Raw dicts returned instead of typed Pydantic/TS models
- Supabase 1:1 joins normalized inline instead of via helper function
- Missing error.tsx boundaries

For each issue found:
1. State the file path and line number
2. Classify: CRITICAL | HIGH | MEDIUM | LOW
3. Describe the exact problem
4. Show the offending code snippet
5. Suggest the correct fix

Be exhaustive. Do not skip files. Do not be polite. This is an adversarial audit.
"
```

### Step 3 — Synthesize Report

After Codex completes, produce a structured report:

```
## Adversarial Audit Report
**Date:** YYYY-MM-DD
**Model:** o3 (Codex CLI)
**Scope:** Full codebase

### CRITICAL (must fix before next commit)
[list]

### HIGH (fix this session)
[list]

### MEDIUM (fix this sprint)
[list]

### LOW (fix when touching the file)
[list]

### Rule Violations (zero tolerance)
[any CLAUDE.md or rules/ violations found]

### Summary
- Total issues: N
- Critical: N | High: N | Medium: N | Low: N
- Rule violations: N
```

### Step 4 — Prioritize

After the report, recommend the top 5 issues to fix immediately, ranked by risk/impact.

## Notes
- This skill is read-only by default (--approval-policy suggest). Codex will NOT make changes.
- To fix issues found, use `/codex:fix <issue description>` or fix manually.
- Run this after every major Claude Code session.
- If Codex times out on the full codebase, re-run with `--model o4-mini` for speed.
