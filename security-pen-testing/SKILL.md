---
name: "security-pen-testing"
description: 'Use when the user asks to perform security audits, penetration testing, vulnerability scanning, OWASP Top 10 checks, or o. Triggers: "use security-pen-testing", "security pen testing", "security task.'
allowed-tools: Bash, Glob, Grep, Read
---

# Security Penetration Testing

Hands-on offensive security testing skill for finding vulnerabilities before attackers do. This is NOT compliance checking (see senior-secops) or security policy writing (see senior-security) — this is about systematic vulnerability discovery through authorized testing.

---

## Table of Contents

- [Overview](#overview)
- [OWASP Top 10 Systematic Audit](#owasp-top-10-systematic-audit)
- [Static Analysis](#static-analysis)
- [Dependency Vulnerability Scanning](#dependency-vulnerability-scanning)
- [Secret Scanning](#secret-scanning)
- [API Security Testing](#api-security-testing)
- [Web Vulnerability Testing](#web-vulnerability-testing)
- [Infrastructure Security](#infrastructure-security)
- [Pen Test Report Generation](#pen-test-report-generation)
- [Responsible Disclosure Workflow](#responsible-disclosure-workflow)
- [Workflows](#workflows)
- [Anti-Patterns](#anti-patterns)
- [Cross-References](#cross-references)

---

## Overview

### What This Skill Does

This skill provides the methodology, checklists, and automation for **offensive security testing** — actively probing systems to discover exploitable vulnerabilities. It covers web applications, APIs, infrastructure, and supply chain security.

### Distinction from Other Security Skills

| Skill | Focus | Approach |
|-------|-------|----------|
| **security-pen-testing** (this) | Finding vulnerabilities | Offensive — simulate attacker techniques |
| senior-secops | Security operations | Defensive — monitoring, incident response, SIEM |
| senior-security | Security policy | Governance — policies, frameworks, risk registers |
| skill-security-auditor | CI/CD gates | Automated — pre-merge security checks |

### Prerequisites

All testing described here assumes **written authorization** from the system owner. Unauthorized testing is illegal under the CFAA and equivalent laws worldwide. Always obtain a signed scope-of-work or rules-of-engagement document before starting.

---

## OWASP Top 10 Systematic Audit

Use the vulnerability scanner tool for automated checklist generation:

```bash
# Generate OWASP checklist for a web application
python scripts/vulnerability_scanner.py --target web --scope full

# Quick API-focused scan
python scripts/vulnerability_scanner.py --target api --scope quick --json
```

### Quick Reference

| # | Category | Key Tests |
|---|----------|-----------|
| A01 | Broken Access Control | IDOR, vertical escalation, CORS, JWT claim manipulation, forced browsing |
| A02 | Cryptographic Failures | TLS version, password hashing, hardcoded keys, weak PRNG |
| A03 | Injection | SQLi, NoSQLi, command injection, template injection, XSS |
| A04 | Insecure Design | Rate limiting, business logic abuse, multi-step flow bypass |
| A05 | Security Misconfiguration | Default credentials, debug mode, security headers, directory listing |
| A06 | Vulnerable Components | Dependency audit (npm/pip/go), EOL checks, known CVEs |
| A07 | Auth Failures | Brute force, session cookie flags, session invalidation, MFA bypass |
| A08 | Integrity Failures | Unsafe deserialization, SRI checks, CI/CD pipeline integrity |
| A09 | Logging Failures | Auth event logging, sensitive data in logs, alerting thresholds |
| A10 | SSRF | Internal IP access, cloud metadata endpoints, DNS rebinding |

```bash
# Audit dependencies
python scripts/dependency_auditor.py --file package.json --severity high
python scripts/dependency_auditor.py --file requirements.txt --json
```

See [owasp_top_10_checklist.md](references/owasp_top_10_checklist.md) for detailed test procedures, code patterns to detect, remediation steps, and CVSS scoring guidance for each category.

---

## Static Analysis

**Recommended tools:** CodeQL (custom queries for project-specific patterns), Semgrep (rule-based scanning with auto-fix), ESLint security plugins (`eslint-plugin-security`, `eslint-plugin-no-unsanitized`).

Key patterns to detect: SQL injection via string concatenation, hardcoded JWT secrets, unsafe YAML/pickle deserialization, missing security middleware (e.g., Express without Helmet).

See [attack_patterns.md](references/attack_patterns.md) for code patterns and detection payloads across injection types.

---

## Dependency Vulnerability Scanning

**Ecosystem commands:** `npm audit`, `pip audit`, `govulncheck ./...`, `bundle audit check`

**CVE Triage Workflow:**
1. **Collect** — Run ecosystem audit tools, aggregate findings → verify: command exit code 0
2. **Deduplicate** — Group by CVE ID across direct and transitive deps → verify: step output matches expected outcome
3. **Prioritize** — Critical + exploitable + reachable = fix immediately → verify: diff matches intended change
4. **Remediate** — Upgrade, patch, or mitigate with compensating controls → verify: diff matches intended change
5. **Verify** — Rerun audit to confirm fix, update lock files

```bash
python scripts/dependency_auditor.py --file package.json --severity critical --json
```

---

## Secret Scanning

**Tools:** TruffleHog (git history + filesystem), Gitleaks (regex-based with custom rules).

```bash
# Scan git history for verified secrets
trufflehog git file://. --only-verified --json

# Scan filesystem
trufflehog filesystem . --json
```

**Integration points:** Pre-commit hooks (gitleaks, trufflehog), CI/CD gates (GitHub Actions with `trufflesecurity/trufflehog@main`). Configure `.gitleaks.toml` for custom rules (AWS keys, API keys, private key headers) and allowlists for test fixtures.

---

## API Security Testing

### Authentication Bypass

- **JWT manipulation:** Change `alg` to `none`, RS256-to-HS256 confusion, claim modification (`role: "admin"`, `exp: 9999999999`)
- **Session fixation:** Check if session ID changes after authentication

### Authorization Flaws

- **IDOR/BOLA:** Change resource IDs in every endpoint — test read, update, delete across users
- **BFLA:** Regular user tries admin endpoints (expect 403)
- **Mass assignment:** Add privileged fields (`role`, `is_admin`) to update requests

### Rate Limiting & GraphQL

- **Rate limiting:** Rapid-fire requests to auth endpoints; expect 429 after threshold
- **GraphQL:** Test introspection (should be disabled in prod), query depth attacks, batch mutations bypassing rate limits

See [attack_patterns.md](references/attack_patterns.md) for complete JWT manipulation payloads, IDOR testing methodology, BFLA endpoint lists, GraphQL introspection/depth/batch attack patterns, and rate limiting bypass techniques.

---

## Web Vulnerability Testing

| Vulnerability | Key Tests |
|--------------|-----------|
| **XSS** | Reflected (script/img/svg payloads), Stored (persistent fields), DOM-based (innerHTML + location.hash) |
| **CSRF** | Replay without token (expect 403), cross-session token replay, check SameSite cookie attribute |
| **SQL Injection** | Error-based (`' OR 1=1--`), union-based enumeration, time-based blind (`SLEEP(5)`), boolean-based blind |
| **SSRF** | Internal IPs, cloud metadata endpoints (AWS/GCP/Azure), IPv6/hex/decimal encoding bypasses |
| **Path Traversal** | `../../../etc/passwd`, URL encoding, double encoding bypasses |

See [attack_patterns.md](references/attack_patterns.md) for complete test payloads (XSS filter bypasses, context-specific XSS, SQL injection per database engine, SSRF bypass techniques, and DOM-based XSS source/sink pairs).

---

## Infrastructure Security

**Key checks:**
- **Cloud storage:** S3 bucket public access (`aws s3 ls s3://bucket --no-sign-request`), bucket policies, ACLs
- **HTTP security headers:** HSTS, CSP (no `unsafe-inline`/`unsafe-eval`), X-Content-Type-Options, X-Frame-Options, Referrer-Policy
- **TLS configuration:** `nmap --script ssl-enum-ciphers -p 443 target.com` or `testssl.sh` — reject TLS 1.0/1.1, RC4, 3DES, export-grade ciphers
- **Port scanning:** `nmap -sV target.com` — flag dangerous open ports (FTP/21, Telnet/23, Redis/6379, MongoDB/27017)

---

## Pen Test Report Generation

Generate professional reports from structured findings:

```bash
# Generate markdown report from findings JSON
python scripts/pentest_report_generator.py --findings findings.json --format md --output report.md

# Generate JSON report
python scripts/pentest_report_generator.py --findings findings.json --format json --output report.json
```

### Findings JSON Format

```json
[
  {
    "title": "SQL Injection in Login Endpoint",
    "severity": "critical",
    "cvss_score": 9.8,
    "cvss_vector": "CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H",
    "category": "A03:2021 - Injection",
    "description": "The /api/login endpoint is vulnerable to SQL injection via the email parameter.",
    "evidence": "Request: POST /api/login {\"email\": \"' OR 1=1--\", \"password\": \"x\"}\nResponse: 200 OK with admin session token",
    "impact": "Full database access, authentication bypass, potential remote code execution",
    "remediation": "Use parameterized queries. Replace string concatenation with prepared statements.",
    "references": ["https://cwe.mitre.org/data/definitions/89.html"]
  }
]
```

### Report Structure

1. **Executive Summary**: Business impact, overall risk level, top 3 findings → verify: step output matches expected outcome
2. **Scope**: What was tested, what was excluded, testing dates → verify: all checks pass
3. **Methodology**: Tools used, testing approach (black/gray/white box) → verify: all checks pass
4. **Findings Table**: Sorted by severity with CVSS scores → verify: step output matches expected outcome
5. **Detailed Findings**: Each with description, evidence, impact, remediation → verify: step output matches expected outcome
6. **Remediation Priority Matrix**: Effort vs. impact for each fix → verify: diff matches intended change
7. **Appendix**: Raw tool output, full payload lists → verify: file content matches expected shape

---

## Responsible Disclosure Workflow

Responsible disclosure is **mandatory** for any vulnerability found during authorized testing. Standard timeline: report on day 1, follow up at day 7, status update at day 30, public disclosure at day 90.

**Key principles:** Never exploit beyond proof of concept, encrypt all communications, do not access real user data, document everything with timestamps.

See [responsible_disclosure.md](references/responsible_disclosure.md) for full disclosure timelines (standard 90-day, accelerated 30-day, extended 120-day), communication templates, legal considerations, bug bounty program integration, and CVE request process.

---

## Workflows

### Workflow 1: Quick Security Check (15 Minutes)

For pre-merge reviews or quick health checks:

```bash
# 1. Generate OWASP checklist
python scripts/vulnerability_scanner.py --target web --scope quick

# 2. Scan dependencies
python scripts/dependency_auditor.py --file package.json --severity high

# 3. Check for secrets in recent commits
# (Use gitleaks or trufflehog as described in Secret Scanning section)

# 4. Review HTTP security headers
curl -sI https://target.com | grep -iE "(strict-transport|content-security|x-frame|x-content-type)"
```

**Decision**: If any critical or high findings, block the merge.

### Workflow 2: Full Penetration Test (Multi-Day Assessment)

**Day 1 — Reconnaissance:**
1. Map the attack surface: endpoints, authentication flows, third-party integrations → verify: step output matches expected outcome
2. Run automated OWASP checklist (full scope) → verify: command exit code 0
3. Run dependency audit across all manifests → verify: command exit code 0
4. Run secret scan on full git history → verify: command exit code 0

**Day 2 — Manual Testing:**
1. Test authentication and authorization (IDOR, BOLA, BFLA) → verify: all tests pass
2. Test injection points (SQLi, XSS, SSRF, command injection) → verify: all tests pass
3. Test business logic flaws → verify: all tests pass
4. Test API-specific vulnerabilities (GraphQL, rate limiting, mass assignment) → verify: all tests pass

**Day 3 — Infrastructure and Reporting:**
1. Check cloud storage permissions → verify: all tests pass
2. Verify TLS configuration and security headers
3. Port scan for unnecessary services → verify: step output matches expected outcome
4. Compile findings into structured JSON → verify: step output matches expected outcome
5. Generate pen test report → verify: output file exists + no syntax error

```bash
# Generate final report
python scripts/pentest_report_generator.py --findings findings.json --format md --output pentest-report.md
```

### Workflow 3: CI/CD Security Gate

Automated security checks on every PR: secret scanning (TruffleHog), dependency audit (`npm audit`, `pip audit`), SAST (Semgrep with `p/security-audit`, `p/owasp-top-ten`), and security headers check on staging.

**Gate Policy**: Block merge on critical/high findings. Warn on medium. Log low/info.

---

## Anti-Patterns

1. **Testing in production without authorization** — Always get written permission and use staging/test environments when possible → verify: all checks pass
2. **Ignoring low-severity findings** — Low findings compound; a chain of lows can become a critical exploit path → verify: step output matches expected outcome
3. **Skipping responsible disclosure** — Every vulnerability found must be reported through proper channels → verify: step output matches expected outcome
4. **Relying solely on automated tools** — Tools miss business logic flaws, chained exploits, and novel attack vectors → verify: step output matches expected outcome
5. **Testing without a defined scope** — Scope creep leads to legal liability; document what is and isn't in scope → verify: all checks pass
6. **Reporting without remediation guidance** — Every finding must include actionable remediation steps → verify: step output matches expected outcome
7. **Storing evidence insecurely** — Pen test evidence (screenshots, payloads, tokens) is sensitive; encrypt and restrict access → verify: file content matches expected shape
8. **One-time testing** — Security testing must be continuous; integrate into CI/CD and schedule periodic assessments → verify: all checks pass

---

## Cross-References

| Skill | Relationship |
|-------|-------------|
| [senior-secops](../senior-secops/SKILL.md) | Defensive security operations — monitoring, incident response, SIEM configuration |
| [senior-security](../senior-security/SKILL.md) | Security policy and governance — frameworks, risk registers, compliance |
| [dependency-auditor](../../engineering/dependency-auditor/SKILL.md) | Deep supply chain security — SBOMs, license compliance, transitive risk |
| [code-reviewer](../code-reviewer/SKILL.md) | Code review practices — includes security review checklist |

## When NOT to use

- Task is unrelated to security pen testing — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Security Pen Testing needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for security pen testing
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving security pen testing
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
