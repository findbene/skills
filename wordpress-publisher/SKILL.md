---
name: wordpress-publisher
description: >
  Formats, structures, and prepares content for publishing to WordPress — including
  post metadata, SEO fields, category/tag recommendations, featured image briefs,
  internal linking suggestions, and WP-ready HTML/Gutenberg blocks. Use this skill
  whenever the user wants to publish to WordPress, format content for a WP site,
  create a post draft, optimize a WP article for SEO, or set up a WordPress
  publishing workflow. Also trigger when they mention WP, Gutenberg, Yoast, RankMath,
  or any WordPress plugin, or want to schedule posts.
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

1. Metadata table for all posts (one row per post)
2. Individual post packages on request
3. Optional: CSV export format for WP import via WP All Import or similar

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
