# Seedance — Shared Reference: Common Boilerplate

## When NOT to use

- Task is unrelated to Seedance 2.0 on Higgsfield video prompt generation
- User needs live video editing software, not AI prompt generation
- User explicitly names a different platform (Runway, Sora, Kling, Pika) — acknowledge and exit
- Request is for audio-only or image-only output
- User asks for raw output without skill discipline → respect override

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Seedance needs domain-specific prompt language, not boilerplate |
| "I'll skip platform specs" | Wrong ratio or duration = rejected or cropped output |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Prompt is technically specific: camera movement, lighting, motion timing, material properties described
- 2-second hook is explicitly built into the opening description
- Aspect ratio and duration declared and matched to user's stated platform
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- User can copy-paste the final prompt directly into Seedance 2.0 on Higgsfield without modification

## Shared Platform Specs

**Aspect ratio → platform mapping:**
- 9:16 — TikTok, Instagram Reels, YouTube Shorts, Stories
- 16:9 — YouTube, web, desktop, LinkedIn
- 1:1 — Instagram feed, Twitter/X feed
- 4:5 — Instagram feed (portrait crop)

**Duration guidelines:**
- 6–15s — viral social hooks, product teasers
- 15–30s — ads, product demos, brand moments
- 30–60s — showcases, lookbooks, music clips
- 60s+ — brand stories, real estate tours, narrative videos

**Frame rate selection:**
- 24fps — cinematic, luxury, narrative, brand
- 30fps — web-optimized, product, social, general
- 60fps — action, tech UI, slow-motion source, sports
