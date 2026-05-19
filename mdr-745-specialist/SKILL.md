---
name: "mdr-745-specialist"
description: "EU MDR 2017/745 compliance specialist for medical device classification, technical documentation, clinical evidence. Triggers: 'use mdr-745-specialist', 'mdr 745 specialist', 'mdr-745-specialist task."
allowed-tools: Bash, Glob, Grep, Read
triggers:
  - MDR compliance
  - EU MDR
  - medical device classification
  - Annex VIII
  - technical documentation
  - clinical evaluation
  - PMCF
  - EUDAMED
  - UDI
  - notified body
---

# MDR 2017/745 Specialist

EU MDR compliance patterns for medical device classification, technical documentation, and clinical evidence.

---

## Table of Contents

- [Device Classification Workflow](#device-classification-workflow)
- [Technical Documentation](#technical-documentation)
- [Clinical Evidence](#clinical-evidence)
- [Post-Market Surveillance](#post-market-surveillance)
- [EUDAMED and UDI](#eudamed-and-udi)
- [Reference Documentation](#reference-documentation)
- [Tools](#tools)

---

## Device Classification Workflow

Classify device under MDR Annex VIII:

1. Identify device duration (transient, short-term, long-term) → verify: step output matches expected outcome
2. Determine invasiveness level (non-invasive, body orifice, surgical) → verify: step output matches expected outcome
3. Assess body system contact (CNS, cardiac, other) → verify: step output matches expected outcome
4. Check if active device (energy dependent) → verify: all tests pass
5. Apply classification rules 1-22 → verify: diff matches intended change
6. For software, apply MDCG 2019-11 algorithm → verify: diff matches intended change
7. Document classification rationale → verify: step output matches expected outcome
8. **Validation:** Classification confirmed with Notified Body → verify: step output matches expected outcome

### Classification Matrix

| Factor | Class I | Class IIa | Class IIb | Class III |
|--------|---------|-----------|-----------|-----------|
| Duration | Any | Short-term | Long-term | Long-term |
| Invasiveness | Non-invasive | Body orifice | Surgical | Implantable |
| System | Any | Non-critical | Critical organs | CNS/cardiac |
| Risk | Lowest | Low-medium | Medium-high | Highest |

### Software Classification (MDCG 2019-11)

| Information Use | Condition Severity | Class |
|-----------------|-------------------|-------|
| Informs decision | Non-serious | IIa |
| Informs decision | Serious | IIb |
| Drives/treats | Critical | III |

### Classification Examples

**Example 1: Absorbable Surgical Suture**
- Rule 8 (implantable, long-term)
- Duration: > 30 days (absorbed)
- Contact: General tissue
- Classification: **Class IIb**

**Example 2: AI Diagnostic Software**
- Rule 11 + MDCG 2019-11
- Function: Diagnoses serious condition
- Classification: **Class IIb**

**Example 3: Cardiac Pacemaker**
- Rule 8 (implantable)
- Contact: Central circulatory system
- Classification: **Class III**

---

## Technical Documentation

Prepare technical file per Annex II and III:

1. Create device description (variants, accessories, intended purpose) → verify: output exists + parses without error
2. Develop labeling (Article 13 requirements, IFU) → verify: step output matches expected outcome
3. Document design and manufacturing process → verify: step output matches expected outcome
4. Complete GSPR compliance matrix → verify: step output matches expected outcome
5. Prepare benefit-risk analysis → verify: step output matches expected outcome
6. Compile verification and validation evidence → verify: step output matches expected outcome
7. Integrate risk management file (ISO 14971) → verify: step output matches expected outcome
8. **Validation:** Technical file reviewed for completeness → verify: step output matches expected outcome

### Technical File Structure

```
ANNEX II TECHNICAL DOCUMENTATION
├── Device description and UDI-DI
├── Label and instructions for use
├── Design and manufacturing info
├── GSPR compliance matrix
├── Benefit-risk analysis
├── Verification and validation
└── Clinical evaluation report
```

### GSPR Compliance Checklist

| Requirement | Evidence | Status |
|-------------|----------|--------|
| Safe design (GSPR 1-3) | Risk management file | ☐ |
| Chemical properties (GSPR 10.1) | Biocompatibility report | ☐ |
| Infection risk (GSPR 10.2) | Sterilization validation | ☐ |
| Software requirements (GSPR 17) | IEC 62304 documentation | ☐ |
| Labeling (GSPR 23) | Label artwork, IFU | ☐ |

### Conformity Assessment Routes

| Class | Route | NB Involvement |
|-------|-------|----------------|
| I | Annex II self-declaration | None |
| Is/Im | Annex II + IX/XI | Sterile/measuring aspects |
| IIa | Annex II + IX or XI | Product or QMS |
| IIb | Annex IX + X or X + XI | Type exam + production |
| III | Annex IX + X | Full QMS + type exam |

---

## Clinical Evidence

Develop clinical evidence strategy per Annex XIV:

1. Define clinical claims and endpoints → verify: step output matches expected outcome
2. Conduct systematic literature search → verify: step output matches expected outcome
3. Appraise clinical data quality → verify: step output matches expected outcome
4. Assess equivalence (technical, biological, clinical) → verify: step output matches expected outcome
5. Identify evidence gaps → verify: step output matches expected outcome
6. Determine if clinical investigation required → verify: step output matches expected outcome
7. Prepare Clinical Evaluation Report (CER) → verify: step output matches expected outcome
8. **Validation:** CER reviewed by qualified evaluator → verify: step output matches expected outcome

### Evidence Requirements by Class

| Class | Minimum Evidence | Investigation |
|-------|------------------|---------------|
| I | Risk-benefit analysis | Not typically required |
| IIa | Literature + post-market | May be required |
| IIb | Systematic literature review | Often required |
| III | Comprehensive clinical data | Required (Article 61) |

### Clinical Evaluation Report Structure

```
CER CONTENTS
├── Executive summary
├── Device scope and intended purpose
├── Clinical background (state of the art)
├── Literature search methodology
├── Data appraisal and analysis
├── Safety and performance conclusions
├── Benefit-risk determination
└── PMCF plan summary
```

### Qualified Evaluator Requirements

- Medical degree or equivalent healthcare qualification
- 4+ years clinical experience in relevant field
- Training in clinical evaluation methodology
- Understanding of MDR requirements

---

## Post-Market Surveillance

Establish PMS system per Chapter VII:

1. Develop PMS plan (Article 84) → verify: step output matches expected outcome
2. Define data collection methods → verify: step output matches expected outcome
3. Establish complaint handling procedures → verify: step output matches expected outcome
4. Create vigilance reporting process → verify: output exists + parses without error
5. Plan Periodic Safety Update Reports (PSUR) → verify: step output matches expected outcome
6. Integrate with PMCF activities → verify: step output matches expected outcome
7. Define trend analysis and signal detection → verify: step output matches expected outcome
8. **Validation:** PMS system audited annually → verify: findings count > 0 OR clean signal returned

### PMS System Components

| Component | Requirement | Frequency |
|-----------|-------------|-----------|
| PMS Plan | Article 84 | Maintain current |
| PSUR | Class IIa and higher | Per class schedule |
| PMCF Plan | Annex XIV Part B | Update with CER |
| PMCF Report | Annex XIV Part B | Annual (Class III) |
| Vigilance | Articles 87-92 | As events occur |

### PSUR Schedule

| Class | Frequency |
|-------|-----------|
| Class III | Annual |
| Class IIb implantable | Annual |
| Class IIb | Every 2 years |
| Class IIa | When necessary |

### Serious Incident Reporting

| Timeline | Requirement |
|----------|-------------|
| 2 days | Serious public health threat |
| 10 days | Death or serious deterioration |
| 15 days | Other serious incidents |

---

## EUDAMED and UDI

Implement UDI system per Article 27:

1. Obtain issuing entity code (GS1, HIBCC, ICCBBA) → verify: step output matches expected outcome
2. Assign UDI-DI to each device variant → verify: step output matches expected outcome
3. Assign UDI-PI (production identifier) → verify: step output matches expected outcome
4. Apply UDI carrier to labels (AIDC + HRI) → verify: diff matches intended change
5. Register actor in EUDAMED → verify: step output matches expected outcome
6. Register devices in EUDAMED → verify: step output matches expected outcome
7. Upload certificates when available → verify: file content matches expected shape
8. **Validation:** UDI verified on sample labels → verify: step output matches expected outcome

### EUDAMED Modules

| Module | Content | Actor |
|--------|---------|-------|
| Actor | Company registration | Manufacturer, AR |
| UDI/Device | Device and variant data | Manufacturer |
| Certificates | NB certificates | Notified Body |
| Clinical Investigation | Study registration | Sponsor |
| Vigilance | Incident reports | Manufacturer |
| Market Surveillance | Authority actions | Competent Authority |

### UDI Label Requirements

Required elements per Article 13:

- [ ] UDI-DI (device identifier)
- [ ] UDI-PI (production identifier) for Class II+
- [ ] AIDC format (barcode/RFID)
- [ ] HRI format (human-readable)
- [ ] Manufacturer name and address
- [ ] Lot/serial number
- [ ] Expiration date (if applicable)

---

## Reference Documentation

### MDR Classification Guide

`references/mdr-classification-guide.md` contains:

- Complete Annex VIII classification rules (Rules 1-22)
- Software classification per MDCG 2019-11
- Worked classification examples
- Conformity assessment route selection

### Clinical Evidence Requirements

`references/clinical-evidence-requirements.md` contains:

- Clinical evidence framework and hierarchy
- Literature search methodology
- Clinical Evaluation Report structure
- PMCF plan and evaluation report guidance

### Technical Documentation Templates

`references/technical-documentation-templates.md` contains:

- Annex II and III content requirements
- Design History File structure
- GSPR compliance matrix template
- Declaration of Conformity template
- Notified Body submission checklist

---

## Tools

### MDR Gap Analyzer

```bash
# Quick gap analysis
python scripts/mdr_gap_analyzer.py --device "Device Name" --class IIa

# JSON output for integration
python scripts/mdr_gap_analyzer.py --device "Device Name" --class III --output json

# Interactive assessment
python scripts/mdr_gap_analyzer.py --interactive
```

Analyzes device against MDR requirements, identifies compliance gaps, generates prioritized recommendations.

**Output includes:**
- Requirements checklist by category
- Gap identification with priorities
- Critical gap highlighting
- Compliance roadmap recommendations

---

## Notified Body Interface

### Selection Criteria

| Factor | Considerations |
|--------|----------------|
| Designation scope | Covers your device type |
| Capacity | Timeline for initial audit |
| Geographic reach | Markets you need to access |
| Technical expertise | Experience with your technology |
| Fee structure | Transparency, predictability |

### Pre-Submission Checklist

- [ ] Technical documentation complete
- [ ] GSPR matrix fully addressed
- [ ] Risk management file current
- [ ] Clinical evaluation report complete
- [ ] QMS (ISO 13485) certified
- [ ] Labeling and IFU finalized
- [ ] **Validation:** Internal gap assessment complete

## When NOT to use

- Task is unrelated to mdr 745 specialist — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Mdr 745 Specialist needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for mdr 745 specialist
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving mdr 745 specialist
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
