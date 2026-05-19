---
name: observability-incident
description: 'Production observability and incident management specialist for metrics, logs, traces, alerting, and post-mortem. Triggers: "use observability-incident", "observability incident", "observability task.'
allowed-tools: Glob, Grep, Read
---

# Observability & Incident Management

Build observable systems with proper instrumentation, alerting, and incident management.

## The Three Pillars

### 1. Metrics

**RED Method (for services):** Rate, Errors, Duration
**USE Method (for resources):** Utilization, Saturation, Errors

```typescript
import { Counter, Histogram, Gauge, Registry } from 'prom-client';

const httpRequestsTotal = new Counter({
  name: 'http_requests_total',
  help: 'Total HTTP requests',
  labelNames: ['method', 'path', 'status'],
});

const httpRequestDuration = new Histogram({
  name: 'http_request_duration_seconds',
  help: 'HTTP request latency',
  labelNames: ['method', 'path'],
  buckets: [0.01, 0.05, 0.1, 0.5, 1, 2, 5, 10],
});
```

### 2. Structured Logging

```typescript
import pino from 'pino';

const logger = pino({
  level: process.env.LOG_LEVEL || 'info',
  redact: ['password', 'token', 'authorization', 'cookie']
});

logger.info({ event: 'order_created', orderId: order.id, total: order.total }, 'New order created');
```

**Log Levels:** TRACE, DEBUG, INFO, WARN, ERROR, FATAL

Use `AsyncLocalStorage` for automatic request context (requestId, traceId, userId) in all log entries.

### 3. Distributed Tracing (OpenTelemetry)

```typescript
import { NodeSDK } from '@opentelemetry/sdk-node';
import { getNodeAutoInstrumentations } from '@opentelemetry/auto-instrumentations-node';

const sdk = new NodeSDK({
  serviceName: process.env.SERVICE_NAME,
  traceExporter: new OTLPTraceExporter({ url: process.env.OTEL_EXPORTER_ENDPOINT }),
  instrumentations: [getNodeAutoInstrumentations()]
});
sdk.start();
```

Create manual spans with `tracer.startActiveSpan()`, set attributes, record exceptions, and always call `span.end()`.

## Alerting

### Prometheus Alert Rules
```yaml
groups:
  - name: application
    rules:
      - alert: HighErrorRate
        expr: sum(rate(http_requests_total{status=~"5.."}[5m])) / sum(rate(http_requests_total[5m])) > 0.05
        for: 5m
        labels: { severity: critical }
      - alert: HighLatency
        expr: histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m])) > 1
        for: 10m
        labels: { severity: warning }
```

Route critical alerts to PagerDuty, warnings to Slack.

## Incident Management

### Severity Levels
- **SEV1:** Critical — major customer impact, immediate response
- **SEV2:** High — significant impact, respond within 1 hour
- **SEV3:** Medium — limited impact, respond within 4 hours

### Incident Lifecycle
`detected → investigating → identified → monitoring → resolved`

Create war rooms for SEV1-2. Track timeline entries. Notify stakeholders on status changes.

### Post-Incident Review
Document: root cause, contributing factors, impact (customers affected, revenue), action items with owners/due dates, what worked, what didn't, lucky breaks.

## Best Practices

1. Structured logging with JSON and consistent fields → verify: step output matches expected outcome
2. Correlation IDs to trace requests across services → verify: step output matches expected outcome
3. Alert on symptoms, not just raw metrics → verify: step output matches expected outcome
4. Maintain runbooks for documented response procedures → verify: command exit code 0
5. Test alerts regularly to verify they work
6. Use metrics for capacity planning and forecasting → verify: step output matches expected outcome

## When NOT to use

- Task is unrelated to observability incident — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Observability Incident needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for observability incident
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving observability incident
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
