---
name: ads-generate
description: "AI image generation for paid ad creatives. Reads campaign-brief.md and brand-profile.json to produce platform-sized ad images using ba. Triggers: 'use ads-generate', 'run ads generate', 'ads generate."
allowed-tools: Glob, Grep, Read, Task
user-invokable: false
---

# Ads Generate: AI Ad Image Generator

Generates platform-sized ad creative images from your campaign brief and brand
profile. Uses banana-claude as the image generation provider.

## Process

### Step 1: Verify banana-claude

Verify banana-claude is installed (run `/banana setup` to check). If not installed,
display setup instructions and exit.

### Step 2: Locate Source Files

Check for:
- `campaign-brief.md` → primary source for prompts and dimensions
- `brand-profile.json` → brand color/style injection (optional but recommended)

**If campaign-brief.md is found**: Use `## Image Generation Briefs` section as the
generation job list.

**If no campaign-brief.md**: Enter standalone mode (Step 2b).

#### Step 2b: Standalone Mode

Ask the user:
1. Generation prompt (what should the image show?) → verify: step output matches expected outcome
2. Target platform (to set correct dimensions) → verify: step output matches expected outcome
3. Output filename (optional) → verify: step output matches expected outcome

Then skip to Step 5.

### Step 3: Read Provider Config

Load `~/.claude/skills/ads/references/image-providers.md` to confirm:
- Active provider pricing (show user the cost estimate)
- Rate limits for current tier
- Batch API availability

### Step 4: Read Platform Specs

For each platform in the campaign brief, load the relevant spec reference:
- `~/.claude/skills/ads/references/meta-creative-specs.md`
- `~/.claude/skills/ads/references/google-creative-specs.md`
- `~/.claude/skills/ads/references/tiktok-creative-specs.md`
- `~/.claude/skills/ads/references/linkedin-creative-specs.md`
- `~/.claude/skills/ads/references/youtube-creative-specs.md`
- `~/.claude/skills/ads/references/microsoft-creative-specs.md`

### Step 5: Prepare banana Configuration

Create banana brand preset from brand-profile.json if one does not already exist
at `~/.banana/presets/{brand-slug}.json`.

Select banana domain mode based on campaign brief content:
- **Product**: e-commerce, packshots
- **Editorial**: brand awareness, lifestyle
- **Cinema**: video thumbnails, dramatic
- **UI/Web**: app install, SaaS
- **Portrait**: testimonials, people

### Step 6: Spawn Visual Designer Agent

Spawn the `visual-designer` agent using the Task tool with `context: fork`,
passing the selected domain mode and preset name.

The agent will:
- Parse the image generation briefs from campaign-brief.md
- Inject brand colors and mood from brand-profile.json
- Use banana-claude with the configured domain mode for each asset
- Save to `./ad-assets/[platform]/[concept]/` directory structure
- Write `generation-manifest.json`

### Step 7: Validate with Format Adapter

After the visual-designer completes, spawn the `format-adapter` agent
with `context: fork` to validate dimensions and report missing formats.

### Step 8: Quality Gate

Use Claude vision to assess each generated image against the brief (score 1 to 10
on brand alignment, composition, platform fit). If any image scores below 6,
regenerate once with an adjusted prompt.

Quality Gate Rubric:
- 9-10: Professional quality, brand-aligned, platform-optimized, no issues
- 7-8: Good quality, minor composition or brand alignment improvements possible
- 5-6: Acceptable but needs regeneration. Text readability issues, poor composition, or brand mismatch
- Below 5: Reject. Regenerate with adjusted prompt

### Step 9: Aggregate Costs

Read banana cost data from `~/.banana/costs.json` and include total creative spend
in generation-manifest.json.

### Step 10: Report Results

Present a summary:
```
Generation complete:

  Generated assets:
    ✓ ./ad-assets/meta/concept-1/feed-1080x1350.png
    ✓ ./ad-assets/tiktok/concept-1/vertical-1080x1920.png
    ✗ ./ad-assets/google/concept-1/landscape-1200x628.png [error reason]

  Format validation: See format-report.md

  Cost: $[N] total creative spend (from ~/.banana/costs.json)

  Next steps:
    1. Review assets in ./ad-assets/ → verify: step output matches expected outcome
    2. Check format-report.md for any missing formats → verify: all tests pass
    3. Upload to your ad platform managers → verify: file readable + content matches expected shape
```

## Reference Files

- `~/.claude/skills/ads/references/image-providers.md`: provider config, pricing, limits
- `~/.claude/skills/ads/references/[platform]-creative-specs.md`: per-platform specs
- `~/.claude/skills/ads/references/brand-dna-template.md`: brand injection schema

## When NOT to use

- Task is unrelated to ads generate — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Ads Generate needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for ads generate
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving ads generate
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

## References

Extended sections moved to `references/details.md`.
