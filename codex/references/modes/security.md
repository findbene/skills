# Mode: security — Deep Security Audit

Runs Codex CLI in security-specialist mode. Focuses exclusively on vulnerabilities, secrets exposure, auth bypass, injection vectors, and tenant isolation gaps.

## Execution Steps

### Step 1 — Run Codex

```bash
codex --approval-policy suggest "
You are a senior application security engineer conducting a penetration-test-style code review. Your job is to find every exploitable vulnerability in this codebase. Be aggressive and assume an adversarial attacker perspective.

FOCUS AREAS:

SECRETS & CREDENTIALS:
- Hardcoded API keys, tokens, passwords anywhere in source
- os.environ['KEY'] (crashes in prod, exposes key in error)
- .env files that may be committed
- Secrets appearing in log statements
- SUPABASE_SERVICE_ROLE_KEY in any frontend file under dashboard/

INJECTION:
- SQL injection: string concatenation into queries instead of parameterized queries
- Prompt injection: user input passed directly into LLM prompts without sanitization
- Command injection: user input in subprocess calls
- XSS: unsanitized output in templates or React components

AUTHENTICATION & AUTHORIZATION:
- JWT verification skipped or using alg:none
- tenant_id sourced from request body instead of verified JWT claims
- Missing auth checks on FastAPI routes
- Routes that should require TenantDep but don't
- SUPABASE_SERVICE_ROLE_KEY used client-side

TENANT ISOLATION:
- Supabase tables missing tenant_id column
- Supabase tables missing RLS policies
- Queries that don't filter by tenant_id
- Cross-tenant data leakage vectors

API SECURITY:
- Internal exception details exposed in HTTP 500 responses
- Stack traces in API error responses
- Missing input validation at API boundaries (no Pydantic models)
- Rate limiting absent on expensive endpoints

DEPENDENCY RISKS:
- Direct use of deprecated or known-vulnerable patterns
- Dangerous subprocess usage

For each vulnerability:
1. File path + line number
2. Severity: CRITICAL | HIGH | MEDIUM | LOW
3. Vulnerability class (OWASP category if applicable)
4. Exact exploit scenario
5. Remediation

Be exhaustive. A missed vulnerability is a breach waiting to happen.
"
```

### Step 2 — Report

Produce a structured security report with:
- Critical vulnerabilities (immediate action required)
- High vulnerabilities (fix this session)
- Medium/Low (fix this sprint)
- Tenant isolation audit results
- Secret exposure audit results

## Notes
- Read-only audit. Codex will not make changes.
- For fixes, use `/codex-fix` with the specific issue.

## When NOT to use

- Architecture or general code review without security focus — use `/codex-adversarial-review` or `code-review`
- Performance/optimization audits — use `/codex-optimize`
- Test coverage assessment — use `/codex-test`
- Infrastructure-level security (network, k8s policies) — use `senior-secops` or `cloud-security`
- Already-known issue triage — go straight to `/codex-fix`

## Red Flags (security-specific)

| Thought | Reality |
|---------|---------|
| "Skip the report, just fix the criticals" | Without the structured report, mediums/lows go silent and accumulate |
| "Service-role key in frontend is fine if hidden" | It's not; rotation + move to backend is mandatory |
| "tenant_id from request body, JWT for auth" | Always read tenant_id from verified JWT claims — body is attacker-controlled |
| "RLS later, ship now" | Multi-tenant without RLS = guaranteed cross-tenant leak |

## Output Contract (security-specific)

Done when:
- Codex run completed in read-only mode (no code changes)
- Report includes Critical / High / Medium / Low buckets
- Each finding has: file path + line, severity, vulnerability class (OWASP if applicable), exploit scenario, remediation
- Tenant isolation audit section present
- Secret exposure audit section present
- Next-action triage list (fix this session, fix this sprint)
