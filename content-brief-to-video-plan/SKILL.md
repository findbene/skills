---
name: content-brief-to-video-plan
description: 'Converts a content brief, topic idea, keyword, or article into a complete video production plan — including. Triggers: "use content-brief-to-video-plan", "content brief to video plan", "content task".'
allowed-tools: Glob, Grep, Read
---

# Content Brief to Video Plan

You turn ideas into actionable video production plans. The output should be usable
by a solo creator or an automated pipeline — clear enough that the creator (human
or AI) can execute without any additional decisions.

## Inputs to collect

If not already provided:

- **Topic / brief / keyword** — what is the video about?
- **Platform** — YouTube Shorts / TikTok / Instagram Reels / Rumble / long-form YouTube?
- **Duration target** — 30s, 60s, 3 min, 10 min? (Platform implies a default if not stated)
- **Niche / brand voice** — who is the creator? What tone? (e.g., "TheRightClip — news clips, fast-cut, neutral")
- **Content format** — talking head, voiceover + B-roll, text-on-screen, or mixed?
- **Goal** — views/reach, clicks to affiliate, brand awareness, channel growth?

Topic alone is enough to start. Infer reasonable defaults for anything missing.

## Video plan output structure

---

## Video Plan: [Topic]
*Platform: [X] | Duration: [X]s / [X] min | Format: [X]*

### Hook (0–3 seconds)
The first frame must stop the scroll. Write 2–3 hook options — the creator picks one.

Hook types to consider:
- **Pattern interrupt** — something visually unexpected or a bold claim
- **Open loop** — "You've been doing X wrong for years."
- **Curiosity gap** — "Most people don't know this about X."
- **Direct value** — "Here's how to X in 60 seconds."

For short-form, the hook IS the title frame and first spoken line. They must be
the same message.

### Title / headline
Suggested title optimized for the platform's algorithm:
- **YouTube Shorts**: searchable, keyword-forward ("How X Actually Works in 2025")
- **TikTok / Reels**: emotional or curiosity-driven ("POV: You finally understand X")
- **Rumble**: clear news/informational framing

### Script outline

Break the video into timed segments. Use this format:

| Timestamp | Scene | Spoken / On-screen text | Visual / B-roll note |
|-----------|-------|------------------------|----------------------|
| 0:00–0:03 | Hook | "[Exact words]" | [Close-up, fast cut, graphic overlay] |
| 0:03–0:15 | Setup | "..." | [talking head / stock clip of X] |
| ... | ... | ... | ... |

Be specific about on-screen text (exact wording, not "add text here").
For B-roll, name the type of shot: "aerial cityscape," "hands on keyboard CU," not "relevant footage."

### Full script (optional — generate if user asks, or for short-form always include)

Write the complete spoken script word-for-word. Mark emphasis words in **bold**.
Short punchy sentences. One idea per sentence.

### Caption / description copy

**Short-form caption** (TikTok / Reels / Shorts):
- First line = hook restatement (must work as standalone in the feed)
- 3–5 hashtags (specific over generic: `#newstok` not `#viral`)
- CTA if relevant

**YouTube description** (for long-form or Shorts with description):
- 2–3 sentence summary with keywords
- Timestamps if applicable
- Links / CTA

### Thumbnail concept (YouTube / Rumble)
Describe the thumbnail in enough detail to brief a designer or generate with AI:
- Background: [color / scene]
- Subject: [person, graphic, screenshot]
- Text overlay: [exact words, max 5]
- Emotion/energy: [shocked / confident / urgent / curious]

### Production notes
Any specific direction for this video:
- Music energy: [upbeat / tense / neutral]
- Pacing: [fast cut every 2s / steady talking head]
- Graphics needed: [lower thirds / animated text / data overlay]
- Voiceover vs. on-camera: recommendation with rationale

---

## Platform defaults

| Platform | Ideal duration | Aspect ratio | Caption style | Hook window |
|----------|---------------|--------------|---------------|-------------|
| YouTube Shorts | 45–60s | 9:16 | 1 line + hashtags | 2s |
| TikTok | 30–90s | 9:16 | Conversational + hashtags | 1–2s |
| Instagram Reels | 15–90s | 9:16 | Clean, emoji OK | 1s |
| Rumble | 60s–10 min | 16:9 preferred | Full description | 3–5s |
| YouTube long-form | 8–20 min | 16:9 | Full SEO description | 15–30s |

## For automated pipelines

If this is going into an AI pipeline (e.g., Citadel media pipeline), output the
script and B-roll notes in a structured format the pipeline can consume directly.
Ask the user if they need a specific JSON or dict format.

## When NOT to use

- Task is unrelated to content brief to video plan — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Content Brief To Video Plan needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for content brief to video plan
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving content brief to video plan
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
