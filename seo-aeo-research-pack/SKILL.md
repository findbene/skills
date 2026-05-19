---
name: seo-aeo-research-pack
description: 'Delivers a full SEO + AEO (Answer Engine Optimization) research pack for a given topic, niche, or URL. Triggers: "use seo-aeo-research-pack", "optimize seo aeo research pack", "seo aeo research pack".'
allowed-tools: Glob, Grep, Read
---

# SEO + AEO Research Pack

You produce research packs that serve both traditional SEO (Google/Bing rankings)
and AEO (being cited by AI answer engines). These are increasingly the same game —
but AEO rewards different things: clear definitions, structured answers, authoritative
entity coverage, and first-person expertise signals.

## Inputs to collect

If not already provided:

- **Topic / niche / URL** — what is the content about or for?
- **Target audience** — who are they, what do they already know?
- **Content goal** — organic traffic, AI citations, affiliate conversions, brand authority?
- **Existing content** — do they already have articles on this? (avoids cannibalization)
- **Competitors** — any sites they consider direct competitors?

A URL or niche name alone is enough to start. Ask only for what you need.

## Research pack structure

Deliver all sections below. Use your knowledge of SEO/AEO best practices — if
web search is available, use it to validate SERPs and check AI-cited sources.

---

## SEO + AEO Research Pack: [Topic]
*Generated: [date]*

### 1. Topic landscape
2–3 sentences on the search landscape for this topic. Is it competitive? Dominated
by big brands? Are there underserved angles? Is AI already answering it directly?

### 2. Keyword clusters

Organize keywords into intent-based clusters, not flat lists. Each cluster maps to
one content piece or page.

| Cluster | Primary keyword | Supporting keywords | Monthly volume (est.) | Intent |
|---------|----------------|---------------------|----------------------|--------|
| ... | ... | ... | ... | Info / Commercial / Navigational |

Aim for 4–8 clusters covering the full topic space. Flag which clusters are
AI-friendly (definitional, how-to, comparison) vs. require fresh data/opinion.

### 3. Search intent map
For the top 3 clusters, describe:
- What the searcher is actually trying to do
- What format Google / AI engines currently surface (list, definition, how-to, etc.)
- What gaps the current top results leave open

### 4. AEO visibility analysis
AEO is about being the source AI engines cite. Check for:

- **Definition gaps** — does this topic lack a clean, citable definition? If so,
  writing the authoritative one is a fast AEO win.
- **Entity coverage** — are there named concepts, tools, people, or frameworks in
  this niche that deserve their own dedicated section?
- **Structured answer opportunities** — questions that AI engines answer directly
  (zero-click) where you can still get attributed if you're the canonical source.
- **Citation triggers** — first-person expertise, original data, unique frameworks,
  and contrarian takes all increase citation likelihood.

### 5. Content action plan

Prioritized list of content pieces to create, ordered by opportunity size.

| Priority | Title / angle | Target cluster | Format | AEO potential | Notes |
|----------|--------------|----------------|--------|---------------|-------|
| P1 | ... | ... | Article / Guide / Comparison | High / Med / Low | ... |

Include at least one quick-win (low competition, high intent) and one authority-builder
(harder to rank, high AEO value).

### 6. On-page and structure recommendations
Specific to this topic:
- Recommended H2/H3 structure for the primary piece
- Schema markup types worth adding (FAQ, HowTo, Article, etc.)
- Internal linking opportunities if they have existing content
- Content freshness signals to include (dates, current examples, live data)

### 7. 30-day action summary
Three to five prioritized actions the user should take this month, with rationale.

---

## Notes on AEO vs. SEO tradeoffs

When the two goals conflict, flag it. Example: a very broad definition article may
get AI-cited but never rank for commercial keywords. Help the user choose the right
goal for each piece.

AI engines favor:
- Clear entity definitions with consistent naming
- Numbered lists and structured sections (easier to quote)
- Original data, frameworks, or named methodologies
- Author expertise signals (bylines, credentials, first-person experience)
- Fast-loading, clean HTML (AI crawlers penalize JS-heavy sites)

## When NOT to use

- Task is unrelated to seo aeo research pack — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Seo Aeo Research Pack needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for seo aeo research pack
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving seo aeo research pack
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
