---
name: security
description: "Application and infrastructure security hardening expert. Use this skill any time security vulnerabilities need to be found or fixed, code needs to be hardened against attacks, threat modeling is needed, or security audits must be run. Trigger immediately on: \"security audit\", \"harden\", \"vulnerability\", \"OWASP\", \"XSS\", \"SQL injection\", \"CSRF\", \"secrets management\", \"authentication bypass\", \"authorization flaw\", \"input validation\", \"rate limiting\", \"penetration testing\", \"security review\", \"CVE\", \"dependency scan\", \"zero trust\", \"encryption\", \"secure headers\". If someone says \"is this code secure?\" or \"check for vulnerabilities\" this skill MUST trigger."
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
1. What assets are valuable? (user data, API keys, payment data, internal systems)
2. Who are the adversaries? (external attackers, insider threats, automated bots)
3. What are the attack vectors? (web, API, dependencies, social engineering)
4. What's the blast radius of a breach?

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
