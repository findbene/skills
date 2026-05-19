---
name: doc-coauthoring
description: 'Structured collaborative document creation workflow for co-authoring, review cycles, and iterating with stakeholders. Triggers: "use doc-coauthoring", "doc coauthoring", "doc task".'
allowed-tools: Glob, Grep, Read
---

# Doc Co-Authoring Workflow

Structured workflow for collaborative document creation through three stages.

## Stage 1: Context Gathering

**Goal:** Close the gap between what the user knows and what Claude knows.

### Initial Questions
1. What type of document is this? (technical spec, decision doc, proposal) → verify: step output matches expected outcome
2. Who's the primary audience? → verify: step output matches expected outcome
3. What's the desired impact when someone reads this? → verify: file content matches expected shape
4. Is there a template or specific format to follow? → verify: step output matches expected outcome
5. Any other constraints or context? → verify: step output matches expected outcome

### Info Dumping
Encourage user to dump all context:
- Background on the project/problem
- Related team discussions or documents
- Why alternative solutions aren't being used
- Organizational context
- Timeline pressures or constraints
- Technical architecture or dependencies

Ask clarifying questions after initial dump. Generate 5-10 numbered questions based on gaps.

## Stage 2: Refinement & Structure

**Goal:** Build the document section by section through brainstorming, curation, and iterative refinement.

For each section:
1. Ask clarifying questions about what to include → verify: user confirms
2. Brainstorm 5-20 options → verify: step output matches expected outcome
3. User indicates what to keep/remove/combine → verify: step output matches expected outcome
4. Draft the section → verify: step output matches expected outcome
5. Refine through surgical edits → verify: diff matches intended change

**Quality Checking:** After 3 consecutive iterations with no substantial changes, ask if anything can be removed.

## Stage 3: Reader Testing

**Goal:** Test the document with a fresh perspective to catch blind spots.

### Testing Steps
1. Predict reader questions (5-10 questions) → verify: file content matches expected shape
2. Test with sub-agent or have user test in fresh Claude conversation → verify: all checks pass
3. Run additional checks for ambiguity, assumptions, contradictions → verify: command exit code 0
4. Report and fix any issues found → verify: diff matches intended change

### Exit Condition
Document passes when Reader Claude consistently answers questions correctly and doesn't surface new gaps.

## Final Review
- User does final read-through
- Double-check facts, links, technical details
- Verify it achieves the intended impact

## When NOT to use

- Task is unrelated to doc coauthoring — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Doc Coauthoring needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for doc coauthoring
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving doc coauthoring
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
