# Extended Details

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/ads create` | Full campaign brief → `campaign-brief.md` |
| `/ads create --platforms meta google` | Brief for specific platforms only |
| `/ads create --objective leads` | Brief optimized for lead generation |

## campaign-brief.md Format Specification

The following section headings are a **parsing contract**; agents downstream depend on these exact heading names.

```markdown
# Campaign Brief: [brand_name]
**Generated:** [date]
**Website:** [website_url]
**Platforms:** [comma-separated list]
**Objective:** [objective]
**Concepts:** [N]

## Brand DNA Summary
[3-sentence synthesis of brand-profile.json: voice, visual identity, target audience]

## Audit Context
[If audit data found: top 3 weaknesses being addressed]
[If no audit data: "No audit data; run /ads audit for weakness-targeted concepts"]

## Campaign Concepts

### Concept 1: [Name]
**Hypothesis:** [why this will work; 1 sentence]
**Primary Message:** [core message; 1 sentence]
**Tone:** [voice reading from brand-profile.json]
**Visual Direction:** [2-3 sentences describing imagery]
**Target Platforms:** [platforms and rationale]
**CTA:** [call to action text]
**Addresses:** [audit finding or "general brand awareness"]

### Concept 2: [Name]
[same structure]

[repeat for all concepts]

## Copy Deck
[appended by copy-writer agent; headlines, primary text, CTAs per concept per platform]

## Image Generation Briefs

### Brief 1: [Concept Name]: [Platform]
**Prompt:** [exact generation prompt]
**Dimensions:** [WxH]
**Safe zone notes:** [constraint or "None"]

### Brief 2: [Concept Name]: [Platform]
**Prompt:** [exact generation prompt]
**Dimensions:** [WxH]
**Safe zone notes:** [constraint or "None"]

[one brief per concept × platform combination]

## Quality Gates

- **Minimum 3 concepts** (unless user requests fewer)
- **Distinct angles**: no two concepts share the same primary message angle
- **Platform fit**: concepts targeting TikTok must acknowledge vertical-only format and sound-on context
- **Offer anchoring**: if the user provided a specific offer, at least 1 concept must lead with it
- **Image briefs**: every concept must have at least one image brief per requested platform

## Meta Andromeda Optimization

For Meta campaigns: recommend diverse creative concepts (different motivators, visual styles, messaging angles) to maximize Andromeda retrieval. Similarity Score >60% between ads triggers clustering and suppression.

## Platform Character Limits

Verify platform character limits are current. Meta reduced headline display length on mobile in 2025.
