---
name: "isms-audit-expert"
description: "Information Security Management System (ISMS) audit expert for ISO 27001 compliance verification, security control ass. Triggers: 'use isms-audit-expert', 'isms audit expert', 'isms-audit-expert task."
allowed-tools: Bash, Glob, Grep, Read
triggers:
  - ISMS audit
  - ISO 27001 audit
  - security audit
  - internal audit ISO 27001
  - security control assessment
  - certification audit
  - surveillance audit
  - audit finding
  - nonconformity
---

# ISMS Audit Expert

Internal and external ISMS audit management for ISO 27001 compliance verification, security control assessment, and certification support.

## Table of Contents

- [Audit Program Management](#audit-program-management)
- [Audit Execution](#audit-execution)
- [Control Assessment](#control-assessment)
- [Finding Management](#finding-management)
- [Certification Support](#certification-support)
- [Tools](#tools)
- [References](#references)

---

## Audit Program Management

### Risk-Based Audit Schedule

| Risk Level | Audit Frequency | Examples |
|------------|-----------------|----------|
| Critical | Quarterly | Privileged access, vulnerability management, logging |
| High | Semi-annual | Access control, incident response, encryption |
| Medium | Annual | Policies, awareness training, physical security |
| Low | Annual | Documentation, asset inventory |

### Annual Audit Planning Workflow

1. Review previous audit findings and risk assessment results → verify: findings count > 0 OR clean signal returned
2. Identify high-risk controls and recent security incidents → verify: step output matches expected outcome
3. Determine audit scope based on ISMS boundaries → verify: findings count > 0 OR clean signal returned
4. Assign auditors ensuring independence from audited areas → verify: findings count > 0 OR clean signal returned
5. Create audit schedule with resource allocation → verify: output exists + parses without error
6. Obtain management approval for audit plan → verify: findings count > 0 OR clean signal returned
7. **Validation:** Audit plan covers all Annex A controls within certification cycle → verify: findings count > 0 OR clean signal returned

### Auditor Competency Requirements

- ISO 27001 Lead Auditor certification (preferred)
- No operational responsibility for audited processes
- Understanding of technical security controls
- Knowledge of applicable regulations (GDPR, HIPAA)

---

## Audit Execution

### Pre-Audit Preparation

1. Review ISMS documentation (policies, SoA, risk assessment) → verify: step output matches expected outcome
2. Analyze previous audit reports and open findings → verify: file content matches expected shape
3. Prepare audit plan with interview schedule → verify: findings count > 0 OR clean signal returned
4. Notify auditees of audit scope and timing → verify: findings count > 0 OR clean signal returned
5. Prepare checklists for controls in scope → verify: step output matches expected outcome
6. **Validation:** All documentation received and reviewed before opening meeting → verify: file content matches expected shape

### Audit Conduct Steps

1. **Opening Meeting**
   - Confirm audit scope and objectives
   - Introduce audit team and methodology
   - Agree on communication channels and logistics

2. **Evidence Collection**
   - Interview control owners and operators
   - Review documentation and records
   - Observe processes in operation
   - Inspect technical configurations

3. **Control Verification**
   - Test control design (does it address the risk?)
   - Test control operation (is it working as intended?)
   - Sample transactions and records
   - Document all evidence collected

4. **Closing Meeting**
   - Present preliminary findings
   - Clarify any factual inaccuracies
   - Agree on finding classification
   - Confirm corrective action timelines

5. **Validation:** All controls in scope assessed with documented evidence → verify: step output matches expected outcome

---

## Control Assessment

### Control Testing Approach

1. Identify control objective from ISO 27002 → verify: step output matches expected outcome
2. Determine testing method (inquiry, observation, inspection, re-performance) → verify: all checks pass
3. Define sample size based on population and risk → verify: step output matches expected outcome
4. Execute test and document results → verify: command exit code 0
5. Evaluate control effectiveness → verify: step output matches expected outcome
6. **Validation:** Evidence supports conclusion about control status → verify: step output matches expected outcome

For detailed technical verification procedures by Annex A control, see [security-control-testing.md](references/security-control-testing.md).

---

## Finding Management

### Finding Classification

| Severity | Definition | Response Time |
|----------|------------|---------------|
| Major Nonconformity | Control failure creating significant risk | 30 days |
| Minor Nonconformity | Isolated deviation with limited impact | 90 days |
| Observation | Improvement opportunity | Next audit cycle |

### Finding Documentation Template

```
Finding ID: ISMS-[YEAR]-[NUMBER]
Control Reference: A.X.X - [Control Name]
Severity: [Major/Minor/Observation]

Evidence:
- [Specific evidence observed]
- [Records reviewed]
- [Interview statements]

Risk Impact:
- [Potential consequences if not addressed]

Root Cause:
- [Why the nonconformity occurred]

Recommendation:
- [Specific corrective action steps]
```

### Corrective Action Workflow

1. Auditee acknowledges finding and severity → verify: findings count > 0 OR clean signal returned
2. Root cause analysis completed within 10 days → verify: step output matches expected outcome
3. Corrective action plan submitted with target dates → verify: step output matches expected outcome
4. Actions implemented by responsible parties → verify: step output matches expected outcome
5. Auditor verifies effectiveness of corrections → verify: findings count > 0 OR clean signal returned
6. Finding closed with evidence of resolution → verify: step output matches expected outcome
7. **Validation:** Root cause addressed, recurrence prevented → verify: step output matches expected outcome

---

## Certification Support

### Stage 1 Audit Preparation

Ensure documentation is complete:
- [ ] ISMS scope statement
- [ ] Information security policy (management signed)
- [ ] Statement of Applicability
- [ ] Risk assessment methodology and results
- [ ] Risk treatment plan
- [ ] Internal audit results (past 12 months)
- [ ] Management review minutes

### Stage 2 Audit Preparation

Verify operational readiness:
- [ ] All Stage 1 findings addressed
- [ ] ISMS operational for minimum 3 months
- [ ] Evidence of control implementation
- [ ] Security awareness training records
- [ ] Incident response evidence (if applicable)
- [ ] Access review documentation

### Surveillance Audit Cycle

| Period | Focus |
|--------|-------|
| Year 1, Q2 | High-risk controls, Stage 2 findings follow-up |
| Year 1, Q4 | Continual improvement, control sample |
| Year 2, Q2 | Full surveillance |
| Year 2, Q4 | Re-certification preparation |

**Validation:** No major nonconformities at surveillance audits.

---

## Tools

### scripts/

| Script | Purpose | Usage |
|--------|---------|-------|
| `isms_audit_scheduler.py` | Generate risk-based audit plans | `python scripts/isms_audit_scheduler.py --year 2025 --format markdown` |

### Audit Planning Example

```bash
# Generate annual audit plan
python scripts/isms_audit_scheduler.py --year 2025 --output audit_plan.json

# With custom control risk ratings
python scripts/isms_audit_scheduler.py --controls controls.csv --format markdown
```

---

## References

| File | Content |
|------|---------|
| [iso27001-audit-methodology.md](references/iso27001-audit-methodology.md) | Audit program structure, pre-audit phase, certification support |
| [security-control-testing.md](references/security-control-testing.md) | Technical verification procedures for ISO 27002 controls |
| [cloud-security-audit.md](references/cloud-security-audit.md) | Cloud provider assessment, configuration security, IAM review |

---

## Audit Performance Metrics

| KPI | Target | Measurement |
|-----|--------|-------------|
| Audit plan completion | 100% | Audits completed vs. planned |
| Finding closure rate | >90% within SLA | Closed on time vs. total |
| Major nonconformities | 0 at certification | Count per certification cycle |
| Audit effectiveness | Incidents prevented | Security improvements implemented |

## Triggers

ISO 27001, ISMS audit, Annex A controls, Statement of Applicability (SOA), gap analysis, nonconformity management, internal audit, surveillance audit, or security certification prepara...

## When NOT to use

- Task is unrelated to isms audit expert — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Isms Audit Expert needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for isms audit expert
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving isms audit expert
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
