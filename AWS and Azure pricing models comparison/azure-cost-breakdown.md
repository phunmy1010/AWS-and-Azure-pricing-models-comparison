# Azure Cost Breakdown — Small Web Application

**Fellow:** Joshua Blessing | FE/24/161412576  
**Region:** East US (Virginia)  
**Pricing basis:** Pay-As-You-Go (June 2026 public rates)

---

## Compute — Azure Virtual Machines

### Instance Selection Rationale

The application requires **2 vCPU and 4 GB RAM**. The **B2s** (Burstable series) is the Azure equivalent to AWS t3.medium — both are burstable, general-purpose instances designed for workloads with moderate baseline and burst CPU requirements.

| Attribute | Value |
|---|---|
| VM series | Bsv2 / B-series (burstable) |
| VM size | Standard_B2s |
| vCPU | 2 |
| Memory | 4 GB |
| Temp storage | 8 GB |
| Max data disks | 4 |
| Max NICs | 2 |

---

## Scenario A: Linux (Ubuntu 22.04) — No Hybrid Benefit

| Pricing Model | $/hr | 730 hrs/month | Annual |
|---|---|---|---|
| Pay-As-You-Go | $0.0416 | **$30.37** | $364.42 |
| 1-Year Reserved | $0.0250 | **$18.25** | $219.00 |
| 3-Year Reserved | $0.0154 | **$11.24** | $134.88 |
| Spot VM (variable) | ~$0.0090 avg | ~$6.57 | ~$78.84 |

---

## Scenario B: Windows Server 2022 — With Azure Hybrid Benefit

### What is Azure Hybrid Benefit?

Azure Hybrid Benefit (AHB) allows organisations with **Windows Server licences covered by active Software Assurance (SA)** to reuse those licences on Azure VMs. This eliminates the Windows Server licence component from the VM hourly rate.

- A standard Windows Server licence adds approximately **$0.0544/hr** to a B2s VM
- With AHB applied, you only pay the Linux-equivalent base compute rate
- AHB can reduce the Windows VM cost by **40–49%** depending on VM size

| Pricing Model | $/hr (no HB) | $/hr (with HB) | Monthly (730 hrs) |
|---|---|---|---|
| Pay-As-You-Go Windows | $0.0960 | $0.0576 | **$42.05** |
| 1-Year Reserved + HB | $0.0650 | $0.0390 | **$28.47** |
| 3-Year Reserved + HB | $0.0415 | $0.0249 | **$18.18** |

> 💡 For an enterprise already paying for Windows Server SA through an existing EA (Enterprise Agreement), the savings from AHB effectively make Azure Windows VMs cost-competitive with Linux instances on competing platforms.

---

## Storage — Azure Managed Disks (OS Disk)

### Standard SSD vs Premium SSD

| Disk type | Size tier | Size | IOPS | $/month |
|---|---|---|---|---|
| Premium SSD (P10) | 128 GiB | 128 GiB | 500 | $19.71 |
| **Standard SSD (E10)** | 128 GiB | 128 GiB | 500 | **$9.61** |
| Standard HDD (S10) | 128 GiB | 128 GiB | 500 | $5.28 |
| Ultra Disk | Custom | Custom | Custom | Variable |

**Selected: Standard SSD E10 at $9.61/month**

> Azure Managed Disks come in fixed size tiers; the minimum tier covering 50 GB is the 128 GiB E10/P10. This is a structural difference from AWS gp3, which bills per GB (50 GB = $4.00). This makes Azure OS disk storage inherently more expensive at small sizes.

---

## Storage — Azure Blob Storage

### Storage Class: LRS Hot Tier

| Line item | Volume | Rate | Monthly |
|---|---|---|---|
| Storage (LRS Hot) | 100 GB | $0.018/GB | $1.80 |
| Write operations | 10,000 | $0.05/10,000 | $0.05 |
| Read operations | 100,000 | $0.004/10,000 | $0.04 |
| Data retrieval | N/A | Free (Hot tier) | $0.00 |
| **Blob Monthly Total** | | | **$1.89** |

### Azure Blob Tiers Comparison

| Tier | Use case | $/GB/month | Retrieval |
|---|---|---|---|
| Hot | Frequent access | $0.018 | Free |
| Cool | Infrequent, 30-day min | $0.01 | $0.01/GB |
| Cold | Rarely accessed, 90-day min | $0.0045 | $0.03/GB |
| Archive | Long-term, 180-day min | $0.00099 | $0.022/GB |

---

## Data Transfer

### Outbound to Internet (Egress)

| Tier | Volume | Rate | Cost |
|---|---|---|---|
| First 5 GB/month | 5 GB | $0.00 | $0.00 |
| 5 GB – 10 TB | 95 GB | $0.087/GB | **$8.27** |
| **Total egress cost** | | | **$8.27** |

### Inter-Zone Data Transfer (if multi-zone architecture)

| Traffic type | Volume | Rate | Cost |
|---|---|---|---|
| Between Availability Zones (same region) | 200 GB | $0.01/GB | **$2.00** |

> Azure counts each GB transferred between zones once (regardless of direction), compared to AWS which charges both ingress and egress separately.

---

## Azure Free Tier Reference

| Service | Free Allowance | Duration |
|---|---|---|
| Virtual Machines | 750 hrs/month B1s (Linux) | 12 months |
| Managed Disks | 64 GiB P6 SSD × 2 | 12 months |
| Blob Storage | 5 GB LRS Hot | 12 months |
| Bandwidth (egress) | 15 GB/month | 12 months (always free) |
| Azure Functions | 1M requests/month | Always free |

> ⚠️ B2s is **not** free-tier eligible. The free tier only covers B1s (1 vCPU, 1 GB RAM).

---

## Monthly Totals

### Scenario A — Linux, Pay-As-You-Go (Single Zone)

| Component | Monthly Cost |
|---|---|
| B2s Linux VM | $30.37 |
| Standard SSD E10 (128 GiB) | $9.61 |
| Blob Storage LRS Hot 100 GB | $1.89 |
| Egress 100 GB | $8.27 |
| **Grand Total** | **$50.14** |

### Scenario B — Windows Server + Azure Hybrid Benefit, Pay-As-You-Go

| Component | Monthly Cost |
|---|---|
| B2s Windows VM (with AHB) | $42.05 |
| Standard SSD E10 (128 GiB) | $9.61 |
| Blob Storage LRS Hot 100 GB | $1.89 |
| Egress 100 GB | $8.27 |
| **Grand Total** | **$61.82** |

### Scenario A — Linux, 1-Year Reserved (Single Zone)

| Component | Monthly Cost |
|---|---|
| B2s Linux VM (1-Year Reserved) | $18.25 |
| Standard SSD E10 (128 GiB) | $9.61 |
| Blob Storage LRS Hot 100 GB | $1.89 |
| Egress 100 GB | $8.27 |
| **Grand Total (Reserved)** | **$38.02** |

---

## Azure Pricing Calculator Configuration Steps

1. Go to: https://azure.microsoft.com/en-us/pricing/calculator/
2. **Add Virtual Machines:**
   - Region: East US
   - OS: Linux (or Windows for Scenario B)
   - VM size: B2s
   - Tier: Standard
   - Hours: 730 (1 month)
   - For Windows: enable Azure Hybrid Benefit toggle
3. **Add Managed Disks:**
   - Type: Standard SSD
   - Size: E10 (128 GiB)
   - Redundancy: LRS
4. **Add Storage Accounts (Blob):**
   - Type: Block Blob
   - Tier: Hot
   - Redundancy: LRS
   - Capacity: 100 GB
   - Write operations: 10K, Read operations: 100K
5. **Add Bandwidth:**
   - Outbound data transfer: 100 GB (Zone 1)
6. Click **Save and share estimate** — copy the URL for your submission.

---

*Azure cost breakdown prepared by Joshua Blessing | FE/24/161412576 | June 2026*
