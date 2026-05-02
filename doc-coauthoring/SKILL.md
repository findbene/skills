---
name: doc-coauthoring
description: "Structured collaborative document creation workflow for co-authoring, review cycles, and iterating with stakeholders. Use this skill any time a document needs to be written collaboratively, a structured writing process needs to be followed, document drafts need stakeholder review, or formal documentation needs to be co-authored. Trigger immediately on: \"co-author\", \"write together\", \"collaborative document\", \"document review\", \"draft review\", \"shared document\", \"writing workflow\", \"document collaboration\", \"review this draft\", \"iterate on document\", \"writing process\", \"document feedback\". If someone says \"let us write this document together\" or \"review and iterate on this draft\" this skill MUST trigger."
---

# Doc Co-Authoring Workflow

Structured workflow for collaborative document creation through three stages.

## Stage 1: Context Gathering

**Goal:** Close the gap between what the user knows and what Claude knows.

### Initial Questions
1. What type of document is this? (technical spec, decision doc, proposal)
2. Who's the primary audience?
3. What's the desired impact when someone reads this?
4. Is there a template or specific format to follow?
5. Any other constraints or context?

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
1. Ask clarifying questions about what to include
2. Brainstorm 5-20 options
3. User indicates what to keep/remove/combine
4. Draft the section
5. Refine through surgical edits

**Quality Checking:** After 3 consecutive iterations with no substantial changes, ask if anything can be removed.

## Stage 3: Reader Testing

**Goal:** Test the document with a fresh perspective to catch blind spots.

### Testing Steps
1. Predict reader questions (5-10 questions)
2. Test with sub-agent or have user test in fresh Claude conversation
3. Run additional checks for ambiguity, assumptions, contradictions
4. Report and fix any issues found

### Exit Condition
Document passes when Reader Claude consistently answers questions correctly and doesn't surface new gaps.

## Final Review
- User does final read-through
- Double-check facts, links, technical details
- Verify it achieves the intended impact
