---
name: roi-case-study-builder
description: 'Builds compelling ROI case studies from client data, outcomes, and conversations — producing a publishable document that q. Triggers: "use roi-case-study-builder", "roi case study builder", "roi task.'
allowed-tools: Glob, Grep, Read
---

# ROI Case Study Builder

You build case studies that convert prospects. The best case studies read like a
compelling short story — problem, hero (your service), transformation, results —
with enough hard numbers to make the ROI undeniable.

## What you need

Collect as much of this as you can before writing. Missing data can be estimated
(labeled clearly) but real numbers are always stronger.

- **Client name / type** — company name (or anonymized descriptor like "a 12-person law firm")
- **Industry and size** — sector, headcount or revenue range
- **The situation before** — what was the problem? How bad was it? Any metrics?
- **What you did** — the solution, engagement type, timeline
- **The results** — the outcomes. Even qualitative results are useful.
- **Hard numbers (if available)**: time saved, revenue generated, cost reduced, errors
  eliminated, conversion rate change, headcount avoided, etc.
- **Client quote** — a real quote is gold. If none, note that you need one.
- **Publish format** — web page, PDF, social post, sales deck slide?

One paragraph of rough notes is enough to start. You'll refine with the user.

## ROI calculation

Before writing, do the math explicitly so the case study's numbers are defensible.

**Time savings ROI:**
```
Hours saved per week × hourly cost of that role × 52 weeks = annual value
ROI % = (annual value - cost of solution) / cost of solution × 100
```

**Revenue impact ROI:**
```
Revenue increase attributable to solution × attribution factor = attributed revenue
ROI % = attributed revenue / cost of solution × 100
```

**Cost avoidance ROI:**
```
Cost of the old way (or cost of a headcount they didn't hire) - cost of solution = net savings
```

Show the calculation. Prospects trust numbers they can follow.

If no hard numbers are available, use conservative estimates based on industry
benchmarks — and label them clearly as estimates. Fabricated precision destroys
credibility faster than admitting you're estimating.

## Case study structure

### Format A — Short form (1-pager, website, social)

---

**[Client type] cut [metric] by [X%] in [timeframe]**

**The challenge**
2–3 sentences. Describe the pain point in the client's language. Make it relatable
to the target prospect, not just this specific client.

**The solution**
2–3 sentences. What did you build or deliver? Focus on the outcome it enabled, not
the technical details (those go in an appendix if needed).

**The results**
3–5 bullet points with numbers:
- X hours saved per week (→ $Y in annual labor cost)
- X% improvement in [metric]
- ROI: [X]% in [timeframe]

**In their words**
> "[Client quote — ideally one punchy sentence that captures the transformation]"
> — [Name, Title, Company]

**[CTA: Learn how we can do this for your business → link or contact]**

---

### Format B — Long form (PDF, blog, sales deck)

---

**[Client Name/Type] Case Study**
*Industry: [X] | Company size: [X] | Engagement type: [X] | Timeline: [X]*

#### The situation
Paint the picture before you arrived. What was the client doing? What wasn't working?
What was the business impact of the problem? (Time, money, morale, risk)
Aim for 1–2 paragraphs. Use the client's perspective, not yours.

#### The challenge in numbers
If pre-engagement metrics exist:
- [Metric 1]: [value before]
- [Metric 2]: [value before]

#### Our approach
What did you do and why? Keep it outcome-focused, not feature-focused.
A brief process description (3–5 steps or phases) helps prospects understand
what working with you looks like.

#### Implementation
How long did it take? What did the client need to provide? What did you own?
This section reduces "implementation anxiety" — a common objection.

#### Results

**Quantitative outcomes:**
| Metric | Before | After | Change |
|--------|--------|-------|--------|
| [X] | [X] | [X] | [+X% / -X%] |

**ROI summary:**
- Investment: $[X] (or [X hours] of client time)
- Annual value delivered: $[X]
- ROI: [X]% in [X months]
- Payback period: [X weeks/months]

**Qualitative outcomes:**
2–3 bullet points on non-numeric improvements (team morale, confidence, scalability,
competitive advantage).

#### Client perspective
> "[Quote — 2–4 sentences from the client]"
> — [Full name, Title, Company]

#### What's next
Optional: what are they doing now? (signals ongoing relationship and scalability)

---

## Publish format guidance

**Web page**: Short form works best. One strong headline stat in the hero section.
Full story in expandable sections. Strong CTA at bottom.

**PDF / downloadable**: Long form. Add your branding, logo, and contact info.
Include a summary box on page 1 with the headline metrics — many readers skim.

**Social post**: Extract the 1–2 most striking numbers. Frame as transformation:
"[Client type] went from X to Y in Z weeks using [your service]."

**Sales deck slide**: One slide: client type, problem, solution, 3 key results,
client quote. No paragraphs — bullets and numbers only.

## When you don't have hard numbers

Don't make them up. Instead:
- Use time-based framing: "Processes that took 4 hours now take 20 minutes"
- Use frequency framing: "Eliminated 15 manual steps per week"
- Use confidence framing: "Went from guessing to knowing" + client quote
- Flag to the user: "I'd recommend getting [specific metric] from the client before
  publishing — it will significantly strengthen the ROI claim"

## When NOT to use

- Task is unrelated to roi case study builder — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Roi Case Study Builder needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for roi case study builder
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving roi case study builder
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
