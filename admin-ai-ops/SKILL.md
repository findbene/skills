---
name: admin-ai-ops
description: 'DevOps and infrastructure management specialist covering Terraform IaC, Kubernetes orchestration, CI/CD pipelines, Docker, secrets managemen. Triggers: "use admin-ai-ops", "admin ai ops", "admin task.'
allowed-tools: Glob, Grep, Read
---

# Admin AI Ops

Guide infrastructure, CI/CD, and DevOps tasks with AI-assisted automation.

## Infrastructure as Code (IaC)

### Terraform Module Management

Use modules for reusable infrastructure components. Remote backends for state management prevent conflicts when multiple engineers work on the same infrastructure.

```hcl
module "vpc" {
  source      = "./modules/vpc"
  cidr_block  = var.vpc_cidr
  environment = var.environment
}
```

- Version control all infrastructure definitions
- Apply least-privilege IAM policies — over-permissioned roles are the most common source of cloud security incidents

## Container Orchestration

### Kubernetes Operations
- Namespace isolation for multi-tenant workloads
- ResourceQuotas and LimitRanges for resource governance
- PodDisruptionBudgets for high availability
- Horizontal and vertical pod autoscaling

### Helm Chart Management
- Use `values.yaml` for environment-specific configurations
- Implement proper chart versioning
- Test charts with `helm lint` and `helm template`
- Use `helm diff` before upgrades — surprises in production are expensive

## CI/CD Pipeline Patterns

### GitHub Actions

```yaml
name: CI/CD Pipeline
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Cache dependencies
        uses: actions/cache@v4
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
      - run: npm ci
      - run: npm test
```

### Pipeline Stage Progression
1. **Build** — Compile and package artifacts → verify: step output matches expected outcome
2. **Test** — Unit, integration, and security tests → verify: all checks pass
3. **Staging** — Deploy to staging environment → verify: step output matches expected outcome
4. **Approval** — Manual or automated gates → verify: step output matches expected outcome
5. **Production** — Canary/blue-green deployment → verify: step output matches expected outcome

## GitOps Workflow

Declarative configurations in Git with pull-based deployments (ArgoCD, Flux). Automated drift detection ensures infrastructure matches its declared state — manual changes get reverted, which eliminates configuration drift over time.

## Secrets Management

Secrets committed to version control are the #1 cause of security breaches in cloud infrastructure. Use external secret managers (Vault, AWS Secrets Manager) with automatic rotation and least-privilege access.

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: app-secrets
spec:
  secretStoreRef:
    name: vault-backend
    kind: ClusterSecretStore
  target:
    name: app-secrets
  data:
    - secretKey: api-key
      remoteRef:
        key: secret/data/app
        property: api_key
```

## Best Practices

1. **Fail Fast** — Run quick validations early in pipelines to save time → verify: command exit code 0
2. **Parallelize** — Execute independent jobs concurrently → verify: command exit code 0
3. **Cache Aggressively** — Cache dependencies and build artifacts → verify: step output matches expected outcome
4. **Pin Versions** — Use specific versions for actions and images to prevent surprise breakages → verify: step output matches expected outcome
5. **Environment Parity** — Keep dev/staging/prod consistent to catch issues before production → verify: step output matches expected outcome
6. **Automate Rollbacks** — Implement automatic rollback on failures → verify: step output matches expected outcome
7. **Monitor Everything** — Integrate observability into pipelines → verify: dependency resolves + import works
8. **Document Runbooks** — Maintain operational documentation for incident response → verify: command exit code 0

## When NOT to use

- Application-layer feature code (use `backend-architect` or `frontend-developer`)
- One-off `kubectl` / `terraform apply` for an existing well-defined module — just run it
- Pure cloud cost optimization without infra change — use `cost-reducer`
- Application observability config (dashboards, alerts) — use `observability-designer`
- Security policy / threat modeling — use `senior-secops` or `cloud-security`

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll commit this secret just for testing" | Git history is forever; rotate immediately, never commit |
| "Skip `helm diff`, the change is small" | Small Helm changes routinely take down namespaces; diff is cheap |
| "Pin to `latest` for the GitHub Action" | Supply-chain risk + non-reproducible builds; always pin SHA or version |
| "Manual hotfix in prod, I'll IaC it later" | "Later" = configuration drift forever; codify before applying |

## Output Contract

Done when:
- Change expressed as code (Terraform module, Helm values, Action YAML) — no manual console clicks
- Pinned versions and digests for any external action/image
- Least-privilege IAM/RBAC scoped to what the workload actually needs
- Rollback path defined (Helm rollback, Terraform state, blue-green cutover)
- `helm diff` or `terraform plan` output reviewed before apply
- Runbook updated if the change is operationally novel

## Examples

### Example 1 — New service needs CI/CD + Kubernetes deploy
- Input: "Add CI/CD for a new Node service and deploy to our k8s cluster"
- Action: Write a GitHub Actions workflow (build → test → image push to ECR pinned by SHA) + Helm chart with values per env + ExternalSecret for runtime config; pinned action versions; `helm diff` step before apply
- Output: `.github/workflows/service.yml`, `charts/service/`, ExternalSecret manifest, rollback runbook in repo

### Example 2 — Terraform module for new VPC
- Input: "We need a new VPC for the staging environment"
- Action: Author a reusable `modules/vpc` with CIDR/AZ inputs, remote S3 backend with DynamoDB locking, IAM policy scoped to network actions only, `terraform plan` reviewed
- Output: Module + `envs/staging/main.tf` reference, plan output attached to PR, drift-detection alert wired
