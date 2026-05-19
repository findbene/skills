---
name: security-privacy
description: 'Security and privacy implementation for applications covering input validation, XSS/CSRF prevention, data encryption, and privacy. Triggers: "use security-privacy", "security privacy", "security task.'
allowed-tools: Glob, Grep, Read
---

# Security & Privacy

Guide for implementing secure, privacy-compliant applications.

## Security Fundamentals

### Authentication
- Use established libraries (NextAuth, Passport, Auth0)
- Implement MFA where possible
- Secure password storage (bcrypt, argon2)
- Session management best practices

### Authorization
- Role-Based Access Control (RBAC)
- Attribute-Based Access Control (ABAC)
- Principle of least privilege
- Validate permissions server-side

### Input Validation
- Validate all user input server-side
- Sanitize data before display (XSS prevention)
- Parameterized queries (SQL injection prevention)
- File upload validation

### OWASP Top 10 Prevention
1. **Injection**: Parameterized queries, input validation → verify: step output matches expected outcome
2. **Broken Authentication**: Strong session management → verify: step output matches expected outcome
3. **Sensitive Data Exposure**: Encryption at rest and in transit → verify: step output matches expected outcome
4. **XXE**: Disable external entities → verify: step output matches expected outcome
5. **Broken Access Control**: Server-side checks → verify: step output matches expected outcome
6. **Security Misconfiguration**: Secure defaults → verify: step output matches expected outcome
7. **XSS**: Output encoding, CSP headers → verify: step output matches expected outcome
8. **Insecure Deserialization**: Validate serialized data → verify: all checks pass
9. **Vulnerable Components**: Keep dependencies updated → verify: step output matches expected outcome
10. **Insufficient Logging**: Audit trails → verify: findings count > 0 OR clean signal returned

## Privacy Compliance

### GDPR Requirements
- Lawful basis for processing
- Data minimization and purpose limitation
- User consent management
- Right to access/delete/port data
- Privacy by design

### CCPA Requirements
- Disclosure of data collection
- Opt-out of data sale
- Right to deletion
- Non-discrimination

### Implementation Checklist
- Privacy policy documentation
- Cookie consent mechanism
- Data retention policies
- Encryption for PII
- Audit logging
- Data deletion workflows
- Third-party data sharing agreements

## Secure Coding Practices

```javascript
// BAD: SQL Injection vulnerable
const query = `SELECT * FROM users WHERE id = ${userId}`;

// GOOD: Parameterized query
const query = 'SELECT * FROM users WHERE id = $1';
await db.query(query, [userId]);
```

```javascript
// BAD: XSS vulnerable
element.innerHTML = userInput;

// GOOD: Sanitized
element.textContent = userInput;
```

## When NOT to use

- Pen-testing a deployed app — use `security-pen-testing` or `red-team`
- Threat detection / SIEM setup — use `threat-detection` or `senior-secops`
- ISO 27001 / SOC2 / GDPR compliance documentation — use `information-security-manager-iso27001`, `soc2-compliance`, `gdpr-dsgvo-expert`
- Cloud network/IAM security — use `cloud-security`
- AI model safety / prompt injection — use `ai-security`

## Red Flags

| Rationalization | Reality |
|---|---|
| "We will add rate limiting later" | Endpoints without rate limits get abused within hours of being indexed; add it before launch |
| "Client-side validation is enough" | Server-side validation is non-negotiable; clients can be bypassed via direct API calls |
| "Argon2 / bcrypt is slow, use SHA256" | Slow is the point — fast hashes are crackable; never use SHA for passwords |
| "Sanitize on display only" | Defense in depth: validate on input AND escape on output; one layer fails inevitably |

## Output Contract

Finished output must contain:
- Threat model: list of inputs and trust boundaries
- Input validation at every API boundary with Zod (or equivalent) schemas
- Authentication choice with rationale (and MFA enabled where possible)
- Authorization checks at data layer, not just route layer
- Encryption at rest and in transit (which fields, which algorithm)
- Logging policy (what is logged, what is redacted)
- HTTP security headers (CSP, HSTS, X-Frame-Options, X-Content-Type-Options)
- OWASP Top 10 checklist with each item explicitly addressed

## Examples

**Example 1 — Secure a new login flow**
- Input: "Add login to our Next.js app"
- Action: Recommend NextAuth + argon2 hashing + bcrypt-cost 12 → MFA via TOTP → rate-limit `/login` (5/min/IP) → JWT with rotation → CSRF protection on form → Set-Cookie with HttpOnly + Secure + SameSite=Lax
- Output: NextAuth config, password hashing util, rate-limit middleware, session config, CSRF helper, env vars listed (no secrets committed)

**Example 2 — Audit an existing form endpoint**
- Input: "Review POST /api/feedback for security"
- Action: Check input validation (Zod missing → add) → SQL injection (parametrized OK) → XSS (output not escaped → fix) → rate limit (none → add) → CSRF (no token → add) → authz (none, but should require login)
- Output: 5-issue audit report with diffs, ranked critical→low, regression tests for each fix
