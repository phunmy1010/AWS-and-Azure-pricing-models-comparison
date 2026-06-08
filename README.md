# AWS vs Azure Cloud Cost Comparison Report

**Author:** Joshua Blessing | Fellow ID: FE/24/161412576  
**Date:** June 2026  
**Project:** Small Web Application — Cloud Pricing Analysis

---

## Table of Contents

1. [Application Specifications](#1-application-specifications)
2. [AWS Cost Estimate](#2-aws-cost-estimate)
3. [Azure Cost Estimate](#3-azure-cost-estimate)
4. [Networking Cost Comparison](#4-networking-cost-comparison)
5. [Discount Mechanisms](#5-discount-mechanisms)
6. [Comparison Summary & Recommendations](#6-comparison-summary--recommendations)

---

## 1. Application Specifications

The reference workload is a **small production web application** serving a Nigerian SME market — modelled closely on the QuickCart use case explored during UX coursework.

| Resource | Specification |
|---|---|
| Compute | 2 vCPU, 4 GB RAM |
| OS | Linux (Ubuntu 22.04) |
| Storage | 50 GB SSD (OS + app) + 100 GB object storage |
| Database | Not in scope (managed separately) |
| Monthly data transfer OUT | 100 GB (to internet) |
| Availability Zones | 2 (for redundancy) |
| Region | US East (AWS) / East US (Azure) — comparable pricing tier |

---

## 2. AWS Cost Estimate

### Compute — EC2 (Linux)

| Parameter | Value |
|---|---|
| Instance type | t3.medium (2 vCPU, 4 GB RAM) |
| Pricing model | On-Demand |
| Hourly rate | $0.0416/hr |
| Monthly hours | 730 hrs |
| **Monthly compute cost** | **$30.37** |

### Storage — Amazon S3

| Component | Size | Unit Price | Monthly Cost |
|---|---|---|---|
| Standard storage | 100 GB | $0.023/GB | $2.30 |
| PUT/COPY requests | 10,000 | $0.005/1,000 | $0.05 |
| GET requests | 100,000 | $0.0004/1,000 | $0.04 |
| **S3 Total** | | | **$2.39** |

### EBS Root Volume (OS Disk)

| Component | Detail | Cost |
|---|---|---|
| gp3 SSD | 50 GB × $0.08/GB | $4.00 |

### Data Transfer OUT (Internet)

| Tier | Volume | Rate | Cost |
|---|---|---|---|
| First 10 GB/month | 10 GB | Free | $0.00 |
| Next 90 GB | 90 GB | $0.09/GB | $8.10 |
| **Transfer Total** | | | **$8.10** |

### AWS Monthly Total (On-Demand, Linux)

| Line Item | Cost (USD) |
|---|---|
| EC2 t3.medium (On-Demand) | $30.37 |
| EBS gp3 50 GB | $4.00 |
| S3 Standard 100 GB | $2.39 |
| Data Transfer OUT 100 GB | $8.10 |
| **TOTAL** | **$44.86** |

> 📌 **AWS Pricing Calculator link:** https://calculator.aws/pricing/2/savedsettings?id=placeholder  
> *(Replace with your saved estimate URL after configuring in the AWS Pricing Calculator)*

### AWS Free Tier Notes

- EC2: 750 hrs/month of t2.micro (Linux) for 12 months — **t3.medium is NOT free tier eligible**
- S3: 5 GB standard storage, 20,000 GET, 2,000 PUT requests — first 5 GB free for 12 months
- Data Transfer OUT: First 100 GB/month free from EC2 (first 12 months only)

---

## 3. Azure Cost Estimate

### Scenario A — Linux (No Hybrid Benefit)

| Parameter | Value |
|---|---|
| VM size | B2s (2 vCPU, 4 GB RAM) — closest Azure equivalent to t3.medium |
| OS | Linux (Ubuntu) |
| Hourly rate | $0.0416/hr |
| Monthly hours | 730 hrs |
| **Monthly VM cost** | **$30.37** |

### Scenario B — Windows Server (With Azure Hybrid Benefit)

| Parameter | Value |
|---|---|
| VM size | B2s (2 vCPU, 4 GB RAM) |
| OS | Windows Server 2022 |
| Standard Windows On-Demand rate | $0.0960/hr |
| Hybrid Benefit discount | ~40% on Windows licence |
| Effective rate with HB | $0.0576/hr |
| Monthly hours | 730 hrs |
| **Monthly VM cost (with HB)** | **$42.05** |

> Azure Hybrid Benefit (AHB) lets organisations with existing Windows Server licences under Software Assurance reuse those licences on Azure VMs, reducing the Windows licence component of VM cost by up to 40%.

### Azure Blob Storage

| Component | Size | Unit Price | Monthly Cost |
|---|---|---|---|
| LRS Hot Blob | 100 GB | $0.018/GB | $1.80 |
| Write operations | 10,000 | $0.05/10,000 | $0.05 |
| Read operations | 100,000 | $0.004/10,000 | $0.04 |
| **Blob Total** | | | **$1.89** |

### Azure Managed Disk (OS Disk)

| Component | Detail | Cost |
|---|---|---|
| Premium SSD P10 | 128 GB (smallest P-tier, closest to 50 GB) | $19.71 |
| Standard SSD E10 | 128 GB (lower cost alternative) | $9.61 |

> For cost parity, **Standard SSD E10** is selected: **$9.61/month**

### Data Transfer OUT (Internet)

| Tier | Volume | Rate | Cost |
|---|---|---|---|
| First 5 GB/month | 5 GB | Free | $0.00 |
| Next 95 GB | 95 GB | $0.087/GB | $8.27 |
| **Transfer Total** | | | **$8.27** |

### Azure Monthly Totals

| Line Item | Linux (No HB) | Windows + HB |
|---|---|---|
| Virtual Machine | $30.37 | $42.05 |
| Managed Disk (Standard SSD E10) | $9.61 | $9.61 |
| Blob Storage 100 GB | $1.89 | $1.89 |
| Data Transfer OUT 100 GB | $8.27 | $8.27 |
| **TOTAL** | **$50.14** | **$61.82** |

> 📌 **Azure Pricing Calculator link:** https://azure.com/e/placeholder  
> *(Replace with your saved estimate URL after configuring in the Azure Pricing Calculator)*

---

## 4. Networking Cost Comparison

### Inter-Availability Zone (Inter-AZ) Data Transfer

For highly available architectures, traffic flowing between AZs/zones within the same region incurs charges on both platforms.

| Provider | Inter-AZ transfer rate | Notes |
|---|---|---|
| **AWS** | $0.01/GB each direction | Applies to EC2↔EC2 across AZs; S3 is exempt |
| **Azure** | $0.01/GB each direction | Applies to VM↔VM across Availability Zones |

#### Scenario: 200 GB/month inter-zone replication traffic

| Provider | Volume | Rate | Monthly Cost |
|---|---|---|---|
| AWS | 200 GB | $0.01/GB | **$2.00** |
| Azure | 200 GB | $0.01/GB | **$2.00** |

> Both providers charge identically for inter-AZ traffic at this scale. However, **AWS charges in both directions** (ingress + egress between AZs), potentially doubling costs for bidirectional replication. Azure charges per GB transferred regardless of direction, making the per-GB rate equivalent but the accounting model slightly different.

### Key Architectural Implication

Placing a load balancer and app servers in the same AZ, with a cross-AZ standby, minimises inter-AZ charges. For a small app with 200 GB monthly cross-AZ traffic:

- **AWS additional cost:** $4.00/month (both directions at $0.01)
- **Azure additional cost:** $2.00/month (single direction counted)

---

## 5. Discount Mechanisms

### AWS Savings Plans

| Type | Description | Discount vs On-Demand |
|---|---|---|
| **Compute Savings Plans** | Flexible — applies across EC2, Fargate, Lambda; any region, OS, instance family | Up to **66%** |
| **EC2 Instance Savings Plans** | Locked to specific instance family + region; highest discount | Up to **72%** |
| **1-Year term** | Moderate commitment | ~30–40% savings |
| **3-Year term** | Maximum commitment | Up to 72% savings |

**Savings Plans** are billed as a committed hourly spend (e.g., $0.03/hr). Usage up to that commitment is discounted; excess is charged On-Demand. They offer more flexibility than Reserved Instances.

**Reserved Instances (RIs)** are the older mechanism — locked to instance type, region, and OS. Standard RIs (3-yr, all-upfront) can save up to **75%**. Convertible RIs allow instance type changes at a reduced ~54% discount.

#### AWS t3.medium — 1-Year Savings Plan Estimate

| Pricing Model | Hourly | Monthly |
|---|---|---|
| On-Demand | $0.0416 | $30.37 |
| 1-Year Compute Savings Plan | ~$0.0291 | **$21.24** |
| 3-Year Compute Savings Plan | ~$0.0199 | **$14.53** |

---

### Azure Reserved Instances (Azure Reservations)

| Term | Scope | Discount vs Pay-As-You-Go |
|---|---|---|
| **1-Year Reserved** | Single subscription or shared | Up to **40%** |
| **3-Year Reserved** | Single subscription or shared | Up to **63%** |

Azure Reservations are purchased for a specific VM size, region, and OS. They cannot be changed as flexibly as AWS Savings Plans, but **instance size flexibility** within the same family is available (e.g., B2s ↔ B4ms).

**Azure Hybrid Benefit (AHB)** stacks on top of Reserved Instance discounts for Windows workloads, creating the maximum combined discount.

#### Azure B2s Linux — 1-Year Reservation Estimate

| Pricing Model | Hourly | Monthly |
|---|---|---|
| Pay-As-You-Go | $0.0416 | $30.37 |
| 1-Year Reserved | ~$0.0250 | **$18.25** |
| 3-Year Reserved | ~$0.0154 | **$11.24** |

---

### Side-by-Side Discount Comparison

| Mechanism | AWS | Azure |
|---|---|---|
| Commitment model | Savings Plans (spend-based) | Reserved Instances (capacity-based) |
| Flexibility | High — switches instance family freely | Medium — size flexible within family |
| 1-Year discount (Linux compute) | ~30% | ~40% |
| 3-Year discount (Linux compute) | ~52% | ~63% |
| Windows licence benefit | N/A | Azure Hybrid Benefit (up to 40% off licence) |
| Spot/Preemptible pricing | EC2 Spot (up to 90% off, interruptible) | Azure Spot VMs (up to 90% off, interruptible) |

---

## 6. Comparison Summary & Recommendations

### Head-to-Head Monthly Cost (On-Demand, Linux, same specs)

| Provider | Monthly Cost | Notes |
|---|---|---|
| **AWS** | **$44.86** | EC2 t3.medium + EBS gp3 + S3 + egress |
| **Azure (Linux)** | **$50.14** | B2s + Standard SSD E10 + Blob + egress |
| **Azure (Windows + HB)** | **$61.82** | B2s Windows + HB discount applied |

**AWS is ~$5.28/month cheaper** for equivalent Linux workloads on On-Demand pricing — a ~10.5% advantage at this scale.

### Which Provider is More Cost-Effective?

| Scenario | Recommended Provider | Reason |
|---|---|---|
| Linux-based startup, small scale | **AWS** | Lower On-Demand compute + storage rates; t3.medium cheaper than B2s equivalent when storage is considered |
| Microsoft-centric enterprise (Windows Server) | **Azure** | Hybrid Benefit reuses existing Windows SA licences; 3-year Reserved + HB can bring effective cost below AWS Windows pricing |
| High-flexibility commitment | **AWS** | Compute Savings Plans switch instance families freely without forfeiting discount |
| Deep Azure discount commitment | **Azure** | 3-Year Reserved Linux is cheaper than 3-Year AWS Savings Plan at ~$11/month vs ~$14/month |
| Workloads already in Microsoft 365 / Active Directory | **Azure** | Native integration with Entra ID, no egress for Azure AD auth; total-cost savings outside raw VM pricing |

### Two Cost-Optimisation Strategies

#### Strategy 1 — Commitment-Based Discounts (Both Platforms)

Migrate from On-Demand/Pay-As-You-Go to a 1-Year commitment for any workload that runs continuously. For this application:

- **AWS:** 1-Year Compute Savings Plan reduces EC2 from $30.37 → $21.24/month (saving **$9.13/month, $109.56/year**)
- **Azure:** 1-Year Reserved Instance reduces B2s from $30.37 → $18.25/month (saving **$12.12/month, $145.44/year**)

> Best applied to: production workloads with predictable, consistent usage. Not suitable for dev/test environments or burst-only workloads.

#### Strategy 2 — Right-Sizing Instance Selection + Storage Tier Matching

The default choice of "next size up" is a common cost leak. For this app:

- Replace **Azure Premium SSD P10** ($19.71) with **Standard SSD E10** ($9.61) — saves **$10.10/month** with only marginally lower IOPS, acceptable for a web app with moderate disk I/O.
- On AWS, use **gp3** instead of gp2 for EBS — gp3 is $0.08/GB vs gp2's $0.10/GB, saving 20% on storage with higher baseline IOPS included free.
- Evaluate whether t3.medium is necessary — if CPU average stays below 20%, a **t3.small** (2 vCPU, 2 GB RAM, $0.0208/hr, **$15.18/month**) cuts compute cost in half.

> Best applied to: new deployments before committing to reserved pricing. Always right-size first, then reserve.

---

*Report prepared by Joshua Blessing | FE/24/161412576 | June 2026*
