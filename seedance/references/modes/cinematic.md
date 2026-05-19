# Mode: cinematic — Cinematic Film-Style Video

Generate cinematic film-style video prompts: 15 camera techniques, 10 lighting setups, color grading presets, timeline segmentation.

## Process

1. Collect context: genre/tone (thriller, drama, documentary, action, romance), subject/scene, duration (4/8/10/15s timeline structures available), platform
2. Read `~/.claude/skills/seedance-cinematic/references/details.md` for camera movement encyclopedia (22 movements), lighting library, color grading presets, and timeline segmentation guide
3. Apply 2-second hook from the hook table (12 techniques): black-screen reveal, whip pan, extreme color shift, fast entry, rack focus, etc.
4. Select camera technique(s): choose from the 15 foundational techniques (establishing shot, push-in, pull-back, whip pan, parallax, handheld, tracking, crane up, 360 orbital, rack focus, Dutch angle, insert, OTS, bird's eye, POV)
5. Specify lighting setup from the 10 setups: three-point, golden hour, high-key, low-key, neon/practical, candlelight, etc.
6. Select color grading preset matching genre mood from details.md
7. Define timeline segmentation if multi-beat: hook / build / payoff structure
8. Build complete prompt: hook + camera technique + lighting + color grade + pacing
9. Verify: camera technique is specific (not vague "cinematic"), timing markers included for multi-beat clips

## Notes
- Timeline segmentation (4/8/10/15s structures) in details.md — use for ads and social formats
- Extended guide: `~/.claude/skills/seedance-cinematic/references/details.md`
