---
name: "eval"
description: "Evaluate and rank agent results by metric or LLM judge for an AgentHub session. Triggers: 'use eval', 'eval', 'eval task'."
allowed-tools: Bash, Glob, Grep, Read
command: /hub:eval
---

# /hub:eval — Evaluate Agent Results

Rank all agent results for a session. Supports metric-based evaluation (run a command), LLM judge (compare diffs), or hybrid.

## Usage

```
/hub:eval                           # Eval latest session using configured criteria
/hub:eval 20260317-143022           # Eval specific session
/hub:eval --judge                   # Force LLM judge mode (ignore metric config)
```

## What It Does

### Metric Mode (eval command configured)

Run the evaluation command in each agent's worktree:

```bash
python {skill_path}/scripts/result_ranker.py \
  --session {session-id} \
  --eval-cmd "{eval_cmd}" \
  --metric {metric} --direction {direction}
```

Output:
```
RANK  AGENT       METRIC      DELTA      FILES
1     agent-2     142ms       -38ms      2
2     agent-1     165ms       -15ms      3
3     agent-3     190ms       +10ms      1

Winner: agent-2 (142ms)
```

### LLM Judge Mode (no eval command, or --judge flag)

For each agent:
1. Get the diff: `git diff {base_branch}...{agent_branch}` → verify: step output matches expected outcome
2. Read the agent's result post from `.agenthub/board/results/agent-{i}-result.md` → verify: file content matches expected shape
3. Compare all diffs and rank by: → verify: step output matches expected outcome
   - **Correctness** — Does it solve the task?
   - **Simplicity** — Fewer lines changed is better (when equal correctness)
   - **Quality** — Clean execution, good structure, no regressions

Present rankings with justification.

Example LLM judge output for a content task:
```
RANK  AGENT    VERDICT                               WORD COUNT
1     agent-1  Strong narrative, clear CTA            1480
2     agent-3  Good data points, weak intro           1520
3     agent-2  Generic tone, no differentiation       1350

Winner: agent-1 (strongest narrative arc and call-to-action)
```

### Hybrid Mode

1. Run metric evaluation first → verify: command exit code 0
2. If top agents are within 10% of each other, use LLM judge to break ties → verify: step output matches expected outcome
3. Present both metric and qualitative rankings → verify: step output matches expected outcome

## After Eval

1. Update session state: → verify: step output matches expected outcome
```bash
python {skill_path}/scripts/session_manager.py --update {session-id} --state evaluating
```

2. Tell the user: → verify: step output matches expected outcome
   - Ranked results with winner highlighted
   - Next step: `/hub:merge` to merge the winner
   - Or `/hub:merge {session-id} --agent {winner}` to be explicit

## When NOT to use

- Single-agent session (nothing to compare) — skip eval
- LLM-as-judge for security-critical code — bring in `code-reviewer` or `security` skill instead
- No agent results yet — agents must finish before eval runs
- General code quality review — use `code-review` or `pr-review-expert`
- A/B test of human-decision options — use `ab-test-setup`

## Red Flags

| Thought | Reality |
|---------|---------|
| "Eyeball the diffs, pick a winner" | Subjective bias; use ranker script or LLM judge with explicit criteria |
| "Skip the eval, merge agent-1" | Process exists to surface the best output; skipping defeats AgentHub |
| "Use --judge for performance test" | Wrong mode — metric mode runs the actual eval command |
| "Re-run eval until favored agent wins" | Eval drift = unethical; rank once, document, proceed |

## Output Contract

Done when:
- Session ID resolved (latest or specified)
- Mode chosen: metric (eval cmd configured) or LLM judge (--judge flag or no eval cmd)
- All agents' results processed (worktree metric run OR diff comparison)
- Ranking output table: rank / agent / metric (or score) / delta / files
- Winner highlighted
- Session state updated to `evaluating`
- Next-step message includes `/hub:merge` instruction

## Examples

### Example 1 — Performance metric eval
- Input: Session with 3 agents optimizing a hot path; `eval_cmd` = "npm run bench"
- Action: `result_ranker.py --session ... --eval-cmd "npm run bench" --metric latency_ms --direction min`; rank by lower-is-better
- Output: Table with agent-2 winning at 142ms; user told to `/hub:merge --agent agent-2`

### Example 2 — LLM judge mode for content task
- Input: 3 agents wrote variations of a marketing blog; no metric cmd
- Action: For each agent, diff + read result post; LLM judge ranks on correctness / simplicity / quality
- Output: Ranked output with reasoning per criterion; winner named; merge instruction
