---
name: ads-create
description: "Campaign concept and copy brief generator for paid advertising. Reads brand-profile.json and optional audit results to produce structured ca. Triggers: 'use ads-create', 'run ads create', 'ads create."
allowed-tools: Glob, Grep, Read, Task
user-invokable: false
---

# Ads Create: Campaign Concept & Copy Brief Generator

Generates structured campaign concepts and platform-specific copy from your brand
profile and optional audit data. Outputs `campaign-brief.md` for use by `/ads generate`.

## Process

### Step 1: Check for Brand Profile

Look for `brand-profile.json` in the current directory.

- **Found**: Load and proceed.
- **Not found**: Ask the user:
  > "I don't see a brand-profile.json in this directory. Would you like to:
  > 1. Run `/ads dna <url>` first to extract brand DNA automatically
  > 2. Describe your brand manually (I'll create a basic profile from your description)"

If the user chooses manual, collect:
- Brand name and website
- Primary color (or "unsure")
- 3 words that describe the brand voice
- Target audience (age, role, key pain point)
- Main product/service offering

### Step 2: Check for Audit Results

Look for `ADS-AUDIT-REPORT.md` or any `*-audit-results.md` in the current directory.

- **Found**: Read them. Note the top 3 weaknesses (creative fatigue, tracking gaps, wasted spend) to address in concepts.
- **Not found**: Continue without. Note in the brief: "No audit data found; concepts are generalized. Run `/ads audit` for weakness-targeted concepts."

### Step 3: Collect Campaign Parameters

If `--platforms` or `--objective` flags were provided in the command, use those values
and skip the corresponding questions below.

Ask (combine into one message; omit any already provided via flags):
1. **Platforms**: Which ad platforms? (Meta · Google · LinkedIn · TikTok · YouTube · Microsoft · All) → verify: step output matches expected outcome
2. **Objective**: Sales/Revenue · Leads/Demos · App Installs · Brand Awareness · Retargeting → verify: package installed + import succeeds
3. **Offer or brief**: Any specific offer, promotion, or message to highlight? (optional) → verify: step output matches expected outcome
4. **Number of concepts**: How many campaign concepts? (default: 3) → verify: step output matches expected outcome

### Step 4: Select Copy Framework

Read `ads/references/copy-frameworks.md` and recommend a framework based on
campaign goal + platform + audience temperature:

| Framework | Best For |
|-----------|----------|
| AIDA (Attention, Interest, Desire, Action) | Cold audiences, awareness campaigns |
| PAS (Problem, Agitate, Solve) | Pain-point products, problem-aware audiences |
| BAB (Before, After, Bridge) | Transformation offers, coaching, fitness |
| 4P (Promise, Picture, Proof, Push) | Direct response, high-intent audiences |
| FAB (Features, Advantages, Benefits) | Product-focused, comparison shoppers |
| Star-Story-Solution | Brand storytelling, warm audiences |

Include the selected framework name in campaign-brief.md for the copy-writer agent.

### Step 5: Spawn Creative Agents in Sequence

Agents must run **sequentially**; `copy-writer` reads the file that `creative-strategist`
writes, so running them in parallel creates a race condition on `campaign-brief.md`.

**Step 5a; Spawn `creative-strategist`** (Task tool):
This agent creates `campaign-brief.md` and writes the strategic sections:
`## Brand DNA Summary`, `## Campaign Concepts`, `## Image Generation Briefs`, `## Next Steps`.

Additional instructions for `creative-strategist`:
- For e-commerce businesses, also read `skills/ads-plan/assets/ecommerce-creative.md`
  and select the appropriate creative playbook (Product Launch, Sale/Promotion,
  Seasonal, Retargeting, Brand Awareness)
- Include banana domain mode recommendations in each Image Generation Brief
  (Product, Editorial, Cinema, UI/Web, or Portrait)

Wait for `creative-strategist` to **fully complete** before continuing.

**Step 5b; Spawn `copy-writer`** (Task tool):
After `creative-strategist` completes, spawn `copy-writer`. It reads the existing
`campaign-brief.md` and appends the `## Copy Deck` section with platform-specific
headlines, primary text, and CTAs.

Additional instructions for `copy-writer`:
- Read `ads/references/copy-frameworks.md` and apply the selected framework
  structure to all ad copy
- Generate 2 framework variants per platform: primary (recommended framework)
  + secondary (alternative for A/B testing)

Wait for `copy-writer` to complete before proceeding to Step 6.

### Step 6: Review and Present

After both agents complete, confirm `campaign-brief.md` exists and is complete.

Present a summary to the user:
```
✓ campaign-brief.md generated

Summary:
  Concepts: [N] campaign concepts created
  Platforms: [list]
  Copy deck: Headlines, primary text, and CTAs for each concept × platform
  Image briefs: [N] image generation briefs ready

Next steps:
  1. Review campaign-brief.md and adjust any messaging → verify: step output matches expected outcome
  2. Run `/ads generate` to produce AI images from the briefs → verify: output file exists + no syntax error
  3. Upload copy and assets to your ad platforms → verify: file readable + content matches expected shape
```

## Next Steps
1. Review all concepts and select which to move forward with → verify: step output matches expected outcome
2. Run `/ads generate` to produce images from the briefs above → verify: output file exists + no syntax error
3. Adjust CTAs and offers in the copy deck for your specific promotion → verify: step output matches expected outcome
4. Upload final assets to your ad platform managers → verify: file readable + content matches expected shape
```

## When NOT to use

- Task is unrelated to ads create — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Ads Create needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for ads create
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving ads create
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

## References

Extended sections moved to `references/details.md`.
