# Prompt Engineering Techniques Reference

## Core Techniques

### Chain-of-Thought (CoT)
```python
# Zero-shot CoT
prompt = """
Analyze whether this script will go viral.
Script: {script}

Think through this step by step:
1. Hook: Does it stop the scroll in 3 seconds?
2. Retention: Is there a reason to keep watching?
3. Shareability: Would someone send this to a friend?
4. Platform fit: Is the format right for YouTube Shorts?
5. Final verdict: Viral potential score 1-10 with reasoning.
"""

# Few-shot CoT (more reliable)
prompt = """
Analyze viral potential. Show reasoning before scoring.

Example:
Script: "I made $50,000 last month with one AI tool..."
Analysis:
- Hook: Specific dollar amount in first 5 words. Score: 9/10
- Retention: Curiosity gap created (which tool?). Score: 8/10
- Shareability: Aspirational content. Score: 7/10
Viral Score: 8/10

Now analyze:
Script: {script}
Analysis:
"""
```

### XML Tags for Claude (Highly Effective)
Claude follows XML-delimited instructions reliably:
```
SYSTEM:
<instructions>
- Score each dimension 1-10
- Identify the primary weakness
- Suggest one specific improvement
</instructions>

<output_format>
<scores>
  <hook>[1-10]</hook>
  <curiosity>[1-10]</curiosity>
  <value>[1-10]</value>
</scores>
<weakness>[single sentence]</weakness>
<improvement>[single actionable suggestion]</improvement>
</output_format>

USER:
<script>
{script_text}
</script>
```

### Persona Prompting
```python
# Generic (weaker)
system = "You are an AI assistant. Answer marketing questions."

# Persona (stronger results)
system = """
You are Maya Chen, a performance marketing expert with 15 years at DTC brands.
You've managed $50M+ in ad spend across Meta, Google, and TikTok.

Your style:
- Direct and data-driven (cite metrics when possible)
- Skeptical of tactics without proven ROI
- Always ask about unit economics before recommending a channel
"""
```

---

## Structured Output

### JSON Mode (OpenAI / LiteLLM)
```python
from litellm import completion

response = completion(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "Extract lead data as JSON."},
        {"role": "user", "content": text}
    ],
    response_format={"type": "json_object"},
    temperature=0.1
)
data = json.loads(response.choices[0].message.content)
```

### Pydantic + Instructor (Typed Output)
```python
from pydantic import BaseModel, Field
from typing import Optional
import instructor
import anthropic

class QualityScore(BaseModel):
    hook_score: int = Field(ge=1, le=10)
    curiosity_score: int = Field(ge=1, le=10)
    value_score: int = Field(ge=1, le=10)
    loop_score: int = Field(ge=1, le=10)
    overall_score: float
    primary_failure: Optional[str] = None
    recommendation: str = Field(max_length=200)

client = instructor.from_anthropic(anthropic.Anthropic())

score = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=500,
    messages=[{"role": "user", "content": f"Score this script:\n\n{script_text}"}],
    response_model=QualityScore   # enforces schema automatically
)
```

---

## Prompt Caching (Claude — Cost Reduction)

```python
import anthropic

client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1000,
    system=[
        {
            "type": "text",
            "text": LARGE_SYSTEM_PROMPT_OR_KNOWLEDGE_BASE,  # 10K+ tokens
            "cache_control": {"type": "ephemeral"}           # cache for 5 minutes
        }
    ],
    messages=[{"role": "user", "content": user_question}]
)
# Cached tokens cost ~10% of normal input price after first call
```

When to cache:
- System prompts > 2,048 tokens
- Large few-shot example sets
- Knowledge base / RAG base context
- Growing conversation history (cache older turns)

---

## RAG Prompt Design

```python
def build_rag_prompt(query: str, retrieved_chunks: list[dict]) -> str:
    context = "\n\n---\n\n".join([
        f"Source: {chunk['source']}\n{chunk['content']}"
        for chunk in retrieved_chunks
        if chunk["similarity"] > 0.75  # only high-relevance chunks
    ])

    return f"""Answer based on the provided context.
If the answer isn't in the context, say "I don't have that information."

<context>
{context}
</context>

Question: {query}
Answer:"""
```

---

## Automated Prompt Evaluation

```python
import anthropic

client = anthropic.Anthropic()

def evaluate_prompt(prompt_template: str, test_cases: list[dict]) -> dict:
    results = []
    failures = []

    for test in test_cases:
        prompt = prompt_template.format(**test["input"])
        response = client.messages.create(
            model="claude-haiku-4-5",   # use cheap model for eval
            max_tokens=500,
            messages=[{"role": "user", "content": prompt}]
        )
        output = response.content[0].text

        # LLM-as-judge
        judgment = client.messages.create(
            model="claude-haiku-4-5",
            max_tokens=50,
            messages=[{"role": "user", "content": f"""Does this output meet the criteria?
Criteria: {test['criteria']}
Output: {output}
Answer with only: PASS or FAIL"""}]
        )
        passed = "PASS" in judgment.content[0].text
        results.append(passed)
        if not passed:
            failures.append({"input": test["input"], "output": output})

    return {
        "pass_rate": sum(results) / len(results),
        "passed": sum(results),
        "total": len(results),
        "failures": failures
    }
```
