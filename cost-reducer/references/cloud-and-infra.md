# Cloud & Infrastructure Cost Reduction

## AWS Cost Patterns

### Compute (EC2 / ECS / Lambda)
- **Right-sizing**: Use AWS Compute Optimizer. Oversized instances are the #1 waste source.
  - t3.large → t3.medium: ~50% savings if CPU < 30% average
  - Graviton3 (c7g/m7g) instances: 20–40% cheaper than x86 equivalents
- **Spot Instances**: 60–90% off for fault-tolerant workloads (batch jobs, dev environments)
- **Reserved Instances / Savings Plans**:
  - Compute Savings Plans: 17–66% off, most flexible
  - 1-year no-upfront RI: ~40% savings vs on-demand
  - 3-year full-upfront RI: ~60% savings — only after 6+ months stable baseline
- **Lambda**: Watch for over-provisioned memory. Run AWS Lambda Power Tuning tool.
- **ECS Fargate**: Use Fargate Spot for non-critical containers (70% discount)

### Storage (S3 / EBS / EFS)
- **S3 Intelligent-Tiering**: Auto-moves objects to cheaper tiers. Break-even at ~128KB+ objects accessed < monthly.
- **S3 Lifecycle Policies**: Move to Glacier after 90 days of inactivity. Glacier Deep Archive: $0.00099/GB/month.
- **EBS**: Delete unattached volumes (common waste). Snapshot old AMIs. gp3 is cheaper than gp2 with same or better performance.
- **EFS**: Avoid unless you need shared filesystem. S3 + application caching is usually cheaper.

### Data Transfer (Hidden Cost)
- **Cross-region transfer**: $0.02/GB both ways. Co-locate services in same region where possible.
- **NAT Gateway**: $0.045/GB processed. Use VPC Endpoints for S3/DynamoDB (free within VPC).
- **CloudFront**: Caches content at edge, reduces origin data transfer. Often cheaper than serving from EC2.
- **Inter-AZ traffic**: $0.01/GB each way. Place tightly-coupled services in same AZ.

### Database (RDS / DynamoDB / ElastiCache)
- **RDS**: Multi-AZ doubles cost — use only in production. Aurora Serverless v2 scales to zero.
- **DynamoDB**: On-demand pricing is 6–7x more expensive than provisioned at steady load. Switch at 20%+ utilization.
- **RDS Reserved Instances**: 40–60% off 1-year, 60–70% off 3-year.
- **Read Replicas**: Only add when actual read load justifies it. Each replica = full instance cost.

---

## Azure Cost Patterns

### Compute (VMs / AKS / Functions)
- **Azure Advisor**: Built-in right-sizing recommendations. Check weekly.
- **Spot VMs**: 60–90% discount. Ideal for batch jobs and dev/test.
- **Reserved Instances**: 1-year (30–40% off) or 3-year (50–60% off).
- **Azure Hybrid Benefit**: If you have Windows Server / SQL Server licenses, apply to VMs (up to 40% savings).
- **B-series burstable VMs**: B1s/B2s for dev environments, low-traffic APIs.

### Storage
- **Blob Storage Tiers**: Hot → Cool → Archive. Archive: $0.00099/GB/month but rehydration fee.
- **Lifecycle Management Policies**: Auto-tier based on last access date.
- **Managed Disks**: Delete unattached disks. Use Standard HDD for dev, Premium SSD only for prod I/O-intensive.

### Networking
- **Egress from Azure**: $0.087/GB first 10TB. Use Azure CDN to reduce origin requests.
- **VNet Peering**: Cheaper than VPN Gateway for same-region connectivity.
- **Private Endpoints**: Reduce public egress costs for PaaS services.

---

## GCP Cost Patterns

### Compute (GCE / GKE / Cloud Run)
- **Committed Use Discounts**: 1-year (20–28%), 3-year (30–50%). Flexible CUDs apply across instance families.
- **Preemptible / Spot VMs**: 60–91% discount.
- **Cloud Run**: Scales to zero — best for sporadic workloads. Only pay per request.
- **Sustained Use Discounts**: Automatic 20–30% discount when running > 25% of month. No action needed.

### Storage
- **Cloud Storage Nearline**: Access < monthly. $0.01/GB/month (vs $0.02 Standard).
- **Coldline / Archive**: Long-term backups. Archive: $0.0012/GB/month.
- **Object Lifecycle Management**: Auto-delete or archive old versions.

---

## Universal Infrastructure Savings

### Identify Idle/Zombie Resources
```bash
# AWS: Find unattached EBS volumes
aws ec2 describe-volumes --filters Name=status,Values=available --query 'Volumes[*].{ID:VolumeId,Size:Size,Created:CreateTime}'

# AWS: Find unused Elastic IPs
aws ec2 describe-addresses --query 'Addresses[?AssociationId==null]'

# AWS: Find stopped instances (still paying for EBS)
aws ec2 describe-instances --filters Name=instance-state-name,Values=stopped
```

### Tagging Strategy for Cost Allocation
```
Required tags (enforce via AWS SCPs / Azure Policy):
  Environment: prod | staging | dev | test
  Owner: team-name or email
  CostCenter: business-unit-code
  Project: project-name
  AutoShutdown: true | false
```

### Dev Environment Automation
- Shut down dev/staging environments outside business hours: save 65% (16 hours off per day)
- AWS Instance Scheduler or Azure Start/Stop VMs solution
- Lambda/Function triggered by cron: `stop all instances tagged AutoShutdown=true at 8pm`

### Monitoring for Cost Anomalies
- AWS: Set billing alerts at 80% and 100% of budget. Use Cost Anomaly Detection.
- Azure: Cost alerts + Azure Cost Management budgets.
- GCP: Budgets & Alerts in Billing console. Pub/Sub notifications for programmatic response.
