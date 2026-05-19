# Mode: comic-to-video — Comic Book to Animated Video

Convert comic panels, manga pages, webtoons, and illustrated storyboards into animated video using Seedance 2.0 on Higgsfield's story inference.

## Process

1. Collect context: source art format (Western comic, manga, webtoon, storyboard), reading direction (L→R, R→L, vertical scroll), number of panels, duration target, platform ratio
2. Read `~/.claude/skills/seedance-comic-to-video/references/details.md` for full hook framework, panel-to-motion translation guide, and art fidelity techniques
3. Identify the narrative action: what movement, emotion, or transition is being animated from the panel(s)?
4. Apply 2-second hook matching comic genre (action freeze, panel crack effect, ink splash reveal, speech bubble pop-in, dramatic zoom into panel center)
5. Describe art fidelity requirements: line weight preservation, halftone pattern, color separation style, original palette
6. Specify panel sequencing: reading order, camera pan/zoom direction, panel transition style
7. Include character continuity instructions if multi-panel: pose, expression consistency
8. Build complete prompt: source style + hook + art fidelity + panel sequence + motion duration
9. Verify: art style vocabulary used, reading direction specified, duration appropriate to panel count

## Notes
- Seedance infers narrative flow from panel sequences — describe the intended motion, not just describe the static panel
- Extended guide: `~/.claude/skills/seedance-comic-to-video/references/details.md`
