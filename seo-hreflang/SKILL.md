---
name: seo-hreflang
description: >
  Hreflang and international SEO audit, validation, and tag generation. Catches the
  most common implementation mistakes: missing self-referencing tags, missing return
  tags (bidirectional requirement), wrong ISO 639-1 language codes (eng vs en, jp vs ja),
  wrong ISO 3166-1 region codes (en-uk vs en-GB), missing x-default, canonical
  misalignment, and duplicate hreflang conflicts. Supports HTML link tags, HTTP header
  implementation, and XML sitemap hreflang. Generates correct tag sets for any
  language/region combination. Make sure to use this skill whenever the user mentions:
  "hreflang", "international SEO", "multi-language site", "multi-region SEO",
  "i18n SEO", "language tags", "hreflang errors", "wrong country targeting",
  "international targeting", "language selector", "x-default", "en-US vs en-GB",
  "serve different content by country", "translate my site for SEO", or any question
  about targeting different countries or languages in search — even if they don't
  know the word "hreflang".
user-invokable: true
argument-hint: "[url]"
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - WebFetch
---

# Hreflang & International SEO

Validate existing hreflang implementations or generate correct hreflang tags
for multi-language and multi-region sites. Support HTML, HTTP header, and
XML sitemap implementations.

## Validation Checks

### 1. Self-Referencing Tags
- Include an hreflang tag on every page pointing to itself
- Match the self-referencing URL exactly to the page's canonical URL
- Flag missing self-referencing tags as critical — Google ignores the entire hreflang set when absent

### 2. Return Tags
- When page A links to page B with hreflang, page B must link back to page A
- Enforce bidirectionality on every hreflang relationship (A→B and B→A)
- Flag missing return tags — they invalidate the hreflang signal for both pages
- Verify all language versions reference each other (full mesh)

### 3. x-default Tag
- Add an x-default tag to designate the fallback page for unmatched languages/regions
- Point it to the language selector page or English version
- Allow only one x-default per set of alternates
- Add return tags from all other language versions to the x-default target

### 4. Language Code Validation
- Use ISO 639-1 two-letter codes only (e.g., `en`, `fr`, `de`, `ja`)
- Flag these common errors:
  - `eng` instead of `en` (ISO 639-2, not valid for hreflang)
  - `jp` instead of `ja` (incorrect code for Japanese)
  - `zh` without region qualifier (ambiguous; require `zh-Hans` or `zh-Hant`)

### 5. Region Code Validation
- Use ISO 3166-1 Alpha-2 for the optional region qualifier (e.g., `en-US`, `en-GB`, `pt-BR`)
- Enforce format: `language-REGION` (lowercase language, uppercase region)
- Flag these common errors:
  - `en-uk` instead of `en-GB` (UK is not a valid ISO 3166-1 code)
  - `es-LA` (Latin America is not a country; require specific countries)
  - Region code without language prefix

### 6. Canonical URL Alignment
- Place hreflang tags only on canonical URLs
- Flag hreflang on pages where `rel=canonical` points elsewhere — Google ignores it
- Require exact URL match between canonical and hreflang (including trailing slashes)
- Remove non-canonical pages from all hreflang sets

### 7. Protocol Consistency
- Require the same protocol (HTTPS or HTTP) for all URLs in an hreflang set
- Flag mixed HTTP/HTTPS as a validation failure
- After HTTPS migration, verify all hreflang tags are updated to HTTPS

### 8. Cross-Domain Support
- Apply hreflang across different domains (e.g., example.com and example.de) when needed
- Require return tags on both domains for cross-domain hreflang
- Verify both domains are verified in Google Search Console
- Recommend sitemap-based implementation for cross-domain setups

## Common Mistakes

| Issue | Severity | Fix |
|-------|----------|-----|
| Missing self-referencing tag | Critical | Add hreflang pointing to same page URL |
| Missing return tags (A→B but no B→A) | Critical | Add matching return tags on all alternates |
| Missing x-default | High | Add x-default pointing to fallback/selector page |
| Invalid language code (e.g., `eng`) | High | Use ISO 639-1 two-letter codes |
| Invalid region code (e.g., `en-uk`) | High | Use ISO 3166-1 Alpha-2 codes |
| Hreflang on non-canonical URL | High | Move hreflang to canonical URL only |
| HTTP/HTTPS mismatch in URLs | Medium | Standardize all URLs to HTTPS |
| Trailing slash inconsistency | Medium | Match canonical URL format exactly |
| Hreflang in both HTML and sitemap | Low | Choose one method (sitemap preferred for large sites) |
| Language without region when needed | Low | Add region qualifier for geo-targeted content |

## Implementation Methods

### Method 1: HTML Link Tags
Best for: Sites with <50 language/region variants per page.

```html
<link rel="alternate" hreflang="en-US" href="https://example.com/page" />
<link rel="alternate" hreflang="en-GB" href="https://example.co.uk/page" />
<link rel="alternate" hreflang="fr" href="https://example.com/fr/page" />
<link rel="alternate" hreflang="x-default" href="https://example.com/page" />
```

Place in `<head>`. Include all alternates on every page, including the self-referencing tag.

### Method 2: HTTP Headers
Best for: Non-HTML files (PDFs, documents).

```
Link: <https://example.com/page>; rel="alternate"; hreflang="en-US",
      <https://example.com/fr/page>; rel="alternate"; hreflang="fr",
      <https://example.com/page>; rel="alternate"; hreflang="x-default"
```

Set via server configuration or CDN rules.

### Method 3: XML Sitemap (Recommended for large sites)
Best for: Sites with many language variants, cross-domain setups, or 50+ pages.

See Hreflang Sitemap Generation section below.

### Method Comparison
| Method | Best For | Pros | Cons |
|--------|----------|------|------|
| HTML link tags | Small sites (<50 variants) | Easy to implement, visible in source | Bloats `<head>`, hard to maintain at scale |
| HTTP headers | Non-HTML files | Works for PDFs, images | Complex server config, not visible in HTML |
| XML sitemap | Large sites, cross-domain | Scalable, centralized management | Not visible on page, requires sitemap maintenance |

## Hreflang Generation

### Process
1. **Detect languages**: Scan site for language indicators (URL path, subdomain, TLD, HTML lang attribute)
2. **Map page equivalents**: Match corresponding pages across languages/regions
3. **Validate language codes**: Verify all codes against ISO 639-1 and ISO 3166-1
4. **Generate tags**: Create hreflang tags for each page including self-referencing
5. **Verify return tags**: Confirm all relationships are bidirectional
6. **Add x-default**: Set fallback for each page set
7. **Output**: Generate implementation code (HTML, HTTP headers, or sitemap XML)

## Hreflang Sitemap Generation

### Sitemap with Hreflang
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:xhtml="http://www.w3.org/1999/xhtml">
  <url>
    <loc>https://example.com/page</loc>
    <xhtml:link rel="alternate" hreflang="en-US" href="https://example.com/page" />
    <xhtml:link rel="alternate" hreflang="fr" href="https://example.com/fr/page" />
    <xhtml:link rel="alternate" hreflang="de" href="https://example.de/page" />
    <xhtml:link rel="alternate" hreflang="x-default" href="https://example.com/page" />
  </url>
  <url>
    <loc>https://example.com/fr/page</loc>
    <xhtml:link rel="alternate" hreflang="en-US" href="https://example.com/page" />
    <xhtml:link rel="alternate" hreflang="fr" href="https://example.com/fr/page" />
    <xhtml:link rel="alternate" hreflang="de" href="https://example.de/page" />
    <xhtml:link rel="alternate" hreflang="x-default" href="https://example.com/page" />
  </url>
</urlset>
```

Key rules:
- Include the `xmlns:xhtml` namespace declaration
- Include ALL language alternates (including self-reference) in every `<url>` entry
- Add each alternate as a separate `<url>` entry with its own full set
- Split at 50,000 URLs per sitemap file

## Output

### Hreflang Validation Report

#### Summary
- Total pages scanned: XX
- Language variants detected: XX
- Issues found: XX (Critical: X, High: X, Medium: X, Low: X)

#### Validation Results
| Language | URL | Self-Ref | Return Tags | x-default | Status |
|----------|-----|----------|-------------|-----------|--------|
| en-US | https://... | ✅ | ✅ | ✅ | ✅ |
| fr | https://... | ❌ | ⚠️ | ✅ | ❌ |
| de | https://... | ✅ | ❌ | ✅ | ❌ |

### Generated Hreflang Tags
- HTML `<link>` tags (if HTML method chosen)
- HTTP header values (if header method chosen)
- `hreflang-sitemap.xml` (if sitemap method chosen)

### Recommendations
- List missing implementations to add
- List incorrect codes with correct replacements
- Suggest method migration where needed (e.g., HTML to sitemap for scale)

## Error Handling

| Scenario | Action |
|----------|--------|
| URL unreachable (DNS failure, connection refused) | Report the error clearly. Do not guess site structure. Suggest the user verify the URL and try again. |
| No hreflang tags found | Report the absence. Check for other internationalization signals (subdirectories, subdomains, ccTLDs) and recommend the appropriate hreflang implementation method. |
| Invalid language/region codes detected | List each invalid code with the correct replacement. Provide a corrected hreflang tag set ready to implement. |
