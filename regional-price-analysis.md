# Regional Price Analysis — AWS vs Azure
## Same Architecture Across Three Global Regions

**Fellow:** Joshua Blessing | FE/24/161412576  
**Date:** June 2026  
**Architecture:** 2 vCPU / 4 GB RAM / 50 GB SSD / 100 GB Object Storage / 100 GB Egress (Linux)

---

## Architecture Reference

| Component | AWS | Azure |
|---|---|---|
| Compute | EC2 t3.medium (2 vCPU, 4 GB RAM) | B2s (2 vCPU, 4 GB RAM) |
| OS Disk | EBS gp3 50 GB | Standard SSD E10 |
| Object Storage | S3 Standard 100 GB | Blob LRS Hot 100 GB |
| Egress | 100 GB to internet | 100 GB to internet |

---

## Region 1 — United States (US East)

### AWS — US East (N. Virginia) `us-east-1`

| Component | Monthly Cost |
|---|---|
| EC2 t3.medium On-Demand | $30.37 |
| EBS gp3 50 GB | $4.00 |
| S3 Standard 100 GB | $2.39 |
| Egress 100 GB | $8.10 |
| **Total** | **$44.86** |

### Azure — East US (Virginia)

| Component | Monthly Cost |
|---|---|
| B2s Linux VM | $30.37 |
| Standard SSD E10 | $9.61 |
| Blob LRS Hot 100 GB | $1.89 |
| Egress 100 GB | $8.27 |
| **Total** | **$50.14** |

**AWS vs Azure difference (US East): AWS is $5.28 cheaper — 10.5% advantage**

---

## Region 2 — Europe (West Europe / EU West)

### AWS — EU West (Ireland) `eu-west-1`

| Component | Rate | Monthly Cost |
|---|---|---|
| EC2 t3.medium On-Demand | $0.0464/hr | $33.87 |
| EBS gp3 50 GB | $0.0952/GB | $4.76 |
| S3 Standard 100 GB | $0.023/GB | $2.39 |
| Egress 100 GB | $0.09/GB | $8.10 |
| **Total** | | **$49.12** |

> AWS EU West (Ireland) compute is ~11.5% more expensive than US East due to higher regional infrastructure and energy costs.

### Azure — West Europe (Netherlands)

| Component | Rate | Monthly Cost |
|---|---|---|
| B2s Linux VM | $0.0510/hr | $37.23 |
| Standard SSD E10 | $0.0096/GB | $10.37 |
| Blob LRS Hot 100 GB | $0.0200/GB | $2.00 |
| Egress 100 GB | $0.087/GB | $8.27 |
| **Total** | | **$57.87** |

> Azure West Europe is ~15.4% more expensive than Azure East US for equivalent Linux compute.

**AWS vs Azure difference (Europe): AWS is $8.75 cheaper — 15.1% advantage**

---

## Region 3 — Asia Pacific (Southeast Asia / Asia East)

### AWS — AP Southeast (Singapore) `ap-southeast-1`

| Component | Rate | Monthly Cost |
|---|---|---|
| EC2 t3.medium On-Demand | $0.0528/hr | $38.54 |
| EBS gp3 50 GB | $0.096/GB | $4.80 |
| S3 Standard 100 GB | $0.025/GB | $2.50 |
| Egress 100 GB | $0.12/GB | $12.00 |
| **Total** | | **$57.84** |

> Asia Pacific egress rates are significantly higher ($0.12/GB vs $0.09/GB in US), a key cost driver for internet-facing applications.

### Azure — Southeast Asia (Singapore)

| Component | Rate | Monthly Cost |
|---|---|---|
| B2s Linux VM | $0.0528/hr | $38.54 |
| Standard SSD E10 | $0.0110/GB | $11.88 |
| Blob LRS Hot 100 GB | $0.023/GB | $2.30 |
| Egress 100 GB | $0.087/GB | $8.27 |
| **Total** | | **$60.99** |

> Azure Southeast Asia compute matches AWS pricing but storage and egress differ.

**AWS vs Azure difference (Asia Pacific): AWS is $3.15 cheaper — 5.2% advantage**

---

## Regional Summary Table with Percentage Differences

| Region | AWS Total | Azure Total | $ Difference | % Difference | Winner |
|---|---|---|---|---|---|
| **US East** | $44.86 | $50.14 | $5.28 | AWS 10.5% cheaper | AWS |
| **Europe (Ireland/Netherlands)** | $49.12 | $57.87 | $8.75 | AWS 15.1% cheaper | AWS |
| **Asia Pacific (Singapore)** | $57.84 | $60.99 | $3.15 | AWS 5.2% cheaper | AWS |
| **Average across regions** | **$50.61** | **$56.33** | **$5.72** | **AWS 10.2% cheaper** | **AWS** |

---

## Service Category Percentage Breakdown

### Compute (VM/EC2 only)

| Region | AWS EC2 | Azure VM | % Difference |
|---|---|---|---|
| US East | $30.37 | $30.37 | 0% (identical) |
| Europe | $33.87 | $37.23 | Azure 9.9% more expensive |
| Asia Pacific | $38.54 | $38.54 | 0% (identical) |

> Compute pricing is **identical in US East and Asia Pacific** — both providers match each other closely on burstable instance pricing in competitive markets. Europe is where Azure charges a premium.

---

### Storage (OS Disk)

| Region | AWS EBS gp3 | Azure Std SSD E10 | % Difference |
|---|---|---|---|
| US East | $4.00 | $9.61 | Azure 140.3% more expensive |
| Europe | $4.76 | $10.37 | Azure 117.9% more expensive |
| Asia Pacific | $4.80 | $11.88 | Azure 147.5% more expensive |

> **Storage is the biggest structural cost gap.** Azure's minimum Managed Disk tier (128 GiB E10) for a 50 GB workload costs 2–2.5× more than AWS gp3, which bills per exact GB used. This gap exists in every region.

---

### Object Storage (S3/Blob)

| Region | AWS S3 | Azure Blob | % Difference |
|---|---|---|---|
| US East | $2.39 | $1.89 | Azure 20.9% cheaper |
| Europe | $2.39 | $2.00 | Azure 16.3% cheaper |
| Asia Pacific | $2.50 | $2.30 | Azure 8.0% cheaper |

> **Azure Blob Storage is consistently cheaper than AWS S3** across all regions — a meaningful advantage for storage-heavy workloads.

---

### Egress (Data Transfer OUT to Internet)

| Region | AWS Egress | Azure Egress | % Difference |
|---|---|---|---|
| US East | $8.10 | $8.27 | Azure 2.1% more expensive |
| Europe | $8.10 | $8.27 | Azure 2.1% more expensive |
| Asia Pacific | $12.00 | $8.27 | AWS 45.1% more expensive |

> **Asia Pacific is the critical outlier** — AWS charges $0.12/GB egress vs Azure's $0.087/GB, making Azure significantly cheaper for egress-heavy applications in that region. For a content-heavy app serving Southeast Asian users, Azure's egress advantage could offset other cost differences.

---

## Key Regional Findings

### 1. US East is the cheapest region on both platforms
Both AWS and Azure price their US East regions as their most competitive, likely due to the highest infrastructure density and longest operational history. Developers and startups should default to US East unless data residency or latency requirements dictate otherwise.

### 2. Europe adds ~10–15% cost premium on both platforms
EU regulatory compliance (GDPR infrastructure, data residency guarantees) and higher energy costs push European region pricing up across both providers. Azure's Europe premium is slightly higher than AWS's.

### 3. Asia Pacific egress costs are a deciding factor
For applications serving Asian users with high outbound traffic, **Azure has a significant egress advantage** ($0.087/GB vs AWS $0.12/GB). A workload with 1 TB/month egress in Singapore would pay $122.88 on AWS vs $89.09 on Azure — a $33.79/month ($405/year) difference from egress alone.

### 4. Storage tier structure favours AWS at small scales globally
AWS gp3 per-GB billing consistently undercuts Azure's fixed-tier Managed Disks for workloads under 128 GiB across all regions. This advantage shrinks as disk size grows (at 1 TB, Azure's per-GB rate becomes more competitive).

### 5. Best region by use case

| Use Case | Best Region | Best Provider |
|---|---|---|
| Nigerian startup, global CDN not needed | US East | AWS |
| EU-based users, GDPR compliance required | EU West (Ireland/Netherlands) | AWS |
| Southeast Asian users, high egress | AP Southeast (Singapore) | Azure |
| Microsoft enterprise, any region | Nearest Azure region | Azure + HB |

---

## Regional Cost Visualisation

```
Monthly Cost by Region (USD)
─────────────────────────────────────────────────────
US East      AWS  ████████████████████████  $44.86
             AZR  ███████████████████████████  $50.14

Europe       AWS  ████████████████████████████  $49.12
             AZR  █████████████████████████████████  $57.87

Asia Pacific AWS  █████████████████████████████████  $57.84
             AZR  ███████████████████████████████████  $60.99
─────────────────────────────────────────────────────
             AWS consistently lower except Asia egress
```

---

*Regional analysis prepared by Joshua Blessing | FE/24/161412576 | June 2026*  
*Pricing sourced from AWS and Azure public pricing pages, June 2026*
