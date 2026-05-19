# Mode: youtube — YouTube Ads Analysis

YouTube Ads specific analysis covering campaign types, creative quality, audience targeting, and measurement.

## Process

1. Collect YouTube Ads data (Google Ads export filtered to Video campaigns)
2. Read `ads/references/google-audit.md` for YouTube-relevant checks (incl. G-DG1 through G-DG3, G-CTV1)
3. Read `ads/references/platform-specs.md` for video specifications
4. Read `ads/references/benchmarks.md` for YouTube benchmarks
5. Read `ads/references/scoring-system.md` for health score algorithm
6. **Validate**: confirm at least one active video campaign exists before proceeding
7. **Check**: flag any remaining Video Action Campaigns (VAC). All auto-upgraded to Demand Gen by April 2026
8. Evaluate campaign setup, creative quality, targeting, and measurement
9. **Validate**: verify all campaign types identified before generating report
10. Generate YouTube-specific findings report with health score

## Notes
- Extended checklist: `~/.claude/skills/ads-youtube/references/details.md`
- v1.5: Demand Gen replaces VAC, Shorts expansion, CTV section, frequency capping
