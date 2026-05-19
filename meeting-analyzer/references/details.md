# Extended Details

## Top 3 Findings

[Rank by impact. Each finding gets 2-3 sentences + one concrete example with a direct quote and timestamp.]

## Detailed Analysis

### Speaking Dynamics
[Stats table + narrative interpretation + flagged red flags]

### Directness & Conflict Patterns
[Flagged instances grouped by pattern type, with quotes and rewrites]

### Verbal Habits
[Filler word stats, contextual spikes, only if rate > 3/100 words]

### Listening & Questions
[Question type breakdown, listening indicators, specific examples]

### Facilitation
[Only if applicable — agenda, decisions, action items]

### Energy & Sentiment
[Arc summary, flagged drops]

## Strengths
[3 specific things the user does well, with evidence]

## Growth Opportunities
[3 ranked by impact, each with: what to change, why it matters, a concrete "try this next time" action]

## Comparison to Previous Period
[Only if prior analysis exists — delta on key metrics]
```

### 5. Follow-Up Options

After delivering the report, offer:
- Deep dive into any specific meeting or pattern
- A 1-page "communication cheat sheet" with the user's top 3 habits to change
- Tracking setup — save current metrics as a baseline for future comparison
- Export as markdown or structured JSON for use in performance reviews

---

## Edge Cases

- **No speaker labels**: Warn the user upfront. Run text-level analysis (filler words, question types on the full transcript) but skip per-speaker metrics. Suggest re-exporting with speaker diarization enabled.
- **Very short meetings** (< 5 minutes or < 500 words): Analyze but caveat that patterns from short meetings may not be representative.
- **Non-English transcripts**: The filler word and hedging dictionaries are English-centric. For other languages, note the limitation and focus on structural analysis (speaking ratios, turn-taking, question counts).
- **Single meeting vs. corpus**: If only one transcript, skip trend/comparison language. Focus findings on that meeting alone.
- **User not identified**: If you can't determine which speaker is the user after scanning, ask before proceeding. Don't guess.

## Transcript Source Tips

Include this section in output only if the user seems unsure about how to get transcripts:

- **Zoom**: Settings → Recording → enable "Audio transcript". Download `.vtt` from cloud recordings.
- **Google Meet**: Auto-transcription saves to Google Docs in the calendar event's Drive folder.
- **Granola**: Exports to markdown. Best speaker label quality of consumer tools.
- **Otter.ai**: Export as `.txt` or `.json` from the web dashboard.
- **Fireflies.ai**: Export as `.docx` or `.json` — both work.
- **Microsoft Teams**: Transcripts appear in the meeting chat. Download as `.vtt`.

Recommend `YYYY-MM-DD - Meeting Name.ext` naming convention for easy chronological analysis.

---

## Anti-Patterns

| Anti-Pattern | Why It Fails | Better Approach |
|---|---|---|
| Analyzing without speaker labels | Per-person metrics impossible — results are generic word clouds | Ask user to re-export with speaker identification enabled |
| Running all modules on a 5-minute standup | Overkill — filler word and conflict analysis need 20+ min meetings | Auto-detect meeting length and skip irrelevant modules |
| Presenting raw metrics without context | "You said 'um' 47 times" is demoralizing without benchmarks | Always compare to norms and show trajectory over time |
| Analyzing a single meeting in isolation | One meeting is a snapshot, not a pattern — conclusions are unreliable | Require 3+ meetings minimum for trend-based coaching |
| Treating speaking time equality as the goal | A facilitator SHOULD talk less; a presenter SHOULD talk more | Weight speaking ratios by meeting type and role |
| Flagging every hedge word as negative | "I think" and "maybe" are appropriate in brainstorming | Distinguish between decision meetings (hedges are bad) and ideation (hedges are fine) |

---

## Related Skills

| Skill | Relationship |
|-------|-------------|
| `project-management/senior-pm` | Broader PM scope — use for project planning, risk, stakeholders |
| `project-management/scrum-master` | Agile ceremonies — pairs with meeting-analyzer for retro quality |
| `project-management/confluence-expert` | Store meeting analysis outputs as Confluence pages |
| `c-level-advisor/executive-mentor` | Executive communication coaching — complementary perspective |
