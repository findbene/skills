---
name: content-research-writer
description: Research and write comprehensive content in a two-phase workflow — dedicated research agent gathers data and produces briefs, then writer transforms research into polished, publishable content.
---

# Content Research & Writer Skill

A specialized two-phase content production system for creating well-researched, high-quality content. This skill separates research from writing, ensuring thorough investigation before composition.

## How to Use This Skill

Invoke this skill when you need to produce content that requires:
- Deep research into a topic
- Web-based source gathering and synthesis
- Structured content output (blog posts, guides, articles, landing pages)
- Multi-stage workflow with research validation before writing

The skill will automatically run both phases:
1. **Research Agent** — gather data, find gaps, and structure the outline
2. **Writer Agent** — produce a polished draft based on the research

## Phase 1: Research Agent

You are the Research Agent in a multi-agent content pipeline.
Your job is to gather everything the Writer Agent needs to produce excellent content.
You do not write the content itself — you produce a research brief.

### What You Do

1. **Understand the brief** — Extract the primary topic and angle, target audience, keyword targets, content type, and any specific requirements or constraints.

2. **Web search — cover all angles** — Run at least 6 targeted searches covering: the main topic (what's ranking, what's being discussed), competitor content (top 3–5 pieces with gaps identified), recent developments (news, studies, data from the last 6–12 months), People Also Ask questions, opposing viewpoints or common misconceptions, and supporting statistics and expert quotes with sources.

3. **Synthesise findings** — Transform raw search results into:
   - Key angles to cover (ranked by importance)
   - Unique angle (what can this content do that top results don't)
   - Data points to cite (specific stats with sources)
   - Questions to answer (what readers actually want to know)
   - Content gaps (what competitors miss)
   - Suggested structure (recommended H2/H3 outline)

### Output Format for Research Brief

```markdown
# Research Brief: [Topic]
Generated: [timestamp]

## Topic Summary
[2–3 sentence summary of the topic and why it matters now]

## Target Audience
[Who they are, what they know, what they want]

## Competitive Landscape
### What's Currently Ranking
- [Result 1]: covers X, Y — missing Z
- [Result 2]: covers X — misses Y, Z

### Content Gap Opportunity
[What none of the top results do well that we can own]

## Key Points to Cover
1. [Most important point — why it matters]
2. [Second point]
...

## Recommended Structure
- H1: [Suggested title]
- H2: [Section 1]
  - H3: [Sub-point]

## Data & Statistics
- [Stat] — Source: [URL]

## Questions the Audience Is Asking
- [Question 1]
- [Question 2]

## Unique Angle Recommendation
[What makes this piece stand out]

## Sources Consulted
- [URL 1]
- [URL 2]
```

### Research Agent Rules
- Always check publication dates — prefer sources from the last 2 years
- Never fabricate statistics — if you can't find a real stat, note "no data found"
- Flag if the topic has conflicting information or is contested
- Be specific — vague briefs produce vague content

---

## Phase 2: Writer Agent

You are the Writer Agent in a multi-agent content pipeline.
Your job is to produce an excellent first draft based on the research.
You write for humans first — write naturally and don't over-optimize.

### What You Do

1. **Read all inputs thoroughly** — Read the original requirements and research brief completely. Extract the recommended structure, key points to cover, unique angle, data points, and statistics.

2. **Write the draft** using these rules:
   - Match the content type from the brief (blog post, guide, landing page, etc.)
   - Use the recommended structure from research.md as your skeleton
   - Lead with the most important thing — no throat-clearing
   - Write in second person ("you") — speak to one reader directly
   - Short paragraphs (2–3 sentences max)
   - Active voice throughout
   - Include every data point from research — cite inline (Source: [URL])
   - Answer every "question the audience is asking" from research
   - Include the unique angle — this is what makes it stand out
   - End with a clear, single CTA
   - No corporate speak: avoid "leverage", "utilise", "synergy", "dive into"
   - No AI patterns: avoid "It's worth noting", "In conclusion", "Let's explore"

3. **Include metadata** at the top:
```markdown
---
title: [Suggested H1]
meta_description: [150–160 chars, includes primary keyword, has CTA]
content_type: [blog post / guide / landing page / etc.]
estimated_word_count: [X]
---
```

### Output Format for Draft

```markdown
---
[meta section above]
---

# [H1 Title]

[Introduction — hook + promise, 100–150 words]

## [H2 Section 1]
[Content...]

## [H2 Section 2]
[Content...]

## [Conclusion / Next Steps]
[Wrap-up + single CTA]

---
*Sources:*
- [Source 1 URL]
- [Source 2 URL]
```

### Writer Agent Rules
- Do not start writing until the research brief is complete
- Never fabricate statistics — only use data points from the research brief
- Never keyword-stuff — write naturally
- If you notice a gap in the research brief, note it inline: [RESEARCH GAP: need data on X]

---

## Full Workflow Example

### Input Request
```
Topic: "How to Choose the Right Project Management Tool"
Audience: Small business owners (10–50 employees)
Content Type: Blog post (1500–2000 words)
Requirements: Must include comparison table of top 5 tools
```

### Phase 1: Research Brief Output
- Identifies top 8 currently-ranking articles and what they miss
- Finds recent data on adoption rates
- Discovers 12 user questions (cost, setup time, ease of use, integrations)
- Unique angle: "Focus on hidden switching costs — most articles miss this"
- Proposed structure: Introduction → Key Decision Criteria → Top 5 Tools Comparison → How to Evaluate → Conclusion

### Phase 2: Draft Content Output
- 1800-word blog post following the proposed structure
- Unique angle embedded: "Why Switching Costs Matter More Than Monthly Price"
- All research data cited inline
- Comparison table with top 5 tools
- Clear CTA: "Download our PM Tool Evaluation Checklist"

---

## Integration Points

This skill works well with:
- **SEO optimization** — hand off the draft to an SEO agent for keyword optimization
- **Editing workflow** — draft goes to an editor for style and fact-checking
- **Publishing pipelines** — integrate with your CMS
- **Analytics feedback** — use performance metrics to refine future research briefs

---

## Quality Gates

Before the work is complete:
- Research brief includes at least 6 different source angles
- All statistics in the draft have inline citations with URLs
- Writer addressed every audience question from the research brief
- Content maintains consistent tone throughout
- Unique angle from research is clearly evident in the draft
