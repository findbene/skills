---
name: "x-twitter-growth"
description: "X/Twitter growth engine for building audience, crafting viral content, and analyzing engagement. Triggers: 'use x-twitter-growth', 'x twitter growth', 'x-twitter-growth task'."
allowed-tools: Glob, Grep, Read
license: MIT
metadata:
  version: 1.0.0
  author: Alireza Rezvani
  category: marketing
  updated: 2026-03-10
---

# X/Twitter Growth Engine

X-specific growth skill. For general social media content across platforms, see `social-content`. For social strategy and calendar planning, see `social-media-manager`. This skill goes deep on X.

## When to Use This vs Other Skills

| Need | Use |
|------|-----|
| Write a tweet or thread | **This skill** |
| Plan content across LinkedIn + X + Instagram | social-content |
| Analyze engagement metrics across platforms | social-media-analyzer |
| Build overall social strategy | social-media-manager |
| X-specific growth, algorithm, competitive intel | **This skill** |

---

## Step 1 — Profile Audit

Before any growth work, audit the current X presence. Run `scripts/profile_auditor.py` with the handle, or manually assess:

### Bio Checklist
- [ ] Clear value proposition in first line (who you help + how)
- [ ] Specific niche — not "entrepreneur | thinker | builder"
- [ ] Social proof element (followers, title, metric, brand)
- [ ] CTA or link (newsletter, product, site)
- [ ] No hashtags in bio (signals amateur)

### Pinned Tweet
- [ ] Exists and is less than 30 days old
- [ ] Showcases best work or strongest hook
- [ ] Has clear CTA (follow, subscribe, read)

### Recent Activity (last 30 posts)
- [ ] Posting frequency: minimum 1x/day, ideal 3-5x/day
- [ ] Mix of formats: tweets, threads, replies, quotes
- [ ] Reply ratio: >30% of activity should be replies
- [ ] Engagement trend: improving, flat, or declining

Run: `python3 scripts/profile_auditor.py --handle @username`

---

## Step 2 — Competitive Intelligence

Research competitors and successful accounts in your niche using web search.

### Process
1. Search `site:x.com "topic" min_faves:100` via Brave to find high-performing content → verify: step output matches expected outcome
2. Identify 5-10 accounts in your niche with strong engagement → verify: step output matches expected outcome
3. For each, analyze: posting frequency, content types, hook patterns, engagement rates → verify: step output matches expected outcome
4. Run: `python3 scripts/competitor_analyzer.py --handles @acc1 @acc2 @acc3` → verify: command exit code 0

### What to Extract
- **Hook patterns** — How do top posts start? Question? Bold claim? Statistic?
- **Content themes** — What 3-5 topics get the most engagement?
- **Format mix** — Ratio of tweets vs threads vs replies vs quotes
- **Posting times** — When do their best posts go out?
- **Engagement triggers** — What makes people reply vs like vs retweet?

---

## Step 3 — Content Creation

### Tweet Types (ordered by growth impact)

#### 1. Threads (highest reach, highest follow conversion)
```
Structure:
- Tweet 1: Hook — must stop the scroll in <7 words
- Tweet 2: Context or promise ("Here's what I learned:")
- Tweets 3-N: One idea per tweet, each standalone-worthy
- Final tweet: Summary + explicit CTA ("Follow @handle for more")
- Reply to tweet 1: Restate hook + "Follow for more [topic]"

Rules:
- 5-12 tweets optimal (under 5 feels thin, over 12 loses people)
- Each tweet should make sense if read alone
- Use line breaks for readability
- No tweet should be a wall of text (3-4 lines max)
- Number the tweets or use "↓" in tweet 1
```

#### 2. Atomic Tweets (breadth, impression farming)
```
Formats that work:
- Observation: "[Thing] is underrated. Here's why:"
- Listicle: "10 tools I use daily:\n\n1. X — for Y"
- Contrarian: "Unpopular opinion: [statement]"
- Lesson: "I [did X] for [time]. Biggest lesson:"
- Framework: "[Concept] explained in 30 seconds:"

Rules:
- Under 200 characters gets more engagement
- One idea per tweet
- No links in tweet body (kills reach — put link in reply)
- Question tweets drive replies (algorithm loves replies)
```

#### 3. Quote Tweets (authority building)
```
Formula: Original tweet + your unique take
- Add data the original missed
- Provide counterpoint or nuance
- Share personal experience that validates/contradicts
- Never just say "This" or "So true"
```

#### 4. Replies (network growth, fastest path to visibility)
```
Strategy:
- Reply to accounts 2-10x your size
- Add genuine value, not "great post!"
- Be first to reply on accounts with large audiences
- Your reply IS your content — make it tweet-worthy
- Controversial/insightful replies get quote-tweeted (free reach)
```

Run: `python3 scripts/tweet_composer.py --type thread --topic "your topic" --audience "your audience"`

---

## Step 4 — Algorithm Mechanics

### What X rewards (2025-2026)
| Signal | Weight | Action |
|--------|--------|--------|
| Replies received | Very high | Write reply-worthy content (questions, debates) |
| Time spent reading | High | Threads, longer tweets with line breaks |
| Profile visits from tweet | High | Curiosity gaps, tease expertise |
| Bookmarks | High | Tactical, save-worthy content (lists, frameworks) |
| Retweets/Quotes | Medium | Shareable insights, bold takes |
| Likes | Low-medium | Easy agreement, relatable content |
| Link clicks | Low (penalized) | Never put links in tweet body — use reply |

### What kills reach
- Links in tweet body (put in first reply instead)
- Editing tweets within 30 min of posting
- Posting and immediately going offline (no early engagement)
- More than 2 hashtags
- Tagging people who don't engage back
- Threads with inconsistent quality (one weak tweet tanks the whole thread)

### Optimal Posting Cadence
| Account size | Tweets/day | Threads/week | Replies/day |
|-------------|------------|--------------|-------------|
| < 1K followers | 2-3 | 1-2 | 10-20 |
| 1K-10K | 3-5 | 2-3 | 5-15 |
| 10K-50K | 3-7 | 2-4 | 5-10 |
| 50K+ | 2-5 | 1-3 | 5-10 |

---

## Step 5 — Growth Playbook

### Week 1-2: Foundation
1. Optimize bio and pinned tweet (Step 1) → verify: step output matches expected outcome
2. Identify 20 accounts in your niche to engage with daily → verify: step output matches expected outcome
3. Reply 10-20 times per day to larger accounts (genuine value only) → verify: step output matches expected outcome
4. Post 2-3 atomic tweets per day testing different formats → verify: all checks pass
5. Publish 1 thread → verify: file content matches expected shape

### Week 3-4: Pattern Recognition
1. Review what formats got most engagement → verify: step output matches expected outcome
2. Double down on top 2 content formats → verify: step output matches expected outcome
3. Increase to 3-5 posts per day → verify: step output matches expected outcome
4. Publish 2-3 threads per week → verify: file content matches expected shape
5. Start quote-tweeting relevant content daily → verify: step output matches expected outcome

### Month 2+: Scale
1. Develop 3-5 recurring content series (e.g., "Friday Framework") → verify: step output matches expected outcome
2. Cross-pollinate: repurpose threads as LinkedIn posts, newsletter content → verify: file content matches expected shape
3. Build reply relationships with 5-10 accounts your size (mutual engagement) → verify: step output matches expected outcome
4. Experiment with spaces/audio if relevant to niche → verify: step output matches expected outcome
5. Run: `python3 scripts/growth_tracker.py --handle @username --period 30d` → verify: command exit code 0

---

## Step 6 — Content Calendar Generation

Run: `python3 scripts/content_planner.py --niche "your niche" --frequency 5 --weeks 2`

Generates a 2-week posting plan with:
- Daily tweet topics with hook suggestions
- Thread outlines (2-3 per week)
- Reply targets (accounts to engage with)
- Optimal posting times based on niche

---

## Scripts

| Script | Purpose |
|--------|---------|
| `scripts/profile_auditor.py` | Audit X profile: bio, pinned, activity patterns |
| `scripts/tweet_composer.py` | Generate tweets/threads with hook patterns |
| `scripts/competitor_analyzer.py` | Analyze competitor accounts via web search |
| `scripts/content_planner.py` | Generate weekly/monthly content calendars |
| `scripts/growth_tracker.py` | Track follower growth and engagement trends |

## Common Pitfalls

1. **Posting links directly** — Always put links in the first reply, never in the tweet body → verify: step output matches expected outcome
2. **Thread tweet 1 is weak** — If the hook doesn't stop scrolling, nothing else matters → verify: file content matches expected shape
3. **Inconsistent posting** — Algorithm rewards daily consistency over occasional bangers → verify: step output matches expected outcome
4. **Only broadcasting** — Replies and engagement are 50%+ of growth, not just posting → verify: step output matches expected outcome
5. **Generic bio** — "Helping people do things" tells nobody anything → verify: step output matches expected outcome
6. **Copying formats without adapting** — What works for tech Twitter doesn't work for marketing Twitter → verify: step output matches expected outcome

## Related Skills

- `social-content` — Multi-platform content creation
- `social-media-manager` — Overall social strategy
- `social-media-analyzer` — Cross-platform analytics
- `content-production` — Long-form content that feeds X threads
- `copywriting` — Headline and hook writing techniques

## When NOT to use

- LinkedIn / Instagram / TikTok content — use `linkedin-creator`, `instagram-curator`, etc.
- Cross-platform content calendar — use `social-content` or `social-media-manager`
- Twitter advertising / promoted tweets — use `paid-ads` or `ads`
- Engagement metrics analysis only — use `social-media-analyzer`
- Pure copywriting (no platform specifics) — use `copywriting`

## Red Flags

| Rationalization | Reality |
|---|---|
| "Post the link in the main tweet for clicks" | Algorithm demotes tweets with outbound links; use "link in replies" pattern |
| "Long threads of 30 tweets get more reach" | Optimal length is 5-12 tweets; >15 tweets drop completion rate sharply |
| "Buy followers / use engagement pods" | Algorithmically detected, suppresses reach permanently; grow with genuine engagement |
| "Skip the audit, just start posting" | Without profile + voice audit, growth is incoherent — load `scripts/profile_auditor.py` first |

## Output Contract

Finished output must contain:
- Profile audit (bio, banner, pinned tweet, voice) before any content work
- Content plan with proportional mix (educational threads, commentary, replies, hooks)
- Hook tested against curiosity/specificity/contrarian heuristics
- Thread architecture: hook tweet, 1-idea-per-tweet body, payoff tweet, CTA
- Posting cadence recommendation aligned to user's time zone audience
- Engagement plan (which accounts to reply to, in what proportion)

## Examples

**Example 1 — Audit and 30-day growth plan**
- Input: "Audit my account @example and give me a 30-day growth plan"
- Action: Run `scripts/profile_auditor.py @example` → score bio/banner/pinned → analyze last 50 tweets for voice consistency → produce 30-day plan with daily targets
- Output: Audit report (3 fixes), 30-day calendar (1 thread/wk, 3 commentary/day, 10 replies/day), 5 hook templates tailored to niche

**Example 2 — Write a viral thread**
- Input: "Write a thread about lessons from shipping a SaaS to $10K MRR"
- Action: Craft hook (specific number + contrarian angle) → 8-tweet body with one lesson per tweet → payoff with biggest mistake → CTA to follow
- Output: Hook + 9 tweets ready to schedule, each under 280 chars, with rationale for each lesson's placement
