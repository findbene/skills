# Trigger.dev Configuration Reference

## trigger.config.ts
```typescript
import { defineConfig } from "@trigger.dev/sdk/v3";

export default defineConfig({
  project: "proj_your_project_id",
  runtime: "node",
  machine: { preset: "small-1x" },
  maxDuration: 3600,
  dirs: ["./src/trigger"],
  retries: {
    enabledInDev: false,
    default: {
      maxAttempts: 3,
      factor: 2,
      minTimeoutInMs: 1000,
      maxTimeoutInMs: 30000,
    },
  },
});
```

## Environment Variables
```bash
# .env (local)
TRIGGER_SECRET_KEY=tr_dev_xxxxx

# Production
TRIGGER_SECRET_KEY=tr_prod_xxxxx
```

## Machine Presets

| Preset | CPU | RAM | Use Case |
|---|---|---|---|
| `micro` | 0.25 vCPU | 256 MB | Simple transforms |
| `small-1x` | 1 vCPU | 1 GB | Most tasks (default) |
| `small-2x` | 1 vCPU | 2 GB | Memory-intensive |
| `medium-1x` | 2 vCPU | 2 GB | CPU-intensive |
| `medium-2x` | 2 vCPU | 4 GB | Video processing |
| `large-1x` | 4 vCPU | 4 GB | Heavy compute |
| `large-2x` | 4 vCPU | 8 GB | Large model inference |

## Deployment
```bash
npx trigger.dev@beta deploy
npx trigger.dev@beta deploy --env production
```

## GitHub Actions CI/CD
```yaml
name: Deploy Trigger.dev Tasks
on:
  push:
    branches: [main]
    paths: ["src/trigger/**"]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: "20" }
      - run: npm ci
      - run: npx trigger.dev@beta deploy
        env:
          TRIGGER_ACCESS_TOKEN: ${{ secrets.TRIGGER_ACCESS_TOKEN }}
```
