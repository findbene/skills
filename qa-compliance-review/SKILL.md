---
name: qa-compliance-review
description: >
  Performs structured QA and compliance reviews on code, content, documents,
  workflows, or processes — checking against a defined standard, ruleset, or
  checklist and producing a prioritized findings report. Use this skill whenever
  the user wants a QA review, compliance check, quality gate, pre-launch audit,
  code review against a style guide, content review against brand guidelines, or
  wants to verify something meets a defined standard. Also trigger when they say
  "review this before I ship," "does this meet our standards," or "check this
  against our rules."
---

# QA and Compliance Review

You review artifacts against standards and produce clear, actionable findings.
The goal is a report the responsible person can act on immediately — not a generic
list of "best practices" but specific, evidence-backed findings tied to the actual
artifact under review.

## Before you review

Establish two things:

1. **What is being reviewed?** — Code file, document, workflow, process, content piece, etc.
2. **What standard applies?** — House rules, industry standard, regulatory requirement, or
   a checklist the user provides.

If no explicit standard is provided, infer the appropriate one from context:
- Python code → PEP 8 + project conventions + security best practices
- Marketing content → brand voice + legal/compliance basics (no unsubstantiated claims)
- API design → REST conventions + security headers + error handling
- Process/SOP → clarity, ownership, auditability, edge case coverage

Ask the user to clarify if the standard is ambiguous and high-stakes.

## Review dimensions

Adapt these to the artifact type. Not all dimensions apply to all reviews.

### Correctness
Does it do what it's supposed to do? Are there logic errors, missing cases, or
behaviors that contradict the spec?

### Completeness
Are all required components present? Missing error handling, missing fields,
unimplemented requirements, uncovered edge cases?

### Security / risk
For code: injection vectors, hardcoded secrets, missing auth checks, unvalidated input.
For content: unsubstantiated claims, legal exposure, PII handling.
For processes: who owns the failure case? What happens when X goes wrong?

### Consistency
Does it match established patterns in the codebase / brand / workflow?
Inconsistency isn't always a bug but it creates maintenance debt and confusion.

### Clarity
Would someone unfamiliar with this artifact understand what it does and why?
Relevant for code, docs, and processes alike.

### Performance / efficiency (code only)
Obvious bottlenecks, unnecessary computation, missing indexes, N+1 query patterns.

### Compliance with stated rules
Check explicitly against any rules provided (house style guide, CLAUDE.md, brand
guidelines, regulatory requirements). Cite the rule number or name when flagging.

## Severity levels

Every finding gets a severity:

- **P0 — Blocker**: Must fix before shipping. Security vulnerabilities, data loss risk,
  legal exposure, hard crashes.
- **P1 — Major**: Should fix before shipping. Incorrect behavior, significant style
  violations, missing required fields.
- **P2 — Minor**: Fix in next iteration. Non-critical inconsistencies, minor style issues,
  low-risk edge cases.
- **P3 — Note**: Optional improvement. Readability, future-proofing, nice-to-have.

## Output format

---

## QA / Compliance Review: [Artifact Name]
*Standard applied: [what rules / guidelines were checked against]*
*Date: [today]*

### Summary
One paragraph. Overall verdict (pass / pass with conditions / fail), count of findings
by severity, and the most critical issue to fix first.

### Findings

**[P0] [Short title]**
- **Location**: file:line, section name, or process step
- **Finding**: What exactly is wrong
- **Evidence**: Quote or reference the specific text/code/step
- **Required fix**: What must change (be prescriptive, not vague)

*(repeat for each finding, grouped by severity)*

### Pass checklist
Items reviewed that passed. Brevity is fine here — the user wants to know what's
good, not just what's broken.

| Check | Status | Note |
|-------|--------|------|
| [Criterion] | PASS | [optional note] |

### Recommended next steps
1–3 prioritized actions based on findings. If P0s exist, list those first and
recommend not shipping until resolved.

---

## Special modes

### Bulk review
If the user passes multiple files or items, produce a summary triage table first,
then detailed findings for P0/P1 issues only (P2/P3 in brief at the bottom).

### Diff review
If the user provides a git diff or "what changed" context, focus only on the
changed lines and their immediate dependencies. Don't re-audit the whole codebase.

### Pre-launch checklist
If the user says "we're about to launch," switch to a structured go/no-go checklist
format covering: functionality, security, performance, content accuracy, and rollback plan.
