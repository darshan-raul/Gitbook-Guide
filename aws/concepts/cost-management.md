# Cost Management

Below is a **tree-style, exam-oriented breakdown** of **Cost Control & Cost Management topics**&#x20;

***

### 1. Cost Management Foundations

```
AWS Pricing Models
├─ On-Demand pricing
├─ Reserved Instances (RI)
│  ├─ Standard vs Convertible
│  ├─ Regional vs Zonal
│  ├─ Partial / All / No upfront
│  ├─ Instance size flexibility
│  └─ Scope: EC2, RDS, ElastiCache, Redshift
├─ Savings Plans
│  ├─ Compute Savings Plans
│  ├─ EC2 Instance Savings Plans
|  |- Database Savings plan
│  └─ Interaction with Spot & On-Demand
├─ Spot Instances
│  ├─ Spot pricing model
│  ├─ Interruption behavior
│  ├─ Spot Fleets & Capacity Pools
│  └─ When NOT to use Spot 
└─ Free tier & hidden cost traps
```

***

### 2. AWS Cost Visibility & Reporting

```
Billing & Cost Tools
├─ AWS Cost Explorer
│  ├─ Cost & Usage reports (CUR)
│  ├─ Dimension filters (service, account, tag)
│  ├─ RI & Savings Plan utilization
│  └─ Forecasting limitations
├─ Cost and Usage Report (CUR)
│  ├─ Hourly vs daily granularity
│  ├─ Athena + Glue analysis
│  ├─ Multi-account aggregation
│  └─ Chargeback / Showback models
├─ AWS Budgets
│  ├─ Cost budgets
│  ├─ Usage budgets
│  ├─ RI & Savings Plan budgets
│  └─ Alerts (SNS, ChatOps)
└─ Billing Console
   ├─ Linked accounts view
   └─ Consolidated billing
```

***

### 3. Cost Allocation & Governance (VERY IMPORTANT)

```
Cost Allocation
├─ Cost Allocation Tags
│  ├─ User-defined vs AWS-generated
│  ├─ Activation requirement
│  ├─ Tag inheritance pitfalls
│  └─ Enforcement using SCPs
├─ Account-based cost isolation
│  ├─ Prod / Non-Prod separation
│  ├─ Workload-based accounts
│  └─ Regulatory separation
├─ Chargeback / Showback
│  ├─ Business unit mapping
│  ├─ Product-based allocation
│  └─ Shared service cost distribution
└─ Tag enforcement strategies
   ├─ SCP deny without tags
   ├─ AWS Config rules
   └─ CI/CD tag injection
```

***

### 4. AWS Organizations & Consolidated Billing

```
Organizations & Billing
├─ Consolidated Billing
│  ├─ RI & Savings Plan sharing
│  ├─ Volume tier discount sharing
│  └─ Free tier sharing behavior
├─ Organizational Units (OU)
│  ├─ Billing isolation by OU
│  └─ Guardrails for cost control
├─ Service Control Policies (SCP)
│  ├─ Restrict expensive regions
│  ├─ Deny unsupported instance types
│  └─ Prevent unapproved services
└─ Multi-payer billing strategies
```

***

### 5. Compute Cost Optimization

```
EC2 Cost Optimization
├─ Right-sizing
│  ├─ CloudWatch metrics
│  ├─ Compute Optimizer
│  └─ Idle resource detection
├─ Auto Scaling
│  ├─ Predictive scaling
│  ├─ Scheduled scaling
│  └─ Mixed instance policies
├─ Spot + On-Demand + RI mix
│  ├─ Baseline vs burst capacity
│  ├─ Fault-tolerant workloads
│  └─ Spot interruption handling
├─ AMI & instance lifecycle
│  ├─ Zombie instances
│  └─ Orphaned resources
└─ Graviton (ARM) cost trade-offs
```

***

### 6. Storage Cost Optimization

```
Storage Costs
├─ S3 Cost Management
│  ├─ Storage classes
│  │  ├─ Standard
│  │  ├─ Intelligent-Tiering
│  │  ├─ IA / One Zone-IA
│  │  ├─ Glacier / Deep Archive
│  ├─ Lifecycle policies
│  ├─ Data retrieval costs
│  └─ Cross-region replication costs
├─ EBS Optimization
│  ├─ gp2 vs gp3
│  ├─ io1/io2 trade-offs
│  ├─ Unattached volumes
│  └─ Snapshot storage & retention
├─ EFS
│  ├─ Standard vs IA
│  └─ Lifecycle transitions
└─ Backup cost management
   ├─ AWS Backup plans
   ├─ Retention policies
   └─ Cross-account backups
```

***

### 7. Database Cost Optimization

```
Database Costs
├─ RDS
│  ├─ Instance sizing
│  ├─ Multi-AZ cost impact
│  ├─ Read replicas vs scaling up
│  ├─ Storage auto-scaling costs
│  └─ RI coverage
├─ Aurora
│  ├─ Serverless v1 vs v2
│  ├─ Storage auto-growth model
│  └─ Replica vs Multi-AZ trade-offs
├─ DynamoDB
│  ├─ On-Demand vs Provisioned
│  ├─ Auto scaling
│  ├─ Reserved capacity
│  └─ DAX cost justification
└─ ElastiCache
   ├─ Node sizing
   └─ RI usage
```

***

### 8. Network & Data Transfer Costs (EXAM FAVORITE)

```
Networking Costs
├─ Data Transfer
│  ├─ AZ-to-AZ costs
│  ├─ Inter-region costs
│  ├─ Internet egress
│  └─ VPC peering vs Transit Gateway
├─ NAT Gateway
│  ├─ Per-hour cost
│  ├─ Per-GB processing cost
│  └─ NAT vs Gateway Load Balancer
├─ Load Balancers
│  ├─ ALB LCU pricing
│  ├─ NLB vs ALB cost model
│  └─ Idle LBs
├─ CloudFront
│  ├─ Caching strategies
│  ├─ Origin fetch reduction
│  └─ Regional edge caches
└─ Private connectivity
   ├─ VPC Endpoints
   ├─ PrivateLink
   └─ Direct Connect trade-offs
```

***

### 9. Serverless Cost Optimization

```
Serverless Costs
├─ Lambda
│  ├─ Memory vs duration trade-off
│  ├─ Provisioned concurrency
│  ├─ Cold start cost implications
│  └─ VPC Lambda cost traps
├─ API Gateway
│  ├─ REST vs HTTP API pricing
│  └─ Caching costs
├─ Step Functions
│  ├─ Standard vs Express
│  └─ State transition pricing
└─ Event-driven architecture cost analysis
```

***

### 10. Monitoring, Alerts & Automation

```
Cost Controls & Automation
├─ Cost Anomaly Detection
├─ Budget alerts → SNS → Slack / Email
├─ Lambda auto-cleanup
│  ├─ Stop idle EC2
│  ├─ Delete unattached EBS
│  └─ Expire old snapshots
├─ Scheduled shutdowns
└─ CI/CD guardrails for cost
```

***

### 11. Migration & Architecture Trade-offs

```
Cost-Aware Architecture Decisions
├─ Lift-and-shift vs re-architecture
├─ Multi-region vs single-region
├─ Active-active vs active-passive DR
├─ Managed services vs self-managed
└─ Build vs buy cost evaluation
```

***

### 12. Exam-Style Scenario Thinking

```
How AWS Tests Cost Knowledge
├─ “Cheapest long-term option” vs flexibility
├─ “Minimize operational overhead” vs raw cost
├─ Enterprise-scale RI/SP planning
├─ Cost + compliance + performance trade-offs
└─ Multi-account governance scenarios
```

***

#### How I’d recommend you study this (based on your background)

* You already have **deep AWS experience**, so:
  * Focus on **RI vs Savings Plan edge cases**
  * **Network & data transfer costs**
  * **Governance via SCPs and tagging**
* Practice **architecture questions**, not definitions.

If you want, I can next:

* Convert this into a **1-week SAP cost-optimization study plan**
* Or give **10 brutal SAP-C02 cost-optimization scenario questions**
* Or map **each topic → typical exam trick**

Just tell me 👍
