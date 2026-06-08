# AWS Cost Breakdown — Small Web Application

**Fellow:** Joshua Blessing | FE/24/161412576  
**Region:** US East (N. Virginia) — `us-east-1`  
**OS:** Linux (Ubuntu 22.04 LTS)  
**Pricing basis:** On-Demand (June 2026 public rates)
LINK:https://calculator.aws/#/estimate?trk=e8c13544-17d6-4cc0-abab-a4348449de09&sc_channel=ps&trk=e9b7f801-53c8-4092-a08e-2e0ffc7efe36&ef_id=:G:s&s_kwcid=AL!4422!3!!p!!o!!aws!487441811!1148991499401099&msclkid=2ff6cc7e08001b989e10a6591f6c69df&id=df6ae7c61f02d2742e3247c062e7886dd5c167e4s

---

## Compute — Amazon EC2

### Instance Selection Rationale

The application requires **2 vCPU and 4 GB RAM**. The `t3.medium` is the best-fit burstable general-purpose instance for a small web application with moderate, variable CPU usage.

| Attribute | Value |
|---|---|
| Instance family | T3 (burstable performance) |
| Instance size | medium |
| vCPU | 2 |
| Memory | 4 GB |
| Network | Up to 5 Gbps |
| EBS-optimised | Yes (default) |

### EC2 Pricing

| Pricing Model | $/hr | 730 hrs/month | Annual |
|---|---|---|---|
| On-Demand (Linux) | $0.0416 | **$30.37** | $364.42 |
| 1-Year Compute Savings Plan | $0.0291 | **$21.24** | $254.88 |
| 3-Year Compute Savings Plan | $0.0199 | **$14.53** | $174.36 |
| 1-Year Standard RI (no upfront) | $0.0260 | **$18.98** | $227.76 |
| 3-Year Standard RI (no upfront) | $0.0162 | **$11.83** | $141.96 |
| Spot Instance (variable) | ~$0.013 avg | ~$9.49 | ~$113.88 |

> Spot pricing varies in real time; the figure above is an approximate 6-month average for t3.medium in us-east-1.

---

## Storage — Amazon EBS (Root Volume)

| Volume type | Size | $/GB/month | Monthly |
|---|---|---|---|
| gp3 (General Purpose SSD v3) | 50 GB | $0.08 | **$4.00** |
| gp2 (comparison only) | 50 GB | $0.10 | $5.00 |

**gp3 advantages over gp2:**
- $0.08/GB vs $0.10/GB — 20% cheaper
- Baseline 3,000 IOPS included at no extra charge (gp2: 100–3,000 IOPS, burst dependent)
- Throughput: 125 MB/s baseline (upgradeable to 1,000 MB/s separately if needed)

---

## Storage — Amazon S3 (Object Storage)

### Storage Class: S3 Standard

| Line item | Volume | Rate | Monthly |
|---|---|---|---|
| Storage | 100 GB | $0.023/GB | $2.30 |
| PUT, COPY, POST, LIST requests | 10,000 | $0.005/1,000 | $0.05 |
| GET, SELECT, and all other requests | 100,000 | $0.0004/1,000 | $0.04 |
| Data retrieval (S3 Standard) | N/A | Free | $0.00 |
| **S3 Monthly Total** | | | **$2.39** |

### S3 Storage Class Comparison (if archiving is needed later)

| Class | Use case | $/GB/month |
|---|---|---|
| S3 Standard | Frequently accessed | $0.023 |
| S3 Standard-IA | Infrequently accessed | $0.0125 |
| S3 Glacier Instant Retrieval | Archives, ms retrieval | $0.004 |
| S3 Glacier Flexible | Deep archive, hours | $0.0036 |

---

## Data Transfer

### Outbound to Internet (Data Transfer OUT)

| Tier | Volume | Rate | Cost |
|---|---|---|---|
| First 10 GB/month | 10 GB | $0.00 | $0.00 |
| 10 GB – 100 GB | 90 GB | $0.09/GB | **$8.10** |
| 100 GB – 350 GB | 0 GB | $0.085/GB | $0.00 |
| **Total transfer cost** | | | **$8.10** |

### Inter-AZ Data Transfer (if multi-AZ architecture)

| Direction | Volume | Rate | Cost |
|---|---|---|---|
| AZ-A → AZ-B | 100 GB | $0.01/GB | $1.00 |
| AZ-B → AZ-A | 100 GB | $0.01/GB | $1.00 |
| **Inter-AZ total (200 GB, both directions)** | | | **$2.00** |

> Note: AWS charges **both** ingress and egress for inter-AZ traffic between EC2 instances. Plan load-balancer placement to minimise cross-AZ traffic.

---

## AWS Free Tier Reference

| Service | Free Tier Allowance | Duration |
|---|---|---|
| EC2 | 750 hrs/month t2.micro (Linux or Windows) | 12 months |
| EBS | 30 GB gp2 SSD | 12 months |
| S3 | 5 GB Standard storage, 20K GET, 2K PUT | 12 months |
| Data Transfer OUT | 100 GB/month from EC2 | 12 months |
| CloudFront | 1 TB data transfer + 10M requests | 12 months |

> ⚠️ t3.medium is **not** free-tier eligible. The free tier only covers t2.micro.

---

## Monthly Total — AWS (On-Demand, Linux, Single AZ)

| Component | Monthly Cost |
|---|---|
| EC2 t3.medium On-Demand | $30.37 |
| EBS gp3 50 GB | $4.00 |
| S3 Standard 100 GB | $2.39 |
| Data Transfer OUT 100 GB | $8.10 |
| **Grand Total** | **$44.86** |

## Monthly Total — AWS (On-Demand, Linux, Multi-AZ +200GB inter-AZ)

| Component | Monthly Cost |
|---|---|
| EC2 t3.medium × 2 (one per AZ) | $60.74 |
| EBS gp3 50 GB × 2 | $8.00 |
| S3 Standard 100 GB | $2.39 |
| Data Transfer OUT 100 GB | $8.10 |
| Inter-AZ transfer 200 GB (both directions) | $4.00 |
| **Grand Total (HA)** | **$83.23** |

---

## AWS Pricing Calculator Configuration Steps

1. Go to: https://calculator.aws/pricing/2/addService
2. **Add EC2:**
   - Region: US East (N. Virginia)
   - Instance type: t3.medium
   - OS: Linux
   - Tenancy: Shared
   - Usage: 100% (730 hours/month)
   - EBS: 50 GB gp3
3. **Add S3:**
   - Storage class: Standard
   - Storage: 100 GB
   - Requests: 10K PUT, 100K GET
4. **Add Data Transfer:**
   - Outbound to internet: 100 GB
5. Click **Save and share** — copy the URL for your submission.

---https://calculator.aws/#/estimate?trk=e8c13544-17d6-4cc0-abab-a4348449de09&sc_channel=ps&trk=e9b7f801-53c8-4092-a08e-2e0ffc7efe36&ef_id=:G:s&s_kwcid=AL!4422!3!!p!!o!!aws!487441811!1148991499401099&msclkid=2ff6cc7e08001b989e10a6591f6c69df&id=df6ae7c61f02d2742e3247c062e7886dd5c167e4

*AWS cost breakdown prepared by Joshua Blessing | FE/24/161412576 | June 2026*
