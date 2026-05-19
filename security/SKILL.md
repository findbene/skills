---
name: security
description: "Application and infrastructure security hardening expert. Triggers: 'use security', 'security', 'security task'."
allowed-tools: Glob, Grep, Read
version: 1.0.0
triggers:
  - security review
  - security audit
  - vulnerability
  - hardening
  - secrets management
  - OWASP
  - injection
  - XSS
  - CSRF
  - authentication security
  - dependency scanning
  - supply chain
  - endpoint security
references:
  - references/auth-and-secrets.md
  - references/database-and-deps.md
  - references/desktop-security.md
  - references/web-security.md
---

# Security Skill

## Purpose
Identify, prevent, and remediate security vulnerabilities across web applications, APIs, databases, infrastructure, and developer endpoints.

## Activation
Load this skill when the user asks about:
- Security reviews or audits of code or architecture
- Hardening against OWASP Top 10 vulnerabilities
- Secrets management and avoiding credential exposure
- Authentication and authorization security patterns
- Dependency vulnerabilities and supply chain security
- Database injection prevention
- Desktop/endpoint hardening for developers
- Rate limiting, CSRF protection, Content Security Policy

## Security Review Methodology

### Step 1 — Threat Model First
Before reviewing code, understand:
1. What assets are valuable? (user data, API keys, payment data, internal systems) → verify: step output matches expected outcome
2. Who are the adversaries? (external attackers, insider threats, automated bots) → verify: step output matches expected outcome
3. What are the attack vectors? (web, API, dependencies, social engineering) → verify: step output matches expected outcome
4. What's the blast radius of a breach? → verify: step output matches expected outcome

### Step 2 — Layer-by-Layer Review
Run through all four reference layers:
- **Auth & Secrets** — credential storage, JWT validation, OAuth misconfigs, secrets in code
- **Web Security** — OWASP Top 10, CSP headers, CSRF, rate limiting, input validation
- **Database & Deps** — SQL injection, parameterized queries, dependency CVEs
- **Desktop Security** — developer endpoint hardening, least privilege, EDR

### Step 3 — Severity Classification
| Severity | Examples | Response Time |
|---|---|---|
| Critical | Exposed API keys, SQLi, RCE | Fix immediately, rotate secrets |
| High | Stored XSS, IDOR, auth bypass | Fix within 24 hours |
| Medium | Missing rate limiting, weak headers | Fix within 1 week |
| Low | Missing Subresource Integrity, verbose errors | Fix in next sprint |

### Step 4 — Verify the Fix
Every security fix must be verified:
- Reproduce the vulnerability before fixing
- Confirm the fix eliminates the attack vector
- Add a regression test that would catch regression
- Document what changed and why

## Key Principles
- **Secrets never in code** — use env vars, Vault, or secrets manager. Period.
- **Defense in depth** — multiple layers of protection; no single point of failure
- **Principle of least privilege** — grant minimum access needed, never more
- **Validate at boundaries** — all user input, all API responses, all file uploads
- **Fail securely** — on error, deny access by default; never expose internals
- **Security is not optional** — never defer security fixes for "later"

## Common Mistakes to Catch
- `os.environ.get("KEY")` without a default raises `AttributeError` — secrets missing at runtime
- Logging `request.body` or `user.password` — secrets in logs
- `SELECT * FROM users WHERE email = f"'{email}'"` — SQL injection
- `eval()`, `exec()`, `subprocess.shell=True` with user input — code injection
- JWT `alg: none` or not verifying signature — auth bypass
- Missing CORS headers or overly permissive `Access-Control-Allow-Origin: *` on authenticated APIs

## When NOT to use

- Task is unrelated to security — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Security needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for security
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving security
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
