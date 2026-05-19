---
name: wordpress-publisher
description: 'Formats, structures, and prepares content for publishing to WordPress — including post metadata, SEO fields, category/tag. Triggers: "use wordpress-publisher", "wordpress publisher", "wordpress task".'
allowed-tools: Glob, Grep, Read
---

# WordPress Publisher

You prepare content for WordPress publication — taking raw articles, briefs, or
drafts and turning them into fully formatted, SEO-configured, ready-to-publish posts.

## What you need

If not already provided:

- **Content** — the article, draft, or brief to format
- **Site type** — blog, affiliate site, business site, news site? (affects defaults)
- **SEO plugin** — Yoast, RankMath, or neither? (affects SEO field format)
- **Theme / builder** — Classic editor, Gutenberg, Elementor, Divi? (affects markup output)
- **Categories and tags** — existing taxonomy if known, or ask for the niche
- **Post status** — publish immediately, schedule, or save as draft?

Content is the only hard requirement. Infer everything else from context.

## Output package

For every post, produce the following:

---

## WordPress Post: [Title]

### Post metadata

| Field | Value |
|-------|-------|
| Post title | [Optimized title, max 60 chars for SEO] |
| Slug | [url-friendly-lowercase-with-hyphens] |
| Status | Draft / Scheduled / Publish |
| Publish date | [if scheduling: YYYY-MM-DD HH:MM] |
| Author | [if known] |
| Category | [Primary category] |
| Tags | [tag1, tag2, tag3 — max 5, specific over generic] |
| Featured image | [Brief for art direction — see below] |

### SEO fields (Yoast / RankMath)

| Field | Value |
|-------|-------|
| SEO title | [60 chars max — include primary keyword near front] |
| Meta description | [150–160 chars — complete sentence, keyword, value prop] |
| Focus keyword | [primary keyword] |
| Canonical URL | [if different from default, else leave blank] |
| Schema type | Article / BlogPosting / Review / HowTo / FAQPage |
| Open Graph title | [if different from SEO title] |
| Open Graph description | [1–2 sentences for social share preview] |

### Featured image brief

Describe the featured image so a designer or AI image tool can create it:
- **Dimensions**: 1200×628px (standard OG) or 1200×675px (16:9)
- **Subject**: [what should be in the image]
- **Style**: [photo / illustration / graphic / screenshot]
- **Text overlay**: [if any — exact words, placement]
- **Alt text**: [descriptive alt text for accessibility and SEO]

### Internal linking suggestions

If site context is known, suggest 2–4 internal links to add within the content:
- [Anchor text] → [suggested target page / URL]

### Formatted post content

Produce the post content in the appropriate format:

**Gutenberg (Block editor):** Use comment delimiters for blocks where helpful, but
clean HTML is fine — WP will auto-convert on paste. Use `##` for H2, `###` for H3.
No inline CSS.

**Classic editor:** Clean HTML with standard tags (`<h2>`, `<p>`, `<ul>`, `<strong>`).
No unnecessary `<span>` wrapping or inline styles.

**Elementor / Divi:** Note that complex page builders need to be configured in-UI.
Provide the content as structured sections with clear headings so it maps to the
builder's row/column layout.

Structure:
```
[Introduction — 1-2 paragraphs, primary keyword in first 100 words]

[H2: Main section 1]
[content]

[H2: Main section 2]
[content]

... (continue sections)

[H2: FAQ — if applicable, use H3 for each question]

[H2: Conclusion]
[content + CTA]
```

### Publishing checklist

Before hitting publish, confirm:
- [ ] Focus keyword appears in title, first paragraph, at least one H2, and meta description
- [ ] Featured image has alt text
- [ ] At least one internal link added
- [ ] External links open in new tab (`target="_blank"`) and have `rel="noopener"`
- [ ] Affiliate links have `rel="nofollow sponsored"`
- [ ] Post preview looks correct on mobile (Gutenberg preview or theme preview)
- [ ] Scheduling set if not publishing immediately

---

## Bulk publishing workflow

If the user has multiple posts to prepare, offer to produce a batch:

1. Metadata table for all posts (one row per post) → verify: step output matches expected outcome
2. Individual post packages on request → verify: step output matches expected outcome
3. Optional: CSV export format for WP import via WP All Import or similar → verify: step output matches expected outcome

## WP REST API / automation

If the user is publishing programmatically (via the WP REST API, n8n, or a script),
provide the post payload as JSON:

```json
{
  "title": "Post Title",
  "content": "<p>Post content HTML...</p>",
  "status": "publish",
  "slug": "post-slug",
  "categories": [1],
  "tags": [2, 3],
  "meta": {
    "_yoast_wpseo_metadesc": "Meta description here",
    "_yoast_wpseo_focuskw": "focus keyword"
  }
}
```

Adapt to RankMath meta keys if the site uses RankMath (`rank_math_focus_keyword`, etc.).

## When NOT to use

- Drafting raw content (no WP target) — use `content-research-writer` or `copywriting`
- Headless CMS (Sanity, Contentful, Strapi) — use the relevant CMS skill
- Email newsletter formatting — use `email-template-builder`
- Static-site generator (Hugo, Astro, Next.js) — use the matching skill
- Programmatic SEO at scale (1000+ pages) — use `programmatic-seo` / `seo-programmatic`

## Red Flags

| Rationalization | Reality |
|---|---|
| "Skip alt text on featured image" | Required for a11y and image SEO; never publish without alt |
| "Use generic categories like 'Misc'" | Hurts taxonomy; use specific existing categories or propose new ones |
| "Skip schema markup, Yoast handles it" | Yoast handles basics but Article, FAQPage, HowTo benefit from explicit JSON-LD |
| "Single H1, then all H2s" | Use H2/H3/H4 hierarchy; flat H2 list hurts comprehension and SEO |

## Output Contract

Finished output must contain:
- Title (SEO-optimized, ≤60 chars)
- Slug (kebab-case, keyword-aligned)
- Meta description (≤155 chars, with primary keyword)
- Categories (1-2) and tags (3-6) recommended
- Featured image brief (subject, aspect ratio, alt text)
- 3-7 internal-link suggestions to existing posts on the site
- Gutenberg blocks (or classic HTML) — matching the site's editor
- SEO field block formatted for Yoast or RankMath as specified
- Schema markup JSON-LD when type warrants it (Article default)

## Examples

**Example 1 — Publish a how-to post on a Yoast blog**
- Input: Draft "How to set up Stripe Tax for SaaS" + WP site uses Gutenberg + Yoast
- Action: Optimize title for keyword → write meta description → propose featured image brief → produce Gutenberg blocks (headings, paragraphs, code blocks, callouts) → add HowTo schema → suggest 5 internal links
- Output: Complete post package: title, slug, meta, blocks, Yoast fields, schema JSON-LD, image brief, link list

**Example 2 — Affiliate roundup post**
- Input: "Best CRM tools 2026" affiliate article
- Action: Optimize title → ItemList schema → table of comparison → affiliate disclosure block → 6 internal links to related comparisons
- Output: Gutenberg blocks with rel=sponsored on affiliate links, ItemList schema, disclosure paragraph, complete WP-ready package
