---
name: "quality-manager-qms-iso13485"
description: "ISO 13485 Quality Management System implementation and maintenance for medical device. Triggers: 'use quality-manager-qms-iso13485', 'quality manager qms iso13485', 'quality-manager-qms-iso13485 task."
allowed-tools: Glob, Grep, Read
triggers:
  - ISO 13485
  - QMS implementation
  - quality management system
  - document control
  - internal audit
  - management review
  - quality manual
  - CAPA process
  - process validation
  - design control
  - supplier qualification
  - quality records
---

# Quality Manager - QMS ISO 13485 Specialist

ISO 13485:2016 Quality Management System implementation, maintenance, and certification support for medical device organizations.

---

## Table of Contents

- [QMS Implementation Workflow](#qms-implementation-workflow)
- [Document Control Workflow](#document-control-workflow)
- [Internal Audit Workflow](#internal-audit-workflow)
- [Process Validation Workflow](#process-validation-workflow)
- [Supplier Qualification Workflow](#supplier-qualification-workflow)
- [QMS Process Reference](#qms-process-reference)
- [Decision Frameworks](#decision-frameworks)
- [Tools and References](#tools-and-references)

---

## QMS Implementation Workflow

Implement ISO 13485:2016 compliant quality management system from gap analysis through certification.

### Workflow: Initial QMS Implementation

1. Conduct gap analysis against ISO 13485:2016 requirements → verify: step output matches expected outcome
2. Document current state vs. required state for each clause → verify: step output matches expected outcome
3. Prioritize gaps by: → verify: step output matches expected outcome
   - Regulatory criticality
   - Risk to product safety
   - Resource requirements
4. Develop implementation roadmap with milestones → verify: step output matches expected outcome
5. Establish Quality Manual per Clause 4.2.2: → verify: step output matches expected outcome
   - QMS scope with justified exclusions
   - Process interactions
   - Procedure references
6. Create required documented procedures — see [Mandatory Documented Procedures](#quick-reference-mandatory-documented-procedures) for the full list → verify: output file exists + no syntax error
7. Deploy processes with training → verify: step output matches expected outcome
8. **Validation:** Gap analysis complete; Quality Manual approved; all required procedures documented and trained → verify: step output matches expected outcome

> Use the Gap Analysis Matrix template in [qms-process-templates.md](references/qms-process-templates.md) to document clause-by-clause current state, gaps, priority, and actions.

### QMS Structure

| Level | Document Type | Example |
|-------|---------------|---------|
| 1 | Quality Manual | QM-001 |
| 2 | Procedures | SOP-02-001 |
| 3 | Work Instructions | WI-06-012 |
| 4 | Records | Training records |

---

## Document Control Workflow

Establish and maintain document control per ISO 13485 Clause 4.2.3.

### Workflow: Document Creation and Approval

1. Identify need for new document or revision → verify: step output matches expected outcome
2. Assign document number per numbering convention: → verify: step output matches expected outcome
   - Format: `[TYPE]-[AREA]-[SEQUENCE]-[REV]`
   - Example: `SOP-02-001-01`
3. Draft document using approved template → verify: step output matches expected outcome
4. Route for review to subject matter experts → verify: step output matches expected outcome
5. Collect and address review comments → verify: package installed + import succeeds
6. Obtain required approvals based on document type → verify: step output matches expected outcome
7. Update Document Master List → verify: step output matches expected outcome
8. **Validation:** Document numbered correctly; all reviewers signed; Master List updated → verify: step output matches expected outcome

### Document Numbering Convention

| Prefix | Document Type | Approval Authority |
|--------|---------------|-------------------|
| QM | Quality Manual | Management Rep + CEO |
| POL | Policy | Department Head + QA |
| SOP | Procedure | Process Owner + QA |
| WI | Work Instruction | Supervisor + QA |
| TF | Template/Form | Process Owner |
| SPEC | Specification | Engineering + QA |

### Area Codes

| Code | Area | Examples |
|------|------|----------|
| 01 | Quality Management | Quality Manual, policy |
| 02 | Document Control | This procedure |
| 03 | Training | Competency procedures |
| 04 | Design | Design control |
| 05 | Purchasing | Supplier management |
| 06 | Production | Manufacturing |
| 07 | Quality Control | Inspection, testing |
| 08 | CAPA | Corrective actions |

### Document Change Control

| Change Type | Approval Level | Examples |
|-------------|----------------|----------|
| Administrative | Document Control | Typos, formatting |
| Minor | Process Owner + QA | Clarifications |
| Major | Full review cycle | Process changes |
| Emergency | Expedited + retrospective | Safety issues |

### Document Review Schedule

| Document Type | Review Period | Trigger for Unscheduled Review |
|---------------|---------------|-------------------------------|
| Quality Manual | Annual | Organizational change |
| Procedures | Annual | Audit finding, regulation change |
| Work Instructions | 2 years | Process change |
| Forms | 2 years | User feedback |

---

## Internal Audit Workflow

Plan and execute internal audits per ISO 13485 Clause 8.2.4.

### Workflow: Annual Audit Program

1. Identify processes and areas requiring audit coverage → verify: step output matches expected outcome
2. Assess risk factors for audit frequency: → verify: step output matches expected outcome
   - Previous audit findings
   - Regulatory changes
   - Process changes
   - Complaint trends
3. Assign qualified auditors (independent of area audited) → verify: step output matches expected outcome
4. Develop annual audit schedule → verify: step output matches expected outcome
5. Obtain management approval → verify: step output matches expected outcome
6. Communicate schedule to process owners → verify: step output matches expected outcome
7. Track completion and reschedule as needed → verify: step output matches expected outcome
8. **Validation:** All processes covered; auditors qualified and independent; schedule approved → verify: step output matches expected outcome

> Use the Audit Program Template in [qms-process-templates.md](references/qms-process-templates.md) to schedule audits by clause and quarter across processes such as Document Control (4.2.3/4.2.4), Management Review (5.6), Design Control (7.3), Production (7.5), and CAPA (8.5.2/8.5.3).

### Workflow: Individual Audit Execution

1. Prepare audit plan with scope, criteria, and schedule → verify: step output matches expected outcome
2. Notify auditee minimum 1 week prior → verify: step output matches expected outcome
3. Review procedures and previous audit results → verify: step output matches expected outcome
4. Prepare audit checklist → verify: all tests pass
5. Conduct opening meeting → verify: file readable + content matches expected shape
6. Collect evidence through: → verify: step output matches expected outcome
   - Document review
   - Record sampling
   - Process observation
   - Personnel interviews
7. Classify findings: → verify: step output matches expected outcome
   - Major NC: Absence or breakdown of system
   - Minor NC: Single lapse or deviation
   - Observation: Risk of future NC
8. Conduct closing meeting → verify: step output matches expected outcome
9. Issue audit report within 5 business days → verify: step output matches expected outcome
10. **Validation:** All checklist items addressed; findings supported by evidence; report distributed → verify: package installed + import succeeds

### Auditor Qualification Requirements

| Criterion | Requirement |
|-----------|-------------|
| Training | ISO 13485 awareness + auditor training |
| Experience | Minimum 1 audit as observer |
| Independence | Not auditing own work area |
| Competence | Understanding of audited process |

### Finding Classification Guide

| Classification | Criteria | Response Time |
|----------------|----------|---------------|
| Major NC | System absence, total breakdown, regulatory violation | 30 days for CAPA |
| Minor NC | Single instance, partial compliance | 60 days for CAPA |
| Observation | Potential risk, improvement opportunity | Track in next audit |

---

## Process Validation Workflow

Validate special processes per ISO 13485 Clause 7.5.6.

### Workflow: Process Validation Protocol

1. Identify processes requiring validation: → verify: step output matches expected outcome
   - Output cannot be verified by inspection
   - Deficiencies appear only in use
   - Sterilization, welding, sealing, software
2. Form validation team with subject matter experts → verify: step output matches expected outcome
3. Write validation protocol including: → verify: output file exists + no syntax error
   - Process description and parameters
   - Equipment and materials
   - Acceptance criteria
   - Statistical approach
4. Execute IQ: verify equipment installed correctly and document specifications
5. Execute OQ: test parameter ranges and verify process control
6. Execute PQ: run production conditions and verify output meets requirements
7. Write validation report with conclusions → verify: output file exists + no syntax error
8. **Validation:** IQ/OQ/PQ complete; acceptance criteria met; validation report approved → verify: step output matches expected outcome

### Validation Documentation Requirements

| Phase | Content | Evidence |
|-------|---------|----------|
| Protocol | Objectives, methods, criteria | Approved protocol |
| IQ | Equipment verification | Installation records |
| OQ | Parameter verification | Test results |
| PQ | Performance verification | Production data |
| Report | Summary, conclusions | Approval signatures |

### Revalidation Triggers

| Trigger | Action Required |
|---------|-----------------|
| Equipment change | Assess impact, revalidate affected phases |
| Parameter change | OQ and PQ minimum |
| Material change | Assess impact, PQ minimum |
| Process failure | Full revalidation |
| Periodic | Per validation schedule (typically 3 years) |

### Special Process Examples

| Process | Validation Standard | Critical Parameters |
|---------|--------------------|--------------------|
| EO Sterilization | ISO 11135 | Temperature, humidity, EO concentration, time |
| Steam Sterilization | ISO 17665 | Temperature, pressure, time |
| Radiation Sterilization | ISO 11137 | Dose, dose uniformity |
| Sealing | Internal | Temperature, pressure, dwell time |
| Welding | ISO 11607 | Heat, pressure, speed |

---

## Supplier Qualification Workflow

Evaluate and approve suppliers per ISO 13485 Clause 7.4.

### Workflow: New Supplier Qualification

1. Identify supplier category: → verify: step output matches expected outcome
   - Category A: Critical (affects safety/performance)
   - Category B: Major (affects quality)
   - Category C: Minor (indirect impact)
2. Request supplier information: → verify: step output matches expected outcome
   - Quality certifications
   - Product specifications
   - Quality history
3. Evaluate supplier based on: → verify: step output matches expected outcome
   - Quality system (ISO certification)
   - Technical capability
   - Quality history
   - Financial stability
4. For Category A suppliers: → verify: step output matches expected outcome
   - Conduct on-site audit
   - Require quality agreement
5. Calculate qualification score → verify: step output matches expected outcome
6. Make approval decision: → verify: step output matches expected outcome
   - >80: Approved
   - 60-80: Conditional approval
   - <60: Not approved
7. Add to Approved Supplier List → verify: package installed + import succeeds
8. **Validation:** Evaluation criteria scored; qualification records complete; supplier categorized → verify: step output matches expected outcome

### Supplier Evaluation Criteria

| Criterion | Weight | Scoring |
|-----------|--------|---------|
| Quality System | 30% | ISO 13485=30, ISO 9001=20, Documented=10, None=0 |
| Quality History | 25% | Reject rate: <1%=25, 1-3%=15, >3%=0 |
| Delivery | 20% | On-time: >95%=20, 90-95%=10, <90%=0 |
| Technical Capability | 15% | Exceeds=15, Meets=10, Marginal=5 |
| Financial Stability | 10% | Strong=10, Adequate=5, Questionable=0 |

### Supplier Category Requirements

| Category | Qualification | Monitoring | Agreement |
|----------|---------------|------------|-----------|
| A - Critical | On-site audit | Annual review | Quality agreement |
| B - Major | Questionnaire | Semi-annual review | Quality requirements |
| C - Minor | Assessment | Issue-based | Standard terms |

### Supplier Performance Metrics

| Metric | Target | Calculation |
|--------|--------|-------------|
| Accept Rate | >98% | (Accepted lots / Total lots) × 100 |
| On-Time Delivery | >95% | (On-time / Total orders) × 100 |
| Response Time | <5 days | Average days to resolve issues |
| Documentation | 100% | (Complete CoCs / Required CoCs) × 100 |

---

## QMS Process Reference

For detailed requirements and audit questions for each ISO 13485:2016 clause, see [iso13485-clause-requirements.md](references/iso13485-clause-requirements.md).

### Management Review Required Inputs (Clause 5.6.2)

| Input | Source | Prepared By |
|-------|--------|-------------|
| Audit results | Internal and external audits | QA Manager |
| Customer feedback | Complaints, surveys | Customer Quality |
| Process performance | Process metrics | Process Owners |
| Product conformity | Inspection data, NCs | QC Manager |
| CAPA status | CAPA system | CAPA Officer |
| Previous actions | Prior review records | QMR |
| Changes affecting QMS | Regulatory, organizational | RA Manager |
| Recommendations | All sources | All Managers |

### Record Retention Requirements

| Record Type | Minimum Retention | Regulatory Basis |
|-------------|-------------------|------------------|
| Device Master Record | Life of device + 2 years | 21 CFR 820.181 |
| Device History Record | Life of device + 2 years | 21 CFR 820.184 |
| Design History File | Life of device + 2 years | 21 CFR 820.30 |
| Complaint Records | Life of device + 2 years | 21 CFR 820.198 |
| Training Records | Employment + 3 years | Best practice |
| Audit Records | 7 years | Best practice |
| CAPA Records | 7 years | Best practice |
| Calibration Records | Equipment life + 2 years | Best practice |

---

## Decision Frameworks

### Exclusion Justification (Clause 4.2.2)

| Clause | Permissible Exclusion | Justification Required |
|--------|----------------------|------------------------|
| 6.4.2 | Contamination control | Product not affected by contamination |
| 7.3 | Design and development | Organization does not design products |
| 7.5.2 | Product cleanliness | No cleanliness requirements |
| 7.5.3 | Installation | No installation activities |
| 7.5.4 | Servicing | No servicing activities |
| 7.5.5 | Sterile products | No sterile products |

### Nonconformity Disposition Decision Tree

```
Nonconforming Product Identified
            │
            ▼
    Can it be reworked?
            │
       Yes──┴──No
        │       │
        ▼       ▼
    Is rework     Can it be used
    procedure     as is?
    available?        │
        │        Yes──┴──No
    Yes─┴─No     │       │
     │    │     ▼       ▼
     ▼    ▼  Concession  Scrap or
  Rework  Create    approval    return to
  per SOP  rework    needed?    supplier
          procedure     │
                    Yes─┴─No
                     │    │
                     ▼    ▼
                 Customer  Use as is
                 approval  with MRB
                          approval
```

### CAPA Initiation Criteria

| Source | Automatic CAPA | Evaluate for CAPA |
|--------|----------------|-------------------|
| Customer complaint | Safety-related | All others |
| External audit | Major NC | Minor NC |
| Internal audit | Major NC | Repeat minor NC |
| Product NC | Field failure | Trend exceeds threshold |
| Process deviation | Safety impact | Repeated deviations |

---

## Tools and References

### Scripts

| Tool | Purpose | Usage |
|------|---------|-------|
| [qms_audit_checklist.py](scripts/qms_audit_checklist.py) | Generate audit checklists by clause or process | `python qms_audit_checklist.py --help` |

**Audit Checklist Generator Features:**
- Generate clause-specific checklists (e.g., `--clause 7.3`)
- Generate process-based checklists (e.g., `--process design-control`)
- Full system audit checklist (`--audit-type system`)
- Text or JSON output formats
- Interactive mode for guided selection

### References

| Document | Content |
|----------|---------|
| [iso13485-clause-requirements.md](references/iso13485-clause-requirements.md) | Detailed requirements for each ISO 13485:2016 clause with audit questions |
| [qms-process-templates.md](references/qms-process-templates.md) | Ready-to-use templates for gap analysis, audit program, document control, CAPA, supplier, training |

### Quick Reference: Mandatory Documented Procedures

| Procedure | Clause | Key Elements |
|-----------|--------|--------------|
| Document Control | 4.2.3 | Approval, distribution, obsolete control |
| Record Control | 4.2.4 | Identification, retention, disposal |
| Internal Audit | 8.2.4 | Program, auditor qualification, reporting |
| NC Product Control | 8.3 | Identification, segregation, disposition |
| Corrective Action | 8.5.2 | Root cause, implementation, verification |
| Preventive Action | 8.5.3 | Risk identification, implementation |

---

## Related Skills

| Skill | Integration Point |
|-------|-------------------|
| [quality-manager-qmr](../quality-manager-qmr/) | Management review, quality policy |
| [capa-officer](../capa-officer/) | CAPA system management |
| [qms-audit-expert](../qms-audit-expert/) | Advanced audit techniques |
| [quality-documentation-manager](../quality-documentation-manager/) | DHF, DMR, DHR management |
| [risk-management-specialist](../risk-management-specialist/) | ISO 14971 integration |

## When NOT to use

- Task is unrelated to quality manager qms iso13485 — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Quality Manager Qms Iso13485 needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for quality manager qms iso13485
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving quality manager qms iso13485
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
