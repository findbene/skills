---
name: "postmortem"
description: "/em -postmortem — Honest Analysis of What Went Wrong Triggers: 'use postmortem', 'postmortem', 'postmortem task'."
allowed-tools: Glob, Grep, Read
---

# /em:postmortem — Honest Analysis of What Went Wrong

**Command:** `/em:postmortem <event>`

Not blame. Understanding. The failed deal, the missed quarter, the feature that flopped, the hire that didn't work out. What actually happened, why, and what changes as a result.

---

## When NOT to use

- Live, ongoing production incident — use `incident-commander` or `incident-response` first
- Routine retro for a sprint — use `scrum-master` or agile retro skill
- Failure where blame analysis is appropriate (fraud, misconduct) — different process, not a postmortem
- Personal life setback — this is for organizational/business failures
- Compliance breach requiring legal hold — engage legal/IR, not a postmortem doc

## Red Flags

| Rationalization | Reality |
|---|---|
| "Someone should be fired, that solves it" | If the same conditions remain, the next person will fail the same way; investigate the system |
| "12 vague action items is a good outcome" | Vague items never get done; force <5 specific, owned, dated changes |
| "We do not need to talk to the people closest to the failure" | They have the highest-fidelity information; skipping them produces armchair theories |
| "Public postmortem is too risky" | Internal-only postmortems lose 70% of the cross-team learning value; redact and publish |

## Output Contract

Finished output must contain:
- Event defined precisely (what happened, when, scope, who impacted)
- Timeline with key decision points and information available at each
- Contributing factors broken into: process, system, people-conditions (NOT blame)
- Distinguish proximate cause from root causes (5-whys or causal chain)
- <5 specific, owned, dated changes — measurable
- Explicit list of patterns to watch for in future signals
- Plan to share lessons cross-team (presentation, doc, etc.)

## Examples

**Example 1 — Failed enterprise deal**
- Input: "We lost the Acme deal at the final stage after 6 months"
- Action: Reconstruct timeline → interview AE, SE, exec sponsor → identify decision points where signal was missed → distinguish proximate (procurement objection) from root (no exec sponsor coverage in last 4 weeks)
- Output: Postmortem doc with timeline, 3 contributing factors, 4 dated action items (e.g., "exec sponsor cadence audit by 6/30, owner: VP Sales"), 1 cross-team share planned

**Example 2 — Failed product launch**
- Input: "Our new feature launched, got 200 signups, target was 5000"
- Action: Pull metrics → interview PM, eng, marketing → trace decisions (positioning, channel mix, pricing) → identify root (no PMF validation before launch, channel mismatch)
- Output: Postmortem doc with quantified gap, 4 contributing factors, 3 process changes (PMF gate, channel test, pricing research), public blog post outlined for transparency

## References

Extended sections moved to `references/details.md`.
