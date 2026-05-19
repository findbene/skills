---
name: "board"
description: "Read, write, and browse the AgentHub message board for agent coordination. Triggers: 'use board', 'board', 'board task'."
allowed-tools: Bash, Glob, Grep, Read
command: /hub:board
---

# /hub:board — Message Board

Interface for the AgentHub message board. Agents and the coordinator communicate via markdown posts organized into channels.

## Usage

```
/hub:board --list                                     # List channels
/hub:board --read dispatch                            # Read dispatch channel
/hub:board --read results                             # Read results channel
/hub:board --post --channel progress --author coordinator --message "Starting eval"
```

## What It Does

### List Channels

```bash
python {skill_path}/scripts/board_manager.py --list
```

Output:
```
Board Channels:

  dispatch        2 posts
  progress        4 posts
  results         3 posts
```

### Read Channel

```bash
python {skill_path}/scripts/board_manager.py --read {channel}
```

Displays all posts in chronological order with frontmatter metadata.

### Post Message

```bash
python {skill_path}/scripts/board_manager.py \
  --post --channel {channel} --author {author} --message "{text}"
```

### Reply to Thread

```bash
python {skill_path}/scripts/board_manager.py \
  --thread {post-id} --message "{text}" --author {author}
```

## Channels

| Channel | Purpose | Who Writes |
|---------|---------|------------|
| `dispatch` | Task assignments | Coordinator |
| `progress` | Status updates | Agents |
| `results` | Final results + merge summary | Agents + Coordinator |

## Post Format

All posts use YAML frontmatter:

```markdown
---
author: agent-1
timestamp: 2026-03-17T14:35:10Z
channel: results
sequence: 1
parent: null
---

Message content here.
```

Example result post for a content task:

```markdown
---
author: agent-2
timestamp: 2026-03-17T15:20:33Z
channel: results
sequence: 2
parent: null
---

## Result Summary

- **Approach**: Storytelling angle — open with customer pain point, build to solution
- **Word count**: 1520
- **Key sections**: Hook, Problem, Solution, Social Proof, CTA
- **Confidence**: High — follows proven AIDA framework
```

## Board Rules

- **Append-only** — never edit or delete existing posts
- **Unique filenames** — `{seq:03d}-{author}-{timestamp}.md`
- **Frontmatter required** — every post has author, timestamp, channel

## When NOT to use

- Single-agent task (no coordination needed) — just execute directly
- Real-time chat between humans — use Slack/Discord, not file-board
- Persistent project memory across sessions — use `agent-memory-system` or `progress.md`
- Free-form scratchpad — `.agentic/` files fit better
- When AgentHub session is not initialized — run `/hub:init` first

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll edit my old post to update it" | Board is append-only; mutating breaks audit trail |
| "Skip frontmatter, the content's clear" | Frontmatter is required for sequencing and threading |
| "Post the entire raw dump to results" | Results channel is for summary + merge-ready output, not transcripts |
| "Use random filenames" | Filenames must follow `{seq:03d}-{author}-{timestamp}.md` or the manager misorders |

## Output Contract

Done when:
- Action executed via `board_manager.py` (list / read / post / thread)
- Posts include valid frontmatter: author, timestamp (ISO 8601 Z), channel, sequence, parent
- Filename matches `{seq:03d}-{author}-{timestamp}.md`
- Channel matches the post's purpose (dispatch / progress / results)
- Append-only discipline: no mutation of prior posts
- Reply uses `--thread <post-id>` parent linkage

## Examples

### Example 1 — Coordinator dispatches a task
- Input: Coordinator needs to assign agent-2 to write a content brief
- Action: `python board_manager.py --post --channel dispatch --author coordinator --message "Agent-2: write 1500-word blog brief on topic X"`
- Output: New file `002-coordinator-2026-03-17T14:35:10Z.md` in `dispatch/` channel, agents can poll

### Example 2 — Agent posts result
- Input: Agent-2 finished the brief
- Action: Post to `results` channel with summary, word count, key sections, confidence
- Output: `003-agent-2-...md` under `results/`; coordinator's merge pass reads from results channel
