---
name: security-privacy
description: "Security and privacy implementation for applications covering input validation, XSS/CSRF prevention, data encryption, and privacy compliance. Use this skill any time security vulnerabilities need to be prevented in code, privacy requirements need to be implemented, data handling needs to be secured, or security best practices need to be applied. Trigger immediately on: \"security\", \"privacy\", \"XSS\", \"CSRF\", \"input validation\", \"SQL injection\", \"data encryption\", \"secure coding\", \"HTTPS\", \"secure headers\", \"data privacy\", \"GDPR compliance\", \"PII\", \"secure this code\". If someone says \"is this code secure?\" or \"add security to this\" this skill MUST trigger."
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
1. **Injection**: Parameterized queries, input validation
2. **Broken Authentication**: Strong session management
3. **Sensitive Data Exposure**: Encryption at rest and in transit
4. **XXE**: Disable external entities
5. **Broken Access Control**: Server-side checks
6. **Security Misconfiguration**: Secure defaults
7. **XSS**: Output encoding, CSP headers
8. **Insecure Deserialization**: Validate serialized data
9. **Vulnerable Components**: Keep dependencies updated
10. **Insufficient Logging**: Audit trails

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
