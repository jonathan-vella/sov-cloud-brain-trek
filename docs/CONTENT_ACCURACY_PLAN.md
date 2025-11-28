# Plan: Sovereign Cloud Content Accuracy Review

Comprehensive technical validation of all Brain Trek content against current Microsoft Learn documentation (November 2025). This plan ensures accuracy across the entire sovereign cloud ecosystem.

---

## Executive Summary

| Issue Type | Count | Priority |
|------------|-------|----------|
| Terminology outdated | 15+ | 🔴 High |
| Deprecated features | 3 | 🔴 High |
| Missing new features | 12+ | 🟡 Medium |
| Architecture changes | 5 | 🟡 Medium |
| Diagram inaccuracies | 8+ | 🟡 Medium |

**Total estimated effort:** 13-19 hours

---

## Authoritative Sources

All content updates must be validated against these Microsoft Learn documentation sources:

| Domain | Primary URL | Notes |
|--------|-------------|-------|
| **Azure Local** | <https://learn.microsoft.com/en-us/azure/azure-local/> | View: `azloc-2511` (latest) |
| **Azure Arc** | <https://learn.microsoft.com/en-us/azure/azure-arc/> | All Arc-enabled services |
| **Sovereignty** | <https://learn.microsoft.com/en-us/industry/sovereignty/> | Cloud for Sovereignty docs |
| **Edge RAG** | <https://learn.microsoft.com/en-us/azure/azure-arc/edge-rag/> | Preview documentation |
| **Video Indexer** | <https://learn.microsoft.com/en-us/azure/azure-video-indexer/> | Arc-enabled extension |
| **M365 Local** | <https://learn.microsoft.com/en-us/azure/azure-local/concepts/microsoft-365-local-overview> | New deployment type |

---

## Step 1: Terminology Updates (Global Search & Replace)

Update outdated terminology across all 133+ documentation files:

| Old Term | New Term | Scope |
|----------|----------|-------|
| Azure Stack HCI | **Azure Local** | All files except historical context |
| Azure Stack HCI OS | Azure Stack HCI OS | Keep for OS download references |
| Indirectly connected mode | _(Remove)_ | Arc docs — retired Sep 2025 |
| Air-gapped | **Disconnected operations** | Use correct MS terminology |
| Stretch clusters | _(Remove)_ | Not supported in Azure Local |

**Validation script:**

```bash
# Find outdated terminology
grep -rn "Azure Stack HCI" docs/ --include="*.md" | grep -v "OS"
grep -rn "air-gapped\|airgapped" docs/ --include="*.md"
grep -rn "stretch cluster" docs/ --include="*.md"
grep -rn "indirectly connected" docs/ --include="*.md"
```

---

## Step 2: Remove Deprecated Features

### 2a. Stretch Clusters — NOT SUPPORTED

Stretch clusters are **not supported in Azure Local** (only in legacy Azure Stack HCI 22H2).

**Files to audit and update:**

- `docs/level-200/azure-local-ha-patterns.md` — Remove stretch cluster diagrams/content
- `docs/level-300/azure-local-multi-site.md` — Replace with supported multi-site patterns

**Replacement patterns:**

- Azure Site Recovery for VM replication
- Storage Replica for async replication
- Active-passive cluster pairs (separate clusters per site)

### 2b. Indirectly Connected Mode — RETIRED

Azure Arc indirectly connected mode was **retired September 2025**.

**Files to audit:**

- `docs/level-100/azure-arc-*.md`
- `docs/level-200/arc-*.md`
- `docs/level-300/azure-local-disconnected*.md`

### 2c. Node Count Updates

| Deployment | Current Limit |
|------------|---------------|
| Hyperconverged | **1-16 machines** |
| Rack-aware (Preview) | **8 machines max** |
| Multi-rack (Preview) | **Hundreds of machines** |

---

## Step 3: Azure Local Content Updates

### 3a. Deployment Types

Update content to reflect current deployment options:

| Deployment Type | Status | Scale | Description |
|-----------------|--------|-------|-------------|
| **Hyperconverged** | GA | 1-16 machines | Standard HCI with S2D storage |
| **Multi-rack** | Preview | 100s of machines | Preintegrated racks with SAN |
| **Microsoft 365 Local** | New | 9+ servers | Exchange, SharePoint, Skype on-prem |
| **Disconnected operations** | Preview | 3+ node mgmt cluster | Local control plane |

**Files to update:**

- `docs/level-100/azure-local-overview.md`
- `docs/level-200/azure-local-architecture-deep-dive.md`
- `docs/level-300/azure-local-advanced-connected.md`
- `docs/level-300/azure-local-advanced-disconnected.md`

### 3b. Disconnected Operations (Major Rewrite)

**New content needed:**

- Local Azure portal experience (runs on-premises)
- Supported services: ARM, RBAC, Arc-enabled servers, AKS Arc, Container Registry, Key Vault, Azure Policy
- Requirements: Enterprise Agreement, 3-node management cluster, 96GB RAM/node, 24 cores/node
- Preview status — requires application approval

**Files requiring rewrite:**

- `docs/level-100/azure-local-disconnected-mode.md`
- `docs/level-300/azure-local-air-gapped.md` — Rename to `azure-local-disconnected-operations.md`

### 3c. Hardware Requirements Updates

Verify current minimum specifications:

| Component | Current Minimum | Notes |
|-----------|-----------------|-------|
| Machines | 1-16 | Single-node now fully supported |
| CPU | 64-bit Intel Nehalem+ or AMD EPYC | SLAT required |
| Memory | 32 GB per machine (ECC) | 96 GB for disconnected mgmt cluster |
| Storage | 2 disks, 500 GB minimum | NVMe or SSD |
| Network | 2+ adapters, 25Gbps+ recommended | RDMA for storage |
| TPM | 2.0 required | Must be enabled |
| Secure Boot | Required | Must be enabled |

---

## Step 4: Sovereign Cloud Model Updates

### 4a. Three-Tier Sovereignty Model

| Tier | Description | Target Customers |
|------|-------------|------------------|
| **Sovereign Public Cloud** | Hyperscale Azure + sovereignty controls | EU/EFTA public sector, regulated industries |
| **Sovereign Private Cloud** | Customer-operated (Azure Local + M365 Local) | Defense, intelligence, critical infrastructure |
| **National Partner Clouds** | Locally owned/operated by partners | Countries with strict data residency laws |

**Files to update:**

- `docs/level-100/sovereign-cloud-models.md`
- `docs/level-100/sovereign-public-cloud.md`
- `docs/level-100/sovereign-private-cloud.md`
- `docs/level-100/national-partner-clouds.md`

### 4b. Sovereign Public Cloud Capabilities

| Capability | Description | Status |
|------------|-------------|--------|
| **Data Guardian** | Tamper-evident logging, EU-resident approval | GA |
| **External Key Management (EKM)** | Customer-managed HSM keys outside Azure | GA |
| **Confidential Computing** | TEE-based data-in-use protection | GA |
| **Regulated Environment Management** | Unified deployment/monitoring | Preview |
| **Sovereign Landing Zone (SLZ)** | Policy-as-code guardrails (Bicep/Terraform) | GA |

**Files to update:**

- `docs/level-100/digital-sovereignty.md`
- `docs/level-100/operational-sovereignty.md`
- `docs/level-200/encryption-key-management.md`

### 4c. EU Data Boundary

Verify content accuracy:

- Covers 27 EU + 4 EFTA countries
- Customer data and pseudonymized personal data stored in EU/EFTA
- Applies to: Azure, Dynamics 365, Power Platform, Microsoft 365
- Transparency centers in EU

**Files to update:**

- `docs/level-100/european-commitments.md`
- `docs/level-100/data-residency-concepts.md`

### 4d. National Partner Clouds

| Partner | Country | Certification Target |
|---------|---------|---------------------|
| **Bleu** | France | SecNumCloud (ANSSI) |
| **Delos Cloud** | Germany | German Cloud Platform Requirements |

**Files to update:**

- `docs/level-100/national-partner-clouds.md`

---

## Step 5: Azure Arc Content Updates

### 5a. Remove Indirectly Connected Mode

- Retired September 2025
- Update all Arc documentation to reflect only connected mode

### 5b. Arc Resource Bridge Updates

- Auto-deployed during Azure Local setup (not manual)
- Kubernetes-based appliance VM
- Enables VM lifecycle management from Azure
- Requires upgrade every 6 months minimum
- Does NOT support private link

**Files to update:**

- `docs/level-100/azure-arc-intro.md`
- `docs/level-100/azure-arc-servers.md`
- `docs/level-200/arc-enterprise-patterns.md`

### 5c. New Arc-enabled Services to Add

| Service | Status | Priority |
|---------|--------|----------|
| Edge RAG | Preview | 🔴 High — core curriculum |
| Azure IoT Operations | Preview | 🟡 Medium |
| Container Apps on Arc | GA | 🟡 Medium |
| Container Storage on Arc | GA | 🟢 Low |
| Arc site manager | Preview | 🟢 Low |

**Files to update:**

- `docs/level-100/azure-arc-intro.md` — Add services overview
- `docs/level-200/module-02-arc.md` — Add advanced services

---

## Step 6: Edge RAG & AI Content Updates

### 6a. Edge RAG (Preview)

Verify and update:

- Official name: "Edge RAG Preview, enabled by Azure Arc"
- Azure Arc-enabled Kubernetes extension
- Runs on Azure Local infrastructure
- Supported in disconnected operations

**Features to document:**

- Local GenAI language models (CPU and GPU support)
- Turnkey data ingestion and RAG pipeline
- Out-of-the-box prompt engineering/evaluation tool
- Azure-equivalent APIs
- Hybrid/vector/multimodal search support

**Files to update:**

- `docs/level-100/edge-rag-concepts.md`
- `docs/level-100/edge-rag-architecture.md`
- `docs/level-200/edge-rag-implementation.md`
- `docs/level-300/edge-rag-production.md`

### 6b. Azure AI Video Indexer (Arc-enabled)

Add/update content:

- Arc extension for edge video/audio analysis
- **Direct connection mode only** (control plane in cloud)
- Includes Phi language model for summarization
- 35+ source languages

**Hardware requirements:**

| Config | Nodes | Cores | RAM |
|--------|-------|-------|-----|
| Minimum | 1 | 32 | 64 GB |
| Recommended | 2 | 48-64 | 256 GB |

**Files to update:**

- `docs/level-100/edge-rag-use-cases.md` — Add Video Indexer section
- `docs/level-200/edge-rag-implementation.md` — Add deployment details

---

## Step 7: Microsoft 365 Local Content

### 7a. New Deployment Type

Add new content section for M365 Local:

- Exchange Server, SharePoint Server, Skype for Business on Azure Local
- For organizations needing complete on-premises productivity
- Hybrid or fully disconnected/air-gapped scenarios
- Support commitment through 2035

**Baseline hardware (9 servers):**

| Configuration | Purpose |
|---------------|---------|
| 3 servers (3-node cluster) | SharePoint + SQL Server |
| 4 servers (4 single-node) | Exchange mailbox roles |
| 2 servers (2 single-node) | Exchange edge transport |

**Minimum specs per server:**

- 2U chassis, NVMe enabled
- Dual Intel Xeon Gold 5418Y (24 cores) or equivalent
- 512 GB RAM
- 2 × 960 GB NVMe RAID-1 boot drives
- 24 × 4TB NVMe Read Intensive drives
- TPM 2.0 required

**Files to create/update:**

- `docs/level-100/azure-local-overview.md` — Add M365 Local section
- `docs/level-200/azure-local-architecture-deep-dive.md` — Add M365 deployment pattern

---

## Step 8: Diagram Accuracy Review

Review and correct diagrams that may contain outdated information:

| Diagram | File | Issue |
|---------|------|-------|
| Azure Local architecture | `storage-spaces-direct.svg` | Verify node limits, add multi-rack |
| Disconnected mode flow | `disconnected-sync-flow.svg` | Update for new disconnected ops |
| Stretch cluster HA | `azure-local-ha-patterns.md` | **Remove** — not supported |
| Arc topology | `enterprise-arc-topology.svg` | Remove indirectly connected |
| Sovereign cloud selection | `sovereign-selection-tree.svg` | Add three-tier model |
| Edge RAG architecture | `edge-rag-implementation.svg` | Verify against preview docs |

**Validation approach:**

1. Compare each diagram against Microsoft Learn source
2. Flag discrepancies
3. Update Python source and regenerate SVG
4. Verify in Jekyll build

---

## Step 9: Compliance & Regulatory Updates

### 9a. FedRAMP Updates

Verify current authorization levels and boundaries:

- Azure Government FedRAMP High
- Azure Local FedRAMP status
- DoD IL levels supported

**Files to update:**

- `docs/level-200/fedramp-compliance.md`

### 9b. EU Regulations

Verify alignment with:

- EU Data Boundary requirements
- NIS2 Directive implications
- DORA (Digital Operational Resilience Act)

**Files to update:**

- `docs/level-200/gdpr-implementation.md`
- `docs/level-100/regulatory-overview.md`

---

## Step 10: Knowledge Check Accuracy

Review all knowledge check questions for technical accuracy:

| File | Questions | Priority |
|------|-----------|----------|
| `azure-local-knowledge-check.md` | 15 | 🔴 High — may reference deprecated features |
| `azure-arc-knowledge-check.md` | 15 | 🟡 Medium |
| `edge-rag-knowledge-check.md` | 15 | 🟡 Medium |
| `cloud-models-knowledge-check.md` | 15 | 🟡 Medium |
| `compliance-knowledge-check.md` | 15 | 🟡 Medium |

**Review criteria:**

- Answers reflect current product capabilities
- No references to deprecated features
- Node counts, service names are accurate
- Deployment options are current

---

## Step 11: YAML Front Matter Audit

Validate all markdown files have consistent and valid YAML front matter:

**Required fields for all pages:**

```yaml
---
layout: default
title: Page Title
parent: Parent Section Name    # Required for child pages
nav_order: N.N                 # Numeric ordering
---
```

**Optional fields:**

```yaml
has_children: true             # For parent pages with sub-pages
description: "SEO description" # For search optimization
nav_exclude: true              # For internal/specification files
```

**Validation script:**

```bash
# Find files with missing or malformed front matter
find docs/ -name "*.md" -exec grep -L "^---" {} \;

# Check for required fields
for f in docs/**/*.md; do
  if ! grep -q "^layout:" "$f"; then echo "Missing layout: $f"; fi
  if ! grep -q "^title:" "$f"; then echo "Missing title: $f"; fi
done
```

**Common issues to fix:**

- Missing `layout: default`
- Missing or incorrect `parent:` references
- Inconsistent `nav_order` numbering (gaps, duplicates)
- Missing `title:` field
- Invalid YAML syntax (extra colons, unquoted special chars)

**Files to audit (all docs):**

- `docs/level-50/*.md`
- `docs/level-100/*.md`
- `docs/level-200/*.md`
- `docs/level-300/*.md`
- `docs/resources/*.md`

---

## Execution Phases

| Phase | Steps | Effort | Dependencies |
|-------|-------|--------|--------------|
| **Phase 1** | Steps 1-2 (Terminology + Deprecations) | 2-3 hours | None |
| **Phase 2** | Steps 3-4 (Azure Local + Sovereign Models) | 4-6 hours | Phase 1 |
| **Phase 3** | Steps 5-7 (Arc + Edge AI + M365) | 4-6 hours | Phase 1 |
| **Phase 4** | Steps 8-10 (Diagrams + Compliance + Knowledge Checks) | 3-4 hours | Phases 2-3 |
| **Phase 5** | Step 11 (YAML Front Matter Audit) | 1-2 hours | All phases |

---

## Validation Checklist

Before marking each step complete:

- [ ] Cross-referenced against Microsoft Learn documentation
- [ ] Terminology matches official Microsoft naming
- [ ] No references to deprecated features
- [ ] New features documented with correct status (GA/Preview)
- [ ] Diagrams regenerated if architecture changed
- [ ] Jekyll build passes without errors
- [ ] Knowledge check answers verified accurate

---

## Notes

1. **Version pinning**: Document against Azure Local version `azloc-2511` (November 2025). As new versions release, this plan will need updates.

2. **Preview features**: Clearly mark preview features with status badges. Do not present preview capabilities as GA.

3. **Regional availability**: Some sovereign features are region-specific. Document availability clearly.

4. **Partner content**: National partner cloud content (Bleu, Delos) should reference partner documentation for specifics.

5. **Coordination with Visual Assets Plan**: Diagram updates in Step 8 should coordinate with the Visual Assets Enhancement Plan to avoid duplicate work.
