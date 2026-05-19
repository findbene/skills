---
name: seedance
description: "Multi-style AI video prompt generator for Seedance 2.0 on Higgsfield. 15 specialized modes for cinematic, anime, brand, e-commerce, social, and more. Triggers: 'use seedance', 'run seedance', 'seedance'."
allowed-tools: Glob, Grep, Read
---

# Seedance: Multi-Style Video Prompt Generator

Generates production-grade video prompts for **Seedance 2.0 on Higgsfield** across 15 specialized styles.
Each mode routes to a style-specific execution guide plus shared platform expertise.

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/seedance 3d-cgi` | 3D CGI / rendered video: Blender, UE5, Octane, particle systems |
| `/seedance anime-action` | Anime-style: cel shading, genre guides, effects library |
| `/seedance brand-story` | Brand narrative: story arcs, emotional registers, visual metaphors |
| `/seedance cartoon` | Cartoon/animation: 15 art styles, animation principles, palettes |
| `/seedance cinematic` | Cinematic film: 15 camera techniques, 10 lighting setups, color grading |
| `/seedance comic-to-video` | Comic panel → animated video: story inference, panel-to-motion |
| `/seedance ecommerce-ad` | E-commerce product ads: high-fidelity close-ups, platform ratios |
| `/seedance fashion-lookbook` | Fashion/model showcase: garment motion, editorial hooks |
| `/seedance fight-scenes` | Fight scene / action: choreography, physics, multi-character |
| `/seedance food-beverage` | Food/beverage: texture revelation, ASMR moments, appetite triggers |
| `/seedance motion-design-ad` | Motion design ads for SaaS/tech: UI morphing, data visualization |
| `/seedance music-video` | Music video: beat-sync, @audio1 upload, genre aesthetics |
| `/seedance product-360` | Product 360° turntable: reflections, material fidelity, reveal |
| `/seedance real-estate` | Real estate/architecture: walkthroughs, lighting times, immersive tour |
| `/seedance social-hook` | Viral social hooks: TikTok/Reels/Shorts, completion rate mechanics |

## Context Intake (Required: Do This First)

Before generating any prompt, collect:

1. **Mode**: Which style? (see routing table above)
2. **Subject**: What is being filmed/shown? (product, character, scene, brand)
3. **Platform**: Where will it publish? → TikTok/Reels/Shorts (9:16) · YouTube (16:9) · Instagram (1:1) · Web (16:9)
4. **Duration**: How long? → 6–15s (social hook) · 15–30s (ad) · 30–60s (showcase) · 60s+ (brand/narrative)
5. **Mood/tone**: Energy level, color palette direction, emotional register

If user provides enough upfront ("make a 15s TikTok ad for my skincare product"), extract context and proceed.

## Platform Specifications

**Seedance 2.0 on Higgsfield** core capabilities:
- Resolution: 1080p–4K (8K for select modes)
- Frame rates: 24fps (cinematic) · 30fps (web) · 60fps (action/tech)
- Aspect ratios: 16:9 · 9:16 · 1:1 · 4:5
- Duration: 6 seconds to 2+ minutes
- Audio input: `@audio1` for music-video beat-sync
- Render quality: photorealistic, physically accurate lighting, material fidelity

## Quality Gates

Hard rules (never violate):
- Always use the 2-second hook framework — first 2 seconds determine skip/watch
- Never use vague prompts ("make it look cool") — technical specificity required
- Match aspect ratio to platform before generating
- Never request hyperrealistic gore/explicit violence — suggest impact/aftermath instead
- For music-video mode, always include `@audio1` upload reference with timing markers
- Platform optimization: check per-platform specs in mode file before finalizing

## Reference Files

Load on-demand; do NOT load all at startup.

**Path resolution:** All references at `~/.claude/skills/seedance/references/`.

- `references/common.md` — shared When NOT to use, Red Flags, Output Contract
- `references/modes/3d-cgi.md` — 3D CGI execution steps
- `references/modes/anime-action.md` — anime-action execution steps
- `references/modes/brand-story.md` — brand storytelling execution steps
- `references/modes/cartoon.md` — cartoon/animation execution steps
- `references/modes/cinematic.md` — cinematic film execution steps
- `references/modes/comic-to-video.md` — comic-to-video execution steps
- `references/modes/ecommerce-ad.md` — e-commerce ad execution steps
- `references/modes/fashion-lookbook.md` — fashion lookbook execution steps
- `references/modes/fight-scenes.md` — fight scene execution steps
- `references/modes/food-beverage.md` — food/beverage execution steps
- `references/modes/motion-design-ad.md` — motion design ad execution steps
- `references/modes/music-video.md` — music video execution steps
- `references/modes/product-360.md` — product 360° execution steps
- `references/modes/real-estate.md` — real estate execution steps
- `references/modes/social-hook.md` — social hook execution steps

## When NOT to use

- Task is unrelated to video prompt generation or Seedance 2.0 on Higgsfield
- User needs video editing, not prompt writing
- User wants a different AI video platform (Runway, Sora, Kling) — acknowledge platform difference
- Simple one-line operation that doesn't need structured prompt engineering

## Red Flags

| Thought | Reality |
|---------|---------|
| "Generic prompt is good enough" | Vague = poor output; specificity drives quality on Seedance |
| "Skip the hook, get to the content" | First 2 seconds determine completion rate — never skip |
| "Platform ratio doesn't matter" | Wrong ratio = cropped content, algorithm penalty |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Prompt is technically specific (camera, lighting, motion, timing described)
- 2-second hook is built into the opening
- Aspect ratio and duration match stated platform
- Edge cases addressed or flagged with explicit assumption
- User can copy-paste prompt directly into Seedance 2.0 on Higgsfield

## Examples

### Example 1 — golden path
- Input: "Make a 15-second TikTok ad for a skincare serum"
- Action: Detect mode (ecommerce-ad), collect platform (9:16, 15s), load mode file, apply hook framework, build prompt
- Output: Complete Seedance prompt with 2-second hook + product close-up + CTA timing

### Example 2 — edge case
- Input: "Make a fight scene video" with no style specified
- Action: Ask: anime/cinematic/cartoon? Then proceed with correct mode
- Output: Mode-appropriate prompt + explicit assumption noted if user doesn't clarify
