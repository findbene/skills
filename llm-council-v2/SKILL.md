---
name: llm-council-v2
description: Run any high-stakes question, idea, or decision through a council of 5 AI advisors who independently analyze it, peer-review each other anonymously, and synthesize a final verdict. Adapted from Karpathy's LLM Council methodology. MANDATORY TRIGGERS - "council this", "run the council", "war room this", "pressure-test this", "stress-test this", "debate this". STRONG TRIGGERS (when paired with a real tradeoff) - "should I X or Y", "which option", "what would you do", "is this the right move", "validate this", "get multiple perspectives", "I can't decide", "I'm torn between". Do NOT trigger on simple yes/no questions, factual lookups, or casual "should I" without a meaningful tradeoff. DO trigger when the user presents a genuine decision with stakes, multiple options, and context that suggests they want it pressure-tested from multiple angles.
license: MIT
---

# LLM Council

You ask one AI a question, you get one answer. That answer might be great. It might be mid. You have no way to tell because you only saw one perspective.

The council fixes this. It runs your question through 5 independent advisors, each thinking from a fundamentally different angle. Then they review each other's work. Then a chairman synthesizes everything into a final recommendation that tells you where the advisors agree, where they clash, and what you should actually do.

Adapted from Andrej Karpathy's LLM Council. He dispatches queries to multiple models, has them peer-review each other anonymously, then a chairman produces the final answer. We do the same thing inside Claude using sub-agents with different thinking lenses instead of different models.

---

## When to Run the Council

The council is for questions where being wrong is expensive.

**Good council questions:**
- "Should I launch a $97 workshop or a $497 course?"
- "Which of these 3 positioning angles is strongest?"
- "I'm thinking of pivoting from X to Y. Am I crazy?"
- "Here's my landing page copy. What's weak?"
- "Should I hire a VA or build an automation first?"

**Bad council questions:**
- "What's the capital of France?" (one right answer, no need for perspectives)
- "Write me a tweet" (creation task, not a decision)
- "Summarize this article" (processing task, not judgment)

The council shines when there's genuine uncertainty and the cost of a bad call is high. If you already know the answer and just want validation, the council will likely tell you things you don't want to hear. That's the point.

---

## How to Run a Council Session

A session is a 6-step pipeline:

1. **Frame the question** with workspace context enrichment
2. **Convene the council** — spawn all 5 advisors as sub-agents in parallel
3. **Peer review** — anonymize responses, spawn 5 reviewers in parallel
4. **Chairman synthesis** — produce final verdict
5. **Generate HTML report** — `council-report-[timestamp].html`
6. **Save full transcript** — `council-transcript-[timestamp].md`

Full step-by-step instructions, prompt templates, and output formats live in `references/process.md`. **Read it before running a session.**

---

## The Five Advisors

| Advisor | Lens |
|---------|------|
| The Contrarian | Finds fatal flaws, missing pieces, what will fail |
| The First Principles Thinker | "What are we actually trying to solve?" Strips assumptions |
| The Expansionist | Upside everyone's missing, adjacent opportunity, undervalued angles |
| The Outsider | Zero context. Catches curse-of-knowledge. Fresh eyes |
| The Executor | "OK but what do you do Monday morning?" Feasibility, fastest path |

These five create three productive tensions: Contrarian vs Expansionist (downside vs upside), First Principles vs Executor (rethink vs ship), Outsider in the middle keeping everyone honest.

Full advisor briefs in `references/advisors.md`. Each advisor sub-agent receives its full description from there.

---

## Critical Rules

- **Spawn all 5 advisors in parallel.** Sequential wastes time and lets earlier responses leak into later ones.
- **Anonymize for peer review.** If reviewers know which advisor said what, they defer to thinking styles instead of evaluating on merit. Randomize A–E mapping.
- **The chairman can disagree with the majority.** If 4-of-5 say "do it" but the 1 dissenter has the strongest reasoning, side with the dissenter and explain why.
- **Don't council trivial questions.** One right answer? Just answer it.
- **The visual report matters.** Most users scan the report, not the transcript. Make it clean.

---

## Reference Files

- `references/advisors.md` — Full briefs for each of the five advisors (passed verbatim into sub-agent prompts)
- `references/process.md` — Step-by-step pipeline, prompt templates for advisor / reviewer / chairman, output spec for HTML report and transcript
- `references/example.md` — Worked example: counciling a $297 course decision, all 5 advisor responses, full chairman verdict

---

## Output

Every session produces two files in the user's workspace:

```
council-report-[timestamp].html    # visual report — what user reads
council-transcript-[timestamp].md  # full transcript — for reference
```

Open the HTML after generating it.
