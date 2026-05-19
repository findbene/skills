---
name: "secrets-vault-manager"
description: 'Use when the user asks to set up secret management infrastructure, integrate HashiCorp Vault, configure cloud secret sto. Triggers: "use secrets-vault-manager", "secrets vault manager", "secrets task.'
allowed-tools: Glob, Grep, Read
---

# Secrets Vault Manager

**Tier:** POWERFUL
**Category:** Engineering
**Domain:** Security / Infrastructure / DevOps

---

## Overview

Production secret infrastructure management for teams running HashiCorp Vault, cloud-native secret stores, or hybrid architectures. This skill covers policy authoring, auth method configuration, automated rotation, dynamic secrets, audit logging, and incident response.

**Distinct from env-secrets-manager** which handles local `.env` file hygiene and leak detection. This skill operates at the infrastructure layer — Vault clusters, cloud KMS, certificate authorities, and CI/CD secret injection.

### When to Use

- Standing up a new Vault cluster or migrating to a managed secret store
- Designing auth methods for services, CI runners, and human operators
- Implementing automated credential rotation (database, API keys, certificates)
- Auditing secret access patterns for compliance (SOC 2, ISO 27001, HIPAA)
- Responding to a secret leak that requires mass revocation
- Integrating secrets into Kubernetes workloads or CI/CD pipelines

---

## HashiCorp Vault Patterns

### Architecture Decisions

| Decision | Recommendation | Rationale |
|----------|---------------|-----------|
| Deployment mode | HA with Raft storage | No external dependency, built-in leader election |
| Auto-unseal | Cloud KMS (AWS KMS / Azure Key Vault / GCP KMS) | Eliminates manual unseal, enables automated restarts |
| Namespaces | One per environment (dev/staging/prod) | Blast-radius isolation, independent policies |
| Audit devices | File + syslog (dual) | Vault refuses requests if all audit devices fail — dual prevents outages |

### Auth Methods

**AppRole** — Machine-to-machine authentication for services and batch jobs.

```hcl
# Enable AppRole
path "auth/approle/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Application-specific role
vault write auth/approle/role/payment-service \
  token_ttl=1h \
  token_max_ttl=4h \
  secret_id_num_uses=1 \
  secret_id_ttl=10m \
  token_policies="payment-service-read"
```

**Kubernetes** — Pod-native authentication via service account tokens.

```hcl
vault write auth/kubernetes/role/api-server \
  bound_service_account_names=api-server \
  bound_service_account_namespaces=production \
  policies=api-server-secrets \
  ttl=1h
```

**OIDC** — Human operator access via SSO provider (Okta, Azure AD, Google Workspace).

```hcl
vault write auth/oidc/role/engineering \
  bound_audiences="vault" \
  allowed_redirect_uris="https://vault.example.com/ui/vault/auth/oidc/oidc/callback" \
  user_claim="email" \
  oidc_scopes="openid,profile,email" \
  policies="engineering-read" \
  ttl=8h
```

### Secret Engines

| Engine | Use Case | TTL Strategy |
|--------|----------|-------------|
| KV v2 | Static secrets (API keys, config) | Versioned, manual rotation |
| Database | Dynamic DB credentials | 1h default, 24h max |
| PKI | TLS certificates | 90d leaf certs, 5y intermediate CA |
| Transit | Encryption-as-a-service | Key rotation every 90d |
| SSH | Signed SSH certificates | 30m for interactive, 8h for automation |

### Policy Design

Follow least-privilege with path-based granularity:

```hcl
# payment-service-read policy
path "secret/data/production/payment/*" {
  capabilities = ["read"]
}

path "database/creds/payment-readonly" {
  capabilities = ["read"]
}

# Deny access to admin paths explicitly
path "sys/*" {
  capabilities = ["deny"]
}
```

**Policy naming convention:** `{service}-{access-level}` (e.g., `payment-service-read`, `api-gateway-admin`).

---

## Cloud Secret Store Integration

### Comparison Matrix

| Feature | AWS Secrets Manager | Azure Key Vault | GCP Secret Manager |
|---------|--------------------|-----------------|--------------------|
| Rotation | Built-in Lambda | Custom logic via Functions | Cloud Functions |
| Versioning | Automatic | Manual or automatic | Automatic |
| Encryption | AWS KMS (default or CMK) | HSM-backed | Google-managed or CMEK |
| Access control | IAM policies + resource policy | RBAC + Access Policies | IAM bindings |
| Cross-region | Replication supported | Geo-redundant by default | Replication supported |
| Audit | CloudTrail | Azure Monitor + Diagnostic Logs | Cloud Audit Logs |
| Pricing model | Per-secret + per-API call | Per-operation + per-key | Per-secret version + per-access |

### When to Use Which

- **AWS Secrets Manager**: RDS/Aurora credential rotation out of the box. Best when fully on AWS.
- **Azure Key Vault**: Certificate management strength. Required for Azure AD integrated workloads.
- **GCP Secret Manager**: Simplest API surface. Best for GKE-native workloads with Workload Identity.
- **HashiCorp Vault**: Multi-cloud, dynamic secrets, PKI, transit encryption. Best for complex or hybrid environments.

### SDK Access Patterns

**Principle:** Always fetch secrets at startup or via sidecar — never bake into images or config files.

```python
# AWS Secrets Manager pattern
import boto3, json

def get_secret(secret_name, region="us-east-1"):
    client = boto3.client("secretsmanager", region_name=region)
    response = client.get_secret_value(SecretId=secret_name)
    return json.loads(response["SecretString"])
```

```python
# GCP Secret Manager pattern
from google.cloud import secretmanager

def get_secret(project_id, secret_id, version="latest"):
    client = secretmanager.SecretManagerServiceClient()
    name = f"projects/{project_id}/secrets/{secret_id}/versions/{version}"
    response = client.access_secret_version(request={"name": name})
    return response.payload.data.decode("UTF-8")
```

```python
# Azure Key Vault pattern
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

def get_secret(vault_url, secret_name):
    credential = DefaultAzureCredential()
    client = SecretClient(vault_url=vault_url, credential=credential)
    return client.get_secret(secret_name).value
```

---

## Secret Rotation Workflows

### Rotation Strategy by Secret Type

| Secret Type | Rotation Frequency | Method | Downtime Risk |
|-------------|-------------------|--------|---------------|
| Database passwords | 30 days | Dual-account swap | Zero (A/B rotation) |
| API keys | 90 days | Generate new, deprecate old | Zero (overlap window) |
| TLS certificates | 60 days before expiry | ACME or Vault PKI | Zero (graceful reload) |
| SSH keys | 90 days | Vault-signed certificates | Zero (CA-based) |
| Service tokens | 24 hours | Dynamic generation | Zero (short-lived) |
| Encryption keys | 90 days | Key versioning (rewrap) | Zero (version coexistence) |

### Database Credential Rotation (Dual-Account)

1. Two database accounts exist: `app_user_a` and `app_user_b` → verify: step output matches expected outcome
2. Application currently uses `app_user_a` → verify: step output matches expected outcome
3. Rotation rotates `app_user_b` password, updates secret store → verify: step output matches expected outcome
4. Application switches to `app_user_b` on next credential fetch → verify: step output matches expected outcome
5. After grace period, `app_user_a` password is rotated → verify: step output matches expected outcome
6. Cycle repeats → verify: step output matches expected outcome

### API Key Rotation (Overlap Window)

1. Generate new API key with provider → verify: output file exists + no syntax error
2. Store new key in secret store as `current`, move old to `previous` → verify: step output matches expected outcome
3. Deploy applications — they read `current` → verify: file readable + content matches expected shape
4. After all instances restarted (or TTL expired), revoke `previous` → verify: step output matches expected outcome
5. Monitoring confirms zero usage of old key before revocation → verify: step output matches expected outcome

---

## Dynamic Secrets

Dynamic secrets are generated on-demand with automatic expiration. Prefer dynamic secrets over static credentials wherever possible.

### Database Dynamic Credentials (Vault)

```hcl
# Configure database engine
vault write database/config/postgres \
  plugin_name=postgresql-database-plugin \
  connection_url="postgresql://{{username}}:{{password}}@db.example.com:5432/app" \
  allowed_roles="app-readonly,app-readwrite" \
  username="vault_admin" \
  password="<admin-password>"

# Create role with TTL
vault write database/roles/app-readonly \
  db_name=postgres \
  creation_statements="CREATE ROLE \"{{name}}\" WITH LOGIN PASSWORD '{{password}}' VALID UNTIL '{{expiration}}'; GRANT SELECT ON ALL TABLES IN SCHEMA public TO \"{{name}}\";" \
  default_ttl=1h \
  max_ttl=24h
```

### Cloud IAM Dynamic Credentials

Vault can generate short-lived AWS IAM credentials, Azure service principal passwords, or GCP service account keys — eliminating long-lived cloud credentials entirely.

### SSH Certificate Authority

Replace SSH key distribution with a Vault-signed certificate model:

1. Vault acts as SSH CA → verify: step output matches expected outcome
2. Users/machines request signed certificates with short TTL (30 min) → verify: step output matches expected outcome
3. SSH servers trust the CA public key — no `authorized_keys` management → verify: step output matches expected outcome
4. Certificates expire automatically — no revocation needed for normal operations → verify: step output matches expected outcome

---

## Audit Logging

### What to Log

| Event | Priority | Retention |
|-------|----------|-----------|
| Secret read access | HIGH | 1 year minimum |
| Secret creation/update | HIGH | 1 year minimum |
| Auth method login | MEDIUM | 90 days |
| Policy changes | CRITICAL | 2 years (compliance) |
| Failed access attempts | CRITICAL | 1 year |
| Token creation/revocation | MEDIUM | 90 days |
| Seal/unseal operations | CRITICAL | Indefinite |

### Anomaly Detection Signals

- Secret accessed from new IP/CIDR range
- Access volume spike (>3x baseline for a path)
- Off-hours access for human auth methods
- Service accessing secrets outside its policy scope (denied requests)
- Multiple failed auth attempts from single source
- Token created with unusually long TTL

### Compliance Reporting

Generate periodic reports covering:

1. **Access inventory** — Which identities accessed which secrets, when → verify: step output matches expected outcome
2. **Rotation compliance** — Secrets overdue for rotation → verify: step output matches expected outcome
3. **Policy drift** — Policies modified since last review → verify: step output matches expected outcome
4. **Orphaned secrets** — Secrets with no recent access (>90 days) → verify: step output matches expected outcome

Use `audit_log_analyzer.py` to parse Vault or cloud audit logs for these signals.

---

## Emergency Procedures

### Secret Leak Response (Immediate)

**Time target: Contain within 15 minutes of detection.**

1. **Identify scope** — Which secret(s) leaked, where (repo, log, error message, third party) → verify: step output matches expected outcome
2. **Revoke immediately** — Rotate the compromised credential at the source (provider API, Vault, cloud SM) → verify: step output matches expected outcome
3. **Invalidate tokens** — Revoke all Vault tokens that accessed the leaked secret → verify: all checks pass
4. **Audit blast radius** — Query audit logs for usage of the compromised secret in the exposure window → verify: findings count > 0 OR clean signal returned
5. **Notify stakeholders** — Security team, affected service owners, compliance (if PII/regulated data) → verify: step output matches expected outcome
6. **Post-mortem** — Document root cause, update controls to prevent recurrence → verify: step output matches expected outcome

### Vault Seal Operations

**When to seal:** Active security incident affecting Vault infrastructure, suspected key compromise.

**Sealing** stops all Vault operations. Use only as last resort.

**Unseal procedure:**
1. Gather quorum of unseal key holders (Shamir threshold) → verify: step output matches expected outcome
2. Or confirm auto-unseal KMS key is accessible → verify: step output matches expected outcome
3. Unseal via `vault operator unseal` or restart with auto-unseal → verify: step output matches expected outcome
4. Verify audit devices reconnected
5. Check active leases and token validity → verify: all checks pass

See `references/emergency_procedures.md` for complete playbooks.

---

## CI/CD Integration

### Vault Agent Sidecar (Kubernetes)

Vault Agent runs alongside application pods, handles authentication and secret rendering:

```yaml
# Pod annotation for Vault Agent Injector
annotations:
  vault.hashicorp.com/agent-inject: "true"
  vault.hashicorp.com/role: "api-server"
  vault.hashicorp.com/agent-inject-secret-db: "database/creds/app-readonly"
  vault.hashicorp.com/agent-inject-template-db: |
    {{- with secret "database/creds/app-readonly" -}}
    postgresql://{{ .Data.username }}:{{ .Data.password }}@db:5432/app
    {{- end }}
```

### External Secrets Operator (Kubernetes)

For teams preferring declarative GitOps over agent sidecars:

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: api-credentials
spec:
  refreshInterval: 1h
  secretStoreRef:
    name: vault-backend
    kind: ClusterSecretStore
  target:
    name: api-credentials
  data:
    - secretKey: api-key
      remoteRef:
        key: secret/data/production/api
        property: key
```

### GitHub Actions OIDC

Eliminate long-lived secrets in CI by using OIDC federation:

```yaml
- name: Authenticate to Vault
  uses: hashicorp/vault-action@v2
  with:
    url: https://vault.example.com
    method: jwt
    role: github-ci
    jwtGithubAudience: https://vault.example.com
    secrets: |
      secret/data/ci/deploy api_key | DEPLOY_API_KEY ;
      secret/data/ci/deploy db_password | DB_PASSWORD
```

---

## Anti-Patterns

| Anti-Pattern | Risk | Correct Approach |
|-------------|------|-----------------|
| Hardcoded secrets in source code | Leak via repo, logs, error output | Fetch from secret store at runtime |
| Long-lived static tokens (>30 days) | Stale credentials, no accountability | Dynamic secrets or short TTL + rotation |
| Shared service accounts | No audit trail per consumer | Per-service identity with unique credentials |
| No rotation policy | Compromised creds persist indefinitely | Automated rotation on schedule |
| Secrets in environment variables on CI | Visible in build logs, process table | Vault Agent or OIDC-based injection |
| Single unseal key holder | Bus factor of 1, recovery blocked | Shamir split (3-of-5) or auto-unseal |
| No audit device configured | Zero visibility into access | Dual audit devices (file + syslog) |
| Wildcard policies (`path "*"`) | Over-permissioned, violates least privilege | Explicit path-based policies per service |

---

## Tools

| Script | Purpose |
|--------|---------|
| `vault_config_generator.py` | Generate Vault policy and auth config from application requirements |
| `rotation_planner.py` | Create rotation schedule from a secret inventory file |
| `audit_log_analyzer.py` | Analyze audit logs for anomalies and compliance gaps |

---

## Cross-References

- **env-secrets-manager** — Local `.env` file hygiene, leak detection, drift awareness
- **senior-secops** — Security operations, incident response, threat modeling
- **ci-cd-pipeline-builder** — Pipeline design where secrets are consumed
- **docker-development** — Container secret injection patterns
- **helm-chart-builder** — Kubernetes secret management in Helm charts

## When NOT to use

- Task is unrelated to secrets vault manager — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Secrets Vault Manager needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for secrets vault manager
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving secrets vault manager
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
