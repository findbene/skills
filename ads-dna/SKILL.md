---
name: ads-dna
description: "Brand DNA extractor for paid advertising. Scans a website URL to extract visual identity, tone of voice, color palette, typography, and imagery style. Triggers: 'use ads-dna', 'run ads dna', 'ads dna."
allowed-tools: Bash, Glob, Grep, Read, WebFetch
user-invokable: false
---

# Ads DNA: Brand DNA Extractor

Extracts brand identity from a website and saves it as `brand-profile.json`
for use by `/ads create`, `/ads generate`, and `/ads photoshoot`.

## Process

### Step 1: Collect URL

If the user hasn't provided a URL, ask:
> "What website URL should I analyze for brand DNA? (e.g. https://yoursite.com)"

### Step 2: Fetch Pages

Use the **WebFetch tool** to retrieve each page. For each URL, use this fetch prompt:
> "Return all visible text content, the full contents of any `<style>` blocks, inline
> `style=` attributes, `<meta>` tags, Google Fonts `@import` URLs, and any `og:image`
> values found on this page."

Fetch in this order:
1. **Homepage** (`<url>`) → verify: step output matches expected outcome
2. **About page**: try `<url>/about`, then `<url>/about-us`, then `<url>/our-story` → verify: step output matches expected outcome
3. **Product/Services page**: try `<url>/product`, then `<url>/products`, then `<url>/services` → verify: step output matches expected outcome

**If `--quick` flag was provided**: fetch the homepage only; skip steps 2 and 3.

If a secondary page returns a 404 or redirect error, continue with fewer pages and note:
"Secondary pages unavailable; extraction based on homepage only. Confidence may be lower."

### Step 2b: Capture Brand Screenshots

After fetching pages, capture 3 screenshots for comprehensive brand anchoring.
These serve as visual style references during `/ads generate`; the same approach
Pomelli uses to anchor ad images to the actual brand aesthetic.

Capture the following:

1. **Homepage hero section** (above the fold): → verify: step output matches expected outcome
```bash
python ~/.claude/skills/ads/scripts/capture_screenshot.py [url]
```
Saves: `./brand-screenshots/{domain}_homepage.png`

2. **Product or services page**: → verify: step output matches expected outcome
```bash
python ~/.claude/skills/ads/scripts/capture_screenshot.py [url]/products
```
Saves: `./brand-screenshots/{domain}_product.png`

3. **About page** (brand personality): → verify: step output matches expected outcome
```bash
python ~/.claude/skills/ads/scripts/capture_screenshot.py [url]/about
```
Saves: `./brand-screenshots/{domain}_about.png`

If a page is not found or returns an error, skip it gracefully and continue
with the remaining pages.

**If `--quick` flag was provided**: skip screenshot capture entirely.

**If capture fails** (Playwright not installed, network error, JS-heavy SPA that times out):
- Log: `"Screenshot capture skipped; run: python3 -m playwright install chromium"`
- Continue without screenshots
- Do NOT set the `screenshots` field in brand-profile.json

### Step 3: Extract Brand Elements

From the fetched HTML, extract:

**Colors:**
- `og:image` meta tag → analyze dominant colors (note 2-3 prominent hex values)
- CSS `background-color` on `body`, `header`, `.hero`, `.btn-primary`
- CSS `color` on `h1`, `h2`, `.btn`
- CSS `border-color` or `background` on `.cta`, `.button`
- Identify: primary (most prominent brand color), secondary (supporting colors), background, text

**Typography:**
- `@import url(https://fonts.googleapis.com/...)` → extract font names from URL path
- CSS `font-family` on `h1`, `h2`, `body`, `.headline`
- If Google Fonts URL contains `family=Inter:wght@...`, heading_font = "Inter"

**Voice:**
Analyze hero headline, subheadline, About page intro, and CTA button text.
Score each axis 1-10 using these heuristics:

| Signal | Score direction |
|--------|----------------|
| Uses "you/your" frequently | formal_casual → casual (+2) |
| Uses technical jargon | expert_accessible → expert (-2) |
| Short punchy sentences (≤8 words) | bold_subtle → bold (+2) |
| Data/stats in hero | rational_emotional → rational (-2) |
| "Transform", "revolutionize", "disrupt" | traditional_innovative → innovative (+2) |
| Customer testimonials lead | rational_emotional → emotional (+2) |
| Industry awards, "trusted by X" | traditional_innovative → traditional (-1) |

### Confidence Scoring
Each voice axis gets a confidence rating based on signal count:
- **High** (3+ signals): strong evidence for axis position
- **Medium** (2 signals): moderate evidence, may need validation
- **Low** (1 signal): weak evidence, treat as estimate

Also extract structured data when available: schema.org markup, Open Graph tags (og:title, og:description, og:image), Twitter Card metadata.

**Imagery style** (from og:image and any visible hero image descriptions):
- Photography vs. illustration vs. flat design
- Subject matter (people, product, abstract, data)
- Composition style (clean/minimal vs. busy/editorial)

**Forbidden elements** (infer from brand positioning):
- Enterprise/B2B brands → add "cheesy stock photos", "consumer lifestyle imagery"
- Healthcare → add "unqualified medical claims", "before/after imagery"
- Finance → add "get rich quick imagery", "unrealistic wealth displays"
- Consumer brands → usually no forbidden elements

### Step 4: Build brand-profile.json

Read `~/.claude/skills/ads/references/brand-dna-template.md` for the exact schema.

Construct the JSON object following the schema precisely. Use `null` for any
field that cannot be confidently extracted; do not guess.

Example of a low-confidence field:
```json
"typography": {
  "heading_font": null,
  "body_font": "system-ui",
  "pairing_descriptor": "system default (Google Fonts not detected)"
}
```

### Step 5: Write brand-profile.json

Write the JSON to `./brand-profile.json` in the current working directory
(where the user is running Claude Code).

If screenshots were captured successfully in Step 2b, include a `screenshots` field:
```json
"screenshots": {
  "homepage": "./brand-screenshots/{domain}_homepage.png",
  "product": "./brand-screenshots/{domain}_product.png",
  "about": "./brand-screenshots/{domain}_about.png"
}
```
Include only the screenshots that were successfully captured. If a page was not
found or errored, omit that key. Omit the `screenshots` field entirely if Step 2b
was skipped or all captures failed.

### Step 6: Confirm and Summarize

Show the user:
```
✓ brand-profile.json saved to ./brand-profile.json

Brand DNA Summary:
  Brand: [brand_name]
  Voice: [descriptor 1], [descriptor 2], [descriptor 3]
  Primary Color: [hex]
  Typography: [heading_font] / [body_font]
  Target: [age_range] [profession]
  Screenshots: [N captured (homepage, product, about) in ./brand-screenshots/] OR [skipped]

Run `/ads create` to generate campaign concepts from this profile.
```

## When NOT to use

- Generic brand strategy work (positioning, messaging architecture) — use `brand` or `marketing-strategy-pmm`
- Brand guideline documentation creation — use `brand-guidelines`
- Competitive ad teardown — use `competitive-ads-extractor` or `competitor-alternatives`
- When `brand-profile.json` already exists and is current — skip re-extraction unless --refresh
- Site is gated/JS-heavy with no public DOM — WebFetch returns nothing useful

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll infer colors from the logo PNG" | Pull from CSS/inline styles; PNG sampling drifts |
| "Skip screenshots, JSON is enough" | Visual anchors materially improve `/ads generate` output |
| "Make up a font if none detected" | Set `heading_font: null` and document; invented values poison downstream prompts |
| "Mark everything `forbidden: []`" | Without forbidden imagery, generated creative wanders off-brand |

## Output Contract

Done when:
- `brand-profile.json` saved with full schema: colors, typography, imagery, aesthetic, brand_values, target_audience
- 3 screenshots saved under `./brand-screenshots/{domain}_{page}.png` (homepage, product, about) unless --quick
- Unknown fields use `null` or empty arrays — never invented values
- Confidence note included if homepage-only or pages 404
- WebFetch ran against homepage + about + product pages (or homepage only with --quick)
- File is consumable by `/ads create`, `/ads generate`, `/ads photoshoot`

## Examples

### Example 1 — Full extraction
- Input: "/ads dna https://acme.com"
- Action: Fetch homepage + /about + /products, capture 3 screenshots, parse CSS/styles/og:image, extract palette, typography, mood, target audience, write JSON
- Output: `./brand-profile.json` + 3 PNGs in `./brand-screenshots/`; downstream skills can run

### Example 2 — Quick mode for landing-page-only brand
- Input: "/ads dna https://startup.com --quick"
- Action: Homepage only, no screenshots, mark `confidence: "homepage-only"` in JSON
- Output: `./brand-profile.json` flagged low-confidence; user warned to refresh after secondary pages exist

## References

Extended sections moved to `references/details.md`.
