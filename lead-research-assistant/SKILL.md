---
name: lead-research-assistant
description: 'Identifies high-quality leads for your product or service by analyzing your business, searching for target companies, a. Triggers: "use lead-research-assistant", "lead research assistant", "lead task.'
allowed-tools: Glob, Grep, Read
---

# Lead Research Assistant

This skill helps you identify and qualify potential leads for your business by analyzing your product/service, understanding your ideal customer profile, and providing actionable outreach strategies.

## When to Use This Skill

- Finding potential customers or clients for your product/service
- Building a list of companies to reach out to for partnerships
- Identifying target accounts for sales outreach
- Researching companies that match your ideal customer profile
- Preparing for business development activities

## How to Use

### Basic Usage

Simply describe your product/service and what you're looking for:

```
I'm building [product description]. Find me 10 companies in [location/industry] 
that would be good leads for this.
```

### With Your Codebase

For even better results, run this from your product's source code directory:

```
Look at what I'm building in this repository and identify the top 10 companies 
in [location/industry] that would benefit from this product.
```

### Advanced Usage

For more targeted research:

```
My product: [description]
Ideal customer profile:
- Industry: [industry]
- Company size: [size range]
- Location: [location]
- Current pain points: [pain points]
- Technologies they use: [tech stack]

Find me 20 qualified leads with contact strategies for each.
```

## Examples

### Example 1: From Lenny's Newsletter

**User**: "I'm building a tool that masks sensitive data in AI coding assistant queries. Find potential leads."

**Output**: Creates a prioritized list of companies that:
- Use AI coding assistants (Copilot, Cursor, etc.)
- Handle sensitive data (fintech, healthcare, legal)
- Have evidence in their GitHub repos of using coding agents
- May have accidentally exposed sensitive data in code
- Includes LinkedIn URLs of relevant decision-makers

### Example 2: Local Business

**User**: "I run a consulting practice for remote team productivity. Find me 10 companies in the Bay Area that recently went remote."

**Output**: Identifies companies that:
- Recently posted remote job listings
- Announced remote-first policies
- Are hiring distributed teams
- Show signs of remote work challenges
- Provides personalized outreach strategies for each

## When NOT to use

- Task is unrelated to lead research assistant — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Lead Research Assistant needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for lead research assistant
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## References

Extended sections moved to `references/details.md`.
