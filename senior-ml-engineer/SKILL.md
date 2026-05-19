---
name: "senior-ml-engineer"
description: "ML engineering skill for productionizing models, building MLOps pipelines, and integrating LLMs. Triggers: 'use senior-ml-engineer', 'senior ml engineer', 'senior-ml-engineer task'."
allowed-tools: Bash, Glob, Grep, Read
triggers:
  - MLOps pipeline
  - model deployment
  - feature store
  - model monitoring
  - drift detection
  - RAG system
  - LLM integration
  - model serving
  - A/B testing ML
  - automated retraining
---

# Senior ML Engineer

Production ML engineering patterns for model deployment, MLOps infrastructure, and LLM integration.

---

## Table of Contents

- [Model Deployment Workflow](#model-deployment-workflow)
- [MLOps Pipeline Setup](#mlops-pipeline-setup)
- [LLM Integration Workflow](#llm-integration-workflow)
- [RAG System Implementation](#rag-system-implementation)
- [Model Monitoring](#model-monitoring)
- [Reference Documentation](#reference-documentation)
- [Tools](#tools)

---

## Model Deployment Workflow

Deploy a trained model to production with monitoring:

1. Export model to standardized format (ONNX, TorchScript, SavedModel) → verify: step output matches expected outcome
2. Package model with dependencies in Docker container → verify: step output matches expected outcome
3. Deploy to staging environment → verify: step output matches expected outcome
4. Run integration tests against staging → verify: command exit code 0
5. Deploy canary (5% traffic) to production → verify: step output matches expected outcome
6. Monitor latency and error rates for 1 hour → verify: step output matches expected outcome
7. Promote to full production if metrics pass → verify: step output matches expected outcome
8. **Validation:** p95 latency < 100ms, error rate < 0.1% → verify: step output matches expected outcome

### Container Template

```dockerfile
FROM python:3.11-slim

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY model/ /app/model/
COPY src/ /app/src/

HEALTHCHECK CMD curl -f http://localhost:8080/health || exit 1

EXPOSE 8080
CMD ["uvicorn", "src.server:app", "--host", "0.0.0.0", "--port", "8080"]
```

### Serving Options

| Option | Latency | Throughput | Use Case |
|--------|---------|------------|----------|
| FastAPI + Uvicorn | Low | Medium | REST APIs, small models |
| Triton Inference Server | Very Low | Very High | GPU inference, batching |
| TensorFlow Serving | Low | High | TensorFlow models |
| TorchServe | Low | High | PyTorch models |
| Ray Serve | Medium | High | Complex pipelines, multi-model |

---

## MLOps Pipeline Setup

Establish automated training and deployment:

1. Configure feature store (Feast, Tecton) for training data → verify: step output matches expected outcome
2. Set up experiment tracking (MLflow, Weights & Biases) → verify: step output matches expected outcome
3. Create training pipeline with hyperparameter logging → verify: output exists + parses without error
4. Register model in model registry with version metadata → verify: step output matches expected outcome
5. Configure staging deployment triggered by registry events → verify: step output matches expected outcome
6. Set up A/B testing infrastructure for model comparison → verify: all checks pass
7. Enable drift monitoring with alerting → verify: step output matches expected outcome
8. **Validation:** New models automatically evaluated against baseline → verify: step output matches expected outcome

### Feature Store Pattern

```python
from feast import Entity, Feature, FeatureView, FileSource

user = Entity(name="user_id", value_type=ValueType.INT64)

user_features = FeatureView(
    name="user_features",
    entities=["user_id"],
    ttl=timedelta(days=1),
    features=[
        Feature(name="purchase_count_30d", dtype=ValueType.INT64),
        Feature(name="avg_order_value", dtype=ValueType.FLOAT),
    ],
    online=True,
    source=FileSource(path="data/user_features.parquet"),
)
```

### Retraining Triggers

| Trigger | Detection | Action |
|---------|-----------|--------|
| Scheduled | Cron (weekly/monthly) | Full retrain |
| Performance drop | Accuracy < threshold | Immediate retrain |
| Data drift | PSI > 0.2 | Evaluate, then retrain |
| New data volume | X new samples | Incremental update |

---

## LLM Integration Workflow

Integrate LLM APIs into production applications:

1. Create provider abstraction layer for vendor flexibility → verify: output file exists + no syntax error
2. Implement retry logic with exponential backoff → verify: step output matches expected outcome
3. Configure fallback to secondary provider → verify: step output matches expected outcome
4. Set up token counting and context truncation → verify: command exit code 0
5. Add response caching for repeated queries → verify: package installed + import succeeds
6. Implement cost tracking per request → verify: step output matches expected outcome
7. Add structured output validation with Pydantic → verify: package installed + import succeeds
8. **Validation:** Response parses correctly, cost within budget → verify: step output matches expected outcome

### Provider Abstraction

```python
from abc import ABC, abstractmethod
from tenacity import retry, stop_after_attempt, wait_exponential

class LLMProvider(ABC):
    @abstractmethod
    def complete(self, prompt: str, **kwargs) -> str:
        pass

@retry(stop=stop_after_attempt(3), wait=wait_exponential(min=1, max=10))
def call_llm_with_retry(provider: LLMProvider, prompt: str) -> str:
    return provider.complete(prompt)
```

### Cost Management

| Provider | Input Cost | Output Cost |
|----------|------------|-------------|
| GPT-4 | $0.03/1K | $0.06/1K |
| GPT-3.5 | $0.0005/1K | $0.0015/1K |
| Claude 3 Opus | $0.015/1K | $0.075/1K |
| Claude 3 Haiku | $0.00025/1K | $0.00125/1K |

---

## RAG System Implementation

Build retrieval-augmented generation pipeline:

1. Choose vector database (Pinecone, Qdrant, Weaviate) → verify: step output matches expected outcome
2. Select embedding model based on quality/cost tradeoff → verify: step output matches expected outcome
3. Implement document chunking strategy → verify: step output matches expected outcome
4. Create ingestion pipeline with metadata extraction → verify: output exists + parses without error
5. Build retrieval with query embedding → verify: step output matches expected outcome
6. Add reranking for relevance improvement → verify: step output matches expected outcome
7. Format context and send to LLM → verify: step output matches expected outcome
8. **Validation:** Response references retrieved context, no hallucinations → verify: step output matches expected outcome

### Vector Database Selection

| Database | Hosting | Scale | Latency | Best For |
|----------|---------|-------|---------|----------|
| Pinecone | Managed | High | Low | Production, managed |
| Qdrant | Both | High | Very Low | Performance-critical |
| Weaviate | Both | High | Low | Hybrid search |
| Chroma | Self-hosted | Medium | Low | Prototyping |
| pgvector | Self-hosted | Medium | Medium | Existing Postgres |

### Chunking Strategies

| Strategy | Chunk Size | Overlap | Best For |
|----------|------------|---------|----------|
| Fixed | 500-1000 tokens | 50-100 | General text |
| Sentence | 3-5 sentences | 1 sentence | Structured text |
| Semantic | Variable | Based on meaning | Research papers |
| Recursive | Hierarchical | Parent-child | Long documents |

---

## Model Monitoring

Monitor production models for drift and degradation:

1. Set up latency tracking (p50, p95, p99) → verify: step output matches expected outcome
2. Configure error rate alerting → verify: step output matches expected outcome
3. Implement input data drift detection → verify: step output matches expected outcome
4. Track prediction distribution shifts → verify: step output matches expected outcome
5. Log ground truth when available → verify: step output matches expected outcome
6. Compare model versions with A/B metrics → verify: step output matches expected outcome
7. Set up automated retraining triggers → verify: step output matches expected outcome
8. **Validation:** Alerts fire before user-visible degradation → verify: step output matches expected outcome

### Drift Detection

```python
from scipy.stats import ks_2samp

def detect_drift(reference, current, threshold=0.05):
    statistic, p_value = ks_2samp(reference, current)
    return {
        "drift_detected": p_value < threshold,
        "ks_statistic": statistic,
        "p_value": p_value
    }
```

### Alert Thresholds

| Metric | Warning | Critical |
|--------|---------|----------|
| p95 latency | > 100ms | > 200ms |
| Error rate | > 0.1% | > 1% |
| PSI (drift) | > 0.1 | > 0.2 |
| Accuracy drop | > 2% | > 5% |

---

## Reference Documentation

### MLOps Production Patterns

`references/mlops_production_patterns.md` contains:

- Model deployment pipeline with Kubernetes manifests
- Feature store architecture with Feast examples
- Model monitoring with drift detection code
- A/B testing infrastructure with traffic splitting
- Automated retraining pipeline with MLflow

### LLM Integration Guide

`references/llm_integration_guide.md` contains:

- Provider abstraction layer pattern
- Retry and fallback strategies with tenacity
- Prompt engineering templates (few-shot, CoT)
- Token optimization with tiktoken
- Cost calculation and tracking

### RAG System Architecture

`references/rag_system_architecture.md` contains:

- RAG pipeline implementation with code
- Vector database comparison and integration
- Chunking strategies (fixed, semantic, recursive)
- Embedding model selection guide
- Hybrid search and reranking patterns

---

## Tools

### Model Deployment Pipeline

```bash
python scripts/model_deployment_pipeline.py --model model.pkl --target staging
```

Generates deployment artifacts: Dockerfile, Kubernetes manifests, health checks.

### RAG System Builder

```bash
python scripts/rag_system_builder.py --config rag_config.yaml --analyze
```

Scaffolds RAG pipeline with vector store integration and retrieval logic.

### ML Monitoring Suite

```bash
python scripts/ml_monitoring_suite.py --config monitoring.yaml --deploy
```

Sets up drift detection, alerting, and performance dashboards.

---

## Tech Stack

| Category | Tools |
|----------|-------|
| ML Frameworks | PyTorch, TensorFlow, Scikit-learn, XGBoost |
| LLM Frameworks | LangChain, LlamaIndex, DSPy |
| MLOps | MLflow, Weights & Biases, Kubeflow |
| Data | Spark, Airflow, dbt, Kafka |
| Deployment | Docker, Kubernetes, Triton |
| Databases | PostgreSQL, BigQuery, Pinecone, Redis |

## When NOT to use

- Task is unrelated to senior ml engineer — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Senior Ml Engineer needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for senior ml engineer
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving senior ml engineer
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
