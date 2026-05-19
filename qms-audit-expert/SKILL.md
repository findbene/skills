---
name: "qms-audit-expert"
description: "ISO 13485 internal audit expertise for medical device QMS. Covers audit planning, execution, nonconformity classification. Triggers: 'use qms-audit-expert', 'qms audit expert', 'qms-audit-expert task."
allowed-tools: Bash, Glob, Grep, Read
triggers:
  - ISO 13485 audit
  - internal audit
  - QMS audit
  - audit planning
  - nonconformity classification
  - CAPA verification
  - audit checklist
  - audit finding
  - external audit prep
  - audit schedule
---

# QMS Audit Expert

ISO 13485 internal audit methodology for medical device quality management systems.

---

## Table of Contents

- [Audit Planning Workflow](#audit-planning-workflow)
- [Audit Execution](#audit-execution)
- [Nonconformity Management](#nonconformity-management)
- [External Audit Preparation](#external-audit-preparation)
- [Reference Documentation](#reference-documentation)
- [Tools](#tools)

---

## Audit Planning Workflow

Plan risk-based internal audit program:

1. List all QMS processes requiring audit → verify: step output matches expected outcome
2. Assign risk level to each process (High/Medium/Low) → verify: step output matches expected outcome
3. Review previous audit findings and trends → verify: step output matches expected outcome
4. Determine audit frequency by risk level → verify: step output matches expected outcome
5. Assign qualified auditors (verify independence)
6. Create annual audit schedule → verify: output file exists + no syntax error
7. Communicate schedule to process owners → verify: step output matches expected outcome
8. **Validation:** All ISO 13485 clauses covered within cycle → verify: step output matches expected outcome

### Risk-Based Audit Frequency

| Risk Level | Frequency | Criteria |
|------------|-----------|----------|
| High | Quarterly | Design control, CAPA, production validation |
| Medium | Semi-annual | Purchasing, training, document control |
| Low | Annual | Infrastructure, management review (if stable) |

### Audit Scope by Clause

| Clause | Process | Focus Areas |
|--------|---------|-------------|
| 4.2 | Document Control | Document approval, distribution, obsolete control |
| 5.6 | Management Review | Inputs complete, decisions documented, actions tracked |
| 6.2 | Training | Competency defined, records complete, effectiveness verified |
| 7.3 | Design Control | Inputs, reviews, V&V, transfer, changes |
| 7.4 | Purchasing | Supplier evaluation, incoming inspection |
| 7.5 | Production | Work instructions, process validation, DHR |
| 7.6 | Calibration | Equipment list, calibration status, out-of-tolerance |
| 8.2.2 | Internal Audit | Schedule compliance, auditor independence |
| 8.3 | NC Product | Identification, segregation, disposition |
| 8.5 | CAPA | Root cause, implementation, effectiveness |

### Auditor Independence

Verify auditor independence before assignment:

- [ ] Auditor not responsible for area being audited
- [ ] No direct reporting relationship to auditee
- [ ] Not involved in recent activities under audit
- [ ] Documented qualification for audit scope

---

## Audit Execution

Conduct systematic internal audit:

1. Prepare audit plan (scope, criteria, schedule) → verify: findings count > 0 OR clean signal returned
2. Review relevant documentation before audit → verify: findings count > 0 OR clean signal returned
3. Conduct opening meeting with auditee → verify: file content matches expected shape
4. Collect evidence (records, interviews, observation) → verify: step output matches expected outcome
5. Classify findings (Major/Minor/Observation) → verify: step output matches expected outcome
6. Conduct closing meeting with preliminary findings → verify: step output matches expected outcome
7. Prepare audit report within 5 business days → verify: findings count > 0 OR clean signal returned
8. **Validation:** All scope items covered, findings supported by evidence → verify: step output matches expected outcome

### Evidence Collection

| Method | Use For | Documentation |
|--------|---------|---------------|
| Document review | Procedures, records | Document number, version, date |
| Interview | Process understanding | Interviewee name, role, summary |
| Observation | Actual practice | What, where, when observed |
| Record trace | Process flow | Record IDs, dates, linkage |

### Audit Questions by Clause

**Document Control (4.2):**
- Show me the document master list
- How do you control obsolete documents?
- Show me evidence of document change approval

**Design Control (7.3):**
- Show me the Design History File for [product]
- Who participates in design reviews?
- Show me design input to output traceability

**CAPA (8.5):**
- Show me the CAPA log with open items
- How do you determine root cause?
- Show me effectiveness verification records

See `references/iso13485-audit-guide.md` for complete question sets.

### Finding Documentation

Document each finding with:

```
Requirement: [Specific ISO 13485 clause or procedure]
Evidence: [What was observed, reviewed, or heard]
Gap: [How evidence fails to meet requirement]
```

**Example:**
```
Requirement: ISO 13485:2016 Clause 7.6 requires calibration
at specified intervals.

Evidence: Calibration records for pH meter (EQ-042) show
last calibration 2024-01-15. Calibration interval is
12 months. Today is 2025-03-20.

Gap: Equipment is 2 months overdue for calibration,
representing a gap in calibration program execution.
```

---

## Nonconformity Management

Classify and manage audit findings:

1. Evaluate finding against classification criteria → verify: step output matches expected outcome
2. Assign severity (Major/Minor/Observation) → verify: step output matches expected outcome
3. Document finding with objective evidence → verify: step output matches expected outcome
4. Communicate to process owner → verify: step output matches expected outcome
5. Initiate CAPA for Major/Minor findings → verify: step output matches expected outcome
6. Track to closure → verify: step output matches expected outcome
7. Verify effectiveness at follow-up
8. **Validation:** Finding closed only after effective CAPA → verify: step output matches expected outcome

### Classification Criteria

| Category | Definition | CAPA Required | Timeline |
|----------|------------|---------------|----------|
| Major | Systematic failure or absence of element | Yes | 30 days |
| Minor | Isolated lapse or partial implementation | Recommended | 60 days |
| Observation | Improvement opportunity | Optional | As appropriate |

### Classification Decision

```
Is required element absent or failed?
├── Yes → Systematic (multiple instances)? → MAJOR
│   └── No → Could affect product safety? → MAJOR
│       └── No → MINOR
└── No → Deviation from procedure?
    ├── Yes → Recurring? → MAJOR
    │   └── No → MINOR
    └── No → Improvement opportunity? → OBSERVATION
```

### CAPA Integration

| Finding Severity | CAPA Depth | Verification |
|------------------|------------|--------------|
| Major | Full root cause analysis (5-Why, Fishbone) | Next audit or within 6 months |
| Minor | Immediate cause identification | Next scheduled audit |
| Observation | Not required | Noted at next audit |

See `references/nonconformity-classification.md` for detailed guidance.

---

## External Audit Preparation

Prepare for certification body or regulatory audit:

1. Complete all scheduled internal audits → verify: findings count > 0 OR clean signal returned
2. Verify all findings closed with effective CAPA
3. Review documentation for currency and accuracy → verify: step output matches expected outcome
4. Conduct management review with audit as input → verify: findings count > 0 OR clean signal returned
5. Prepare facility and personnel → verify: step output matches expected outcome
6. Conduct mock audit (full scope) → verify: findings count > 0 OR clean signal returned
7. Brief personnel on audit protocol → verify: findings count > 0 OR clean signal returned
8. **Validation:** Mock audit findings addressed before external audit → verify: findings count > 0 OR clean signal returned

### Pre-Audit Readiness Checklist

**Documentation:**
- [ ] Quality Manual current
- [ ] Procedures reflect actual practice
- [ ] Records complete and retrievable
- [ ] Previous audit findings closed

**Personnel:**
- [ ] Key personnel available during audit
- [ ] Subject matter experts identified
- [ ] Personnel briefed on audit protocol
- [ ] Escorts assigned

**Facility:**
- [ ] Work areas organized
- [ ] Documents at point of use current
- [ ] Equipment calibration status visible
- [ ] Nonconforming product segregated

### Mock Audit Protocol

1. Use external auditor or qualified internal auditor → verify: findings count > 0 OR clean signal returned
2. Cover full scope of upcoming external audit → verify: findings count > 0 OR clean signal returned
3. Simulate actual audit conditions (timing, formality) → verify: findings count > 0 OR clean signal returned
4. Document findings as for real audit → verify: findings count > 0 OR clean signal returned
5. Address all Major and Minor findings before external audit → verify: findings count > 0 OR clean signal returned
6. Brief management on readiness status → verify: file content matches expected shape

---

## Reference Documentation

### ISO 13485 Audit Guide

`references/iso13485-audit-guide.md` contains:

- Clause-by-clause audit methodology
- Sample audit questions for each clause
- Evidence collection requirements
- Common nonconformities by clause
- Finding severity classification

### Nonconformity Classification

`references/nonconformity-classification.md` contains:

- Severity classification criteria and decision tree
- Impact vs. occurrence matrix
- CAPA integration requirements
- Finding documentation templates
- Closure requirements by severity

---

## Tools

### Audit Schedule Optimizer

```bash
# Generate optimized audit schedule
python scripts/audit_schedule_optimizer.py --processes processes.json

# Interactive mode
python scripts/audit_schedule_optimizer.py --interactive

# JSON output for integration
python scripts/audit_schedule_optimizer.py --processes processes.json --output json
```

Generates risk-based audit schedule considering:
- Process risk level
- Previous findings
- Days since last audit
- Criticality scores

**Output includes:**
- Prioritized audit schedule
- Quarterly distribution
- Overdue audit alerts
- Resource recommendations

### Sample Process Input

```json
{
  "processes": [
    {
      "name": "Design Control",
      "iso_clause": "7.3",
      "risk_level": "HIGH",
      "last_audit_date": "2024-06-15",
      "previous_findings": 2
    },
    {
      "name": "Document Control",
      "iso_clause": "4.2",
      "risk_level": "MEDIUM",
      "last_audit_date": "2024-09-01",
      "previous_findings": 0
    }
  ]
}
```

---

## Audit Program Metrics

Track audit program effectiveness:

| Metric | Target | Measurement |
|--------|--------|-------------|
| Schedule compliance | >90% | Audits completed on time |
| Finding closure rate | >95% | Findings closed by due date |
| Repeat findings | <10% | Same finding in consecutive audits |
| CAPA effectiveness | >90% | Verified effective at follow-up |
| Auditor utilization | 4 days/month | Audit days per qualified auditor |

## When NOT to use

- Task is unrelated to qms audit expert — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Qms Audit Expert needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for qms audit expert
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving qms audit expert
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
