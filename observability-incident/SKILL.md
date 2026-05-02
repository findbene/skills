---
name: observability-incident
description: "Production observability and incident management specialist for metrics, logs, traces, alerting, and post-mortems using Prometheus, Grafana, and OpenTelemetry. Use this skill any time monitoring needs to be set up, alerting rules need to be configured, structured logging needs to be implemented, distributed tracing needs to be added, or incident response workflows need to be built. Trigger immediately on: \"monitoring\", \"observability\", \"alerting\", \"Prometheus\", \"Grafana\", \"OpenTelemetry\", \"structured logging\", \"distributed tracing\", \"incident response\", \"post-mortem\", \"on-call\", \"error rate\", \"metrics\", \"dashboards\". If someone says \"how do I know when my app is down?\" or \"set up monitoring\" this skill MUST trigger."
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

1. Structured logging with JSON and consistent fields
2. Correlation IDs to trace requests across services
3. Alert on symptoms, not just raw metrics
4. Maintain runbooks for documented response procedures
5. Test alerts regularly to verify they work
6. Use metrics for capacity planning and forecasting
