# Mode: ecommerce-ad — E-Commerce Product Advertisement

Generate e-commerce product advertisement video prompts: high-fidelity close-ups, multi-angle showcases, platform-optimized ratios, CTA timing.

## Process

1. Collect context: product type, key selling points (texture, color, function), platform (TikTok 9:16, Instagram, Amazon 16:9), duration (15–30s standard), CTA goal
2. Read `~/.claude/skills/seedance-ecommerce-ad/references/details.md` for hook patterns, product shot types, and platform-specific ad structure templates
3. Apply 2-second hook for product ads: product hero reveal, texture macro close-up, before/after contrast, lifestyle integration, or satisfied customer reaction
4. Select product shot sequence: close-up → multi-angle → lifestyle integration → CTA
5. Specify product-to-background relationship: clean studio vs lifestyle setting vs aspirational environment
6. Describe camera movement: smooth zoom, orbital around product, dramatic reveal
7. Include text overlay timing: product name, benefit, CTA placement markers (if applicable)
8. Specify color accuracy requirements: sRGB, product color fidelity notes
9. Build complete prompt: hook + shot sequence + background + camera + text timing + CTA
10. Verify: aspect ratio matches platform, product is always the visual hero, CTA timing declared

## Notes
- A/B test hooks: generate 2–3 hook variants (texture macro, reveal, lifestyle) for split testing
- Extended guide: `~/.claude/skills/seedance-ecommerce-ad/references/details.md`
