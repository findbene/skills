# Services, SaaS & FinOps

## SaaS Audit Framework

### Monthly SaaS Review Process
1. Pull all software subscriptions from credit card + bank statements
2. Check last login date for every tool (> 90 days = candidate for removal)
3. Identify duplicate tools (e.g., Slack + Teams + Discord all active)
4. Check actual seat utilization vs licensed seats
5. Consolidate: pick one tool per category, cancel rest

### Common SaaS Waste Patterns
| Category | Common Waste | Action |
|---|---|---|
| Project management | Jira + Linear + Asana all active | Pick one, migrate, cancel rest |
| Communication | Slack + Teams + Discord | Consolidate to one |
| Cloud storage | Dropbox + Google Drive + OneDrive | Consolidate |
| Monitoring | Datadog + New Relic + Grafana Cloud | Evaluate by use case |
| Email marketing | Mailchimp + ConvertKit + Beehiiv | Pick one per channel |
| Video conferencing | Zoom + Google Meet + Teams | Use what org already has |
| Analytics | Mixpanel + Amplitude + GA4 all configured | Consolidate |

---

## FinOps Principles

### FinOps Lifecycle
```
Inform -> Optimize -> Operate
  ^                      |
  +----------------------+
```

### The FinOps Pillars
1. **Visibility** — Everyone sees costs. Engineers see their team's spend in CI/CD.
2. **Accountability** — Teams own their cloud costs. No central cost blame.
3. **Optimization** — Continuous improvement, not one-time cuts.
4. **Culture** — Cost-consciousness is an engineering value, not just a finance problem.

### Cost Allocation Tags
```yaml
# Enforce these tags on ALL resources (AWS SCP / Azure Policy / GCP org policy)
tags:
  Environment: prod | staging | dev | test
  Team: platform | backend | frontend | data | ml
  Project: project-slug
  CostCenter: dept-code
  Owner: engineer@company.com
  AutoShutdown: "true" | "false"
```

### Chargeback vs Showback
- **Showback**: Teams see their costs but don't pay internally. Good starting point.
- **Chargeback**: Teams are actually billed internally. Stronger incentive but requires mature tagging.
- **Recommendation**: Start with showback, move to chargeback after 3 months of clean tagging.

---

## Negotiation Leverage

### When to Negotiate
- Annual contract renewal (30–60 days before expiry)
- When you have a competing offer in writing
- When you're a reference customer / case study candidate
- When you're about to churn (vendors often offer 20–40% retention discounts)

### Negotiation Playbook
1. **Get a competing quote first** — even if you don't intend to switch
2. **Ask for annual pricing** — typically 20–30% discount vs monthly
3. **Negotiate seats, not just price** — unused seats are pure waste
4. **Ask what's not on the price sheet** — free add-ons, extended trials, training credits
5. **Timing matters** — negotiate at end of vendor's quarter/fiscal year

### AWS Enterprise Discount Program (EDP)
- Available at $1M+/year spend
- Commit to 1–3 year spend target, get 5–20% discount
- Negotiate: Private Pricing Agreements (PPAs) for specific services
- AWS credits available for startups (AWS Activate), ISVs, and research

### Azure EA / Microsoft MCCA
- Enterprise Agreement: volume discounts for 3-year commitment
- MCCA (Microsoft Customer Agreement): more flexible than EA
- Azure Hybrid Benefit: apply existing Windows/SQL Server licenses

---

## Cost Monitoring Tooling

### AWS Native
```bash
# AWS Cost Explorer CLI
aws ce get-cost-and-usage \
  --time-period Start=2026-01-01,End=2026-02-01 \
  --granularity MONTHLY \
  --metrics "BlendedCost" \
  --group-by Type=DIMENSION,Key=SERVICE

# Set a budget alert
aws budgets create-budget \
  --account-id $ACCOUNT_ID \
  --budget '{"BudgetName":"monthly-limit","BudgetLimit":{"Amount":"300","Unit":"USD"},"TimeUnit":"MONTHLY","BudgetType":"COST"}' \
  --notifications-with-subscribers '[{"Notification":{"NotificationType":"ACTUAL","ComparisonOperator":"GREATER_THAN","Threshold":80},"Subscribers":[{"SubscriptionType":"EMAIL","Address":"you@example.com"}]}]'
```

### Open Source FinOps Tools
| Tool | Purpose | Cost |
|---|---|---|
| **Infracost** | Cost estimate in CI/CD PR comments for Terraform changes | Free |
| **OpenCost** | Kubernetes cost allocation and monitoring | Free |
| **Cloud Custodian** | Policy-based cloud resource management (auto-cleanup idle resources) | Free |
| **Komiser** | Multi-cloud cost dashboard | Free (self-hosted) |
| **Karpenter** | Kubernetes node autoscaling (better than Cluster Autoscaler) | Free |

### Budget Enforcement Patterns
```python
# Lambda to stop non-critical instances when budget threshold hit
import boto3

def lambda_handler(event, context):
    # Triggered by CloudWatch Billing Alert
    ec2 = boto3.client('ec2')

    # Stop all dev instances
    response = ec2.describe_instances(
        Filters=[
            {'Name': 'tag:Environment', 'Values': ['dev', 'staging']},
            {'Name': 'instance-state-name', 'Values': ['running']}
        ]
    )

    instance_ids = [
        i['InstanceId']
        for r in response['Reservations']
        for i in r['Instances']
    ]

    if instance_ids:
        ec2.stop_instances(InstanceIds=instance_ids)
        print(f"Stopped {len(instance_ids)} dev instances due to budget alert")
```

---

## This Project's Cost Profile (AI Agency)

### Current Budget: $200-$300/month
| Service | Estimated Monthly Cost | Optimization |
|---|---|---|
| Anthropic API (Claude) | $20–$80 | Route simple tasks to Haiku; cache repeated prompts |
| OpenAI API | $10–$40 | Use for high-volume generation only (gpt-4o-mini) |
| ElevenLabs | $5–$22 | Free tier: 10K chars/month; upgrade only when volume justifies |
| Pexels / Storyblocks | $0 / variable | Pexels free tier first; Storyblocks only if free quota exhausted |
| fal.ai (Kling/Minimax) | $0.07–$0.84/video | Kling for quality, Minimax for volume; SaaS Scout decides weekly |
| Supabase | $0 | Free tier covers early stage |
| n8n (self-hosted Docker) | $0 | No SaaS cost |
| Metabase (self-hosted Docker) | $0 | No SaaS cost |
| **Total** | **~$50–$200** | **Well within $300 cap** |

### Cost Reduction Rules for This Project
1. Never use Kling ($0.84/video) for test runs — use PIL placeholder
2. Cache all LLM responses to Supabase with 24h TTL for identical prompts
3. Run batch generation overnight (off-peak API pricing where applicable)
4. Monitor fal.ai spend weekly — alert if > $50/month
