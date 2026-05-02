---
name: admin-ai-ops
description: "DevOps and infrastructure management specialist covering Terraform IaC, Kubernetes orchestration, CI/CD pipelines, Docker, secrets management, and cloud platform operations. Use this skill any time infrastructure needs to be provisioned, CI/CD pipelines need to be built, containers need to be orchestrated, or cloud resources need to be managed. Trigger immediately on: \"Terraform\", \"Kubernetes\", \"CI/CD\", \"infrastructure\", \"Docker\", \"GitHub Actions\", \"secrets management\", \"cloud deployment\", \"k8s\", \"container\", \"IaC\", \"helm chart\", \"cloud ops\", \"provisioning\". If someone says \"set up our infrastructure\" or \"build a CI/CD pipeline\" this skill MUST trigger."
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
1. **Build** — Compile and package artifacts
2. **Test** — Unit, integration, and security tests
3. **Staging** — Deploy to staging environment
4. **Approval** — Manual or automated gates
5. **Production** — Canary/blue-green deployment

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

1. **Fail Fast** — Run quick validations early in pipelines to save time
2. **Parallelize** — Execute independent jobs concurrently
3. **Cache Aggressively** — Cache dependencies and build artifacts
4. **Pin Versions** — Use specific versions for actions and images to prevent surprise breakages
5. **Environment Parity** — Keep dev/staging/prod consistent to catch issues before production
6. **Automate Rollbacks** — Implement automatic rollback on failures
7. **Monitor Everything** — Integrate observability into pipelines
8. **Document Runbooks** — Maintain operational documentation for incident response
