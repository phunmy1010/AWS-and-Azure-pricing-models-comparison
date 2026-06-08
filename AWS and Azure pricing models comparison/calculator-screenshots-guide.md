# Pricing Calculator Screenshots Guide

**Fellow:** Joshua Blessing | FE/24/161412576

This file documents the exact configuration used in each pricing calculator,
so screenshots can be verified against these specifications.

---

## AWS Pricing Calculator — Configuration Reference

**URL: https://calculator.aws/#/estimate?id=df6ae7c61f02d2742e3247c062e7886dd5c167e4*

### Screenshot 1: EC2 Instance Configuration
**What to capture:** The EC2 service configuration screen showing:
- Region: US East (N. Virginia)
- Instance type search result: `t3.medium`
- OS/Platform: Linux
- Tenancy: Shared Instances
- Usage: 100 On-Demand instances × 730 hours/month
- EBS storage: 1 volume × 50 GB gp3

**Expected values to verify:**
- Instance: t3.medium
- On-Demand price: $0.0416/hr
- Monthly EC2 subtotal: ~$30.37

---

### Screenshot 2: S3 Storage Configuration
**What to capture:** The S3 service configuration showing:
- Storage class: S3 Standard
- Storage amount: 100 GB
- PUT requests: 10,000
- GET requests: 100,000

**Expected values to verify:**
- Storage cost: ~$2.30
- Requests cost: ~$0.09
- S3 subtotal: ~$2.39

---

### Screenshot 3: Data Transfer Configuration
**What to capture:** Data Transfer section showing:
- Outbound data transfer to internet: 100 GB
- Region: US East

**Expected values to verify:**
- First 10 GB: $0.00
- Remaining 90 GB at $0.09/GB: $8.10

---

### Screenshot 4: AWS Summary / Total
**What to capture:** The full estimate summary page showing all services and the grand total.

** Total: $45.76/month**
URL: https://calculator.aws/#/estimate?id=df6ae7c61f02d2742e3247c062e7886dd5c167e4

---
## Azure Pricing Calculator — Configuration Reference

**URL:** https://azure.com/e/f2bbfccf8d2242718b7a055ae56352a5  
**Saved estimate:** **

### Screenshot 5: Azure VM Configuration — Linux
**What to capture:** Virtual Machines section showing:
- Region: East US
- Operating System: Linux
- VM Size: B2s (search "B2s" in the size selector)
- Hours: 730 hours (1 month full usage)
- Azure Hybrid Benefit: OFF (for Scenario A)

**Expected values to verify:**
- VM size: Standard B2s
- Pay-As-You-Go hourly: ~$0.0416
- Monthly total: ~$30.37

---

### Screenshot 6: Azure VM Configuration — Windows + Hybrid Benefit
**What to capture:** A second VM configuration (Scenario B) showing:
- Region: East US
- Operating System: Windows
- VM Size: B2s
- Azure Hybrid Benefit: **ON** (checkbox enabled)
- Licensing: Windows Server

**Expected values to verify:**
- Standard Windows rate: ~$0.0960/hr
- Rate with HB applied: ~$0.0576/hr
- Monthly total with HB: ~$42.05

---

### Screenshot 7: Azure Managed Disk Configuration
**What to capture:** Managed Disks section showing:
- Disk type: Standard SSD
- Disk size: E10 (128 GiB)
- Redundancy: LRS (Locally Redundant Storage)
- Region: East US

**Expected values to verify:**
- Disk tier: Standard SSD E10
- Monthly cost: ~$9.61

---

### Screenshot 8: Azure Blob Storage Configuration
**What to capture:** Storage Accounts section showing:
- Blob type: Block Blob
- Access tier: Hot
- Redundancy: LRS
- Capacity: 100 GB
- Write operations: 10,000
- Read operations: 100,000

**Expected values to verify:**
- Storage: ~$1.80
- Operations: ~$0.09
- Total: ~$1.89

---

### Screenshot 9: Azure Bandwidth (Egress) Configuration
**What to capture:** Bandwidth section showing:
- Outbound data transfer: 100 GB
- Zone 1 (Americas, Europe)

**Expected values to verify:**
- First 5 GB: $0.00
- Remaining 95 GB: ~$8.27

---

### Screenshot 10: Azure Summary / Total
**What to capture:** The full estimate summary showing all added services and the grand total for both Scenario A and Scenario B.

** totals:**
- Total: $68.47/month (Linux + Windows HB combined)
URL: https://azure.com/e/f2bbfccf8d2242718b7a055ae56352a5

---

.

---

*Screenshots guide prepared by Joshua Blessing | FE/24/161412576 | June 2026*
