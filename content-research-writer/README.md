# Content Research & Writer Skill

A specialized two-phase content production system for creating well-researched, high-quality content.

## Installation

This skill was installed to `~/.claude/skills/content-research-writer/`

## Source

- **Primary Repository:** https://github.com/arturseo-geo/content-pipeline-skill
- **License:** MIT
- **Authors:** TheGEOLab.net

This skill is derived from the content-pipeline-skill, which includes seven specialist agents for a complete content production pipeline. This version isolates and combines the Research Agent and Writer Agent into a dedicated two-phase workflow focused on researching and writing content.

## What's Included

- `SKILL.md` — Complete skill documentation with usage instructions and agent roles
- `README.md` — This file

## How to Use

Load this skill in Claude Code and invoke it for content that requires:
- Deep research into a topic
- Web-based source gathering and synthesis
- Structured content output (blog posts, guides, articles, landing pages)
- Multi-stage workflow with research validation before writing

### Example Invocation

```
I need a blog post on "AI trends in 2025". Target audience: enterprise IT managers. 
About 2000 words, include recent data from 2024-2025.
```

The skill will:
1. Research the topic, find the top ranking articles, identify gaps, and produce a structured research brief
2. Write a polished first draft based on the research brief with all sources cited

## The Two-Phase Workflow

### Phase 1: Research Agent
- Conducts comprehensive web search (6+ angles)
- Synthesizes competitor content analysis
- Identifies content gaps and unique angles
- Produces a structured research brief with recommended outline, data points, and audience questions

### Phase 2: Writer Agent
- Reads the research brief thoroughly
- Writes a natural, human-first draft
- Incorporates all research data with inline citations
- Answers all audience questions
- Highlights the unique angle
- Includes meta information and clear CTA

## Integration with Other Skills

This skill pairs well with:
- **SEO agents** — for post-draft keyword optimization
- **Editor agents** — for style and fact-checking refinement
- **Publishing workflow agents** — for CMS integration
- **Analytics agents** — for performance feedback and future research refinement

## Original Repository

For the complete 6-agent production pipeline (including SEO optimization, editing, analytics, and quality gates), see:
https://github.com/arturseo-geo/content-pipeline-skill

## License

MIT License (inherited from source repository)
