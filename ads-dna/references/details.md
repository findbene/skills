# Extended Details

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/ads dna <url>` | Full brand extraction → `brand-profile.json` |
| `/ads dna https://acme.com --quick` | Fast extraction (homepage only) |

## Visual Designer Integration

The visual-designer agent uses the most relevant screenshot per concept as a style
reference when generating images via banana. For example, a product-focused concept
references the product page screenshot, while a brand awareness concept references
the homepage or about page screenshot.

## Limitations

- **Sparse content**: Sites with <200 words of body text produce lower-confidence profiles.
  Note: "Low confidence extraction; limited content available for analysis."
- **Dynamic sites**: JavaScript-rendered content may not be captured. Playwright is not
  used by default. If the site appears to be SPA/React with no static HTML, note this.
- **Multi-brand enterprises**: This tool creates one profile per URL. Run separately
  for each brand/product line.
- **Dark mode sites**: If body background is #333 or darker, swap background/text values.
- **CSS-in-JS**: Modern React sites may not have extractable CSS. Use og:image colors as fallback.

## brand-profile.json Schema

```json
{
  "schema_version": "1.0",
  "brand_name": "string",
  "website_url": "string",
  "extracted_at": "ISO-8601",
  "voice": {
    "formal_casual": 1-10,
    "rational_emotional": 1-10,
    "playful_serious": 1-10,
    "bold_subtle": 1-10,
    "traditional_innovative": 1-10,
    "expert_accessible": 1-10,
    "descriptors": ["adjective1", "adjective2", "adjective3"]
  },
  "colors": {
    "primary": "#hexcode or null",
    "secondary": ["#hex1", "#hex2"],
    "forbidden": ["#hex or color name"],
    "background": "#hexcode",
    "text": "#hexcode"
  },
  "typography": {
    "heading_font": "Font Name or null",
    "body_font": "Font Name or system-ui",
    "pairing_descriptor": "brief description"
  },
  "imagery": {
    "style": "professional photography | illustration | flat design | mixed",
    "subjects": ["subject1", "subject2"],
    "composition": "brief description",
    "forbidden": ["element1", "element2"]
  },
  "aesthetic": {
    "mood_keywords": ["keyword1", "keyword2", "keyword3"],
    "texture": "minimal | textured | mixed",
    "negative_space": "generous | moderate | dense"
  },
  "brand_values": ["value1", "value2", "value3"],
  "target_audience": {
    "age_range": "e.g. 25-45",
    "profession": "brief description",
    "pain_points": ["pain1", "pain2"],
    "aspirations": ["aspiration1", "aspiration2"]
  }
}
```
