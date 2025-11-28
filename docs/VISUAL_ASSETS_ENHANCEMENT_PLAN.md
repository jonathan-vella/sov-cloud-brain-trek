# Plan: Visual Assets Embedding & Enhancement (Python-Only)

Replace all Mermaid diagrams with Python-generated SVGs, embed 6 new Level-100 diagrams, add 13 knowledge checks, create an assets index, add SVG interactivity, and update devcontainer dependencies.

---

## Steps

### Step 1: Update devcontainer and add diagram regeneration script ✅ COMPLETED

**Completed:** 2025-11-28 — All infrastructure already in place.

- ✅ Cartopy system deps in `.devcontainer/devcontainer.json`: `libgeos-dev`, `libproj-dev`, `proj-data`, `proj-bin`
- ✅ `requirements.txt` includes: `cartopy>=0.22.0`, `pyproj>=3.6.0`, `shapely>=2.0.0`
- ✅ `scripts/regenerate-diagrams.py` exists — supports `--level` and `--dry-run` flags
- ✅ `package.json` scripts: `npm run diagrams`, `diagrams:level50`, etc.
- ✅ Mermaid VS Code extension available for drafting

### Step 2: Convert Level-50 Mermaid diagrams to Python (highest visibility, 8 diagrams)

- `cloud-computing-primer.md`: Traditional IT vs Cloud comparison, Cloud characteristics mindmap
- `cloud-service-models.md`: Shared Responsibility Model, service model layers
- `security-compliance-basics.md`: CIA Triad, Defense in Depth, encryption states
- `cloud-deployment-models.md`: Deployment model decision tree

### Step 3: Convert Level-100 Mermaid diagrams to Python (4 diagrams)

- `azure-local-connected-mode.md`: Connected mode data flow
- `azure-local-disconnected-mode.md`: Disconnected sync flow
- `azure-arc-kubernetes.md`: GitOps workflow with Flux
- `sovereign-cloud-models.md`: Sovereign cloud selection tree

### Step 4: Convert Level-200/300 Mermaid diagrams to Python (8 diagrams)

- `edge-rag-implementation.md`: RAG implementation flowchart
- `azure-local-ha-patterns.md`: Failover sequence
- `arc-policy-and-governance.md`: Policy inheritance hierarchy
- `encryption-key-management.md`: Key lifecycle states
- `azure-local-air-gapped.md`: Air-gapped update process
- `edge-rag-mlops.md`: MLOps continuous improvement
- `zero-trust-monitoring.md`: Compliance monitoring loop

### Step 5: Embed 6 new Level-100 diagrams into target pages (with collapsible wrappers)

| Diagram | Target File |
|---------|-------------|
| `azure-regions-map.svg` | `docs/level-50/azure-global-infrastructure.md` |
| `capex-opex-comparison.svg` | `docs/level-50/cloud-computing-primer.md` |
| `nist-cloud-characteristics.svg` | `docs/level-50/cloud-computing-primer.md` |
| `data-classification-pyramid.svg` | `docs/level-50/security-compliance-basics.md` |
| `regulatory-timeline.svg` | `docs/level-50/compliance-frameworks.md` |
| `eu-data-boundary.svg` | `docs/level-100/european-commitments.md` |

### Step 6: Create knowledge checks for 13 Phase 3 pages (10-15 questions each, separate files)

- Create `level-300-architecture-knowledge-check.md` covering: sovereign-landing-zone, data-classification, incident-response
- Create `level-300-industry-knowledge-check.md` covering: healthcare, financial-services, government, critical-infrastructure
- Create `level-300-patterns-knowledge-check.md` covering: api-gateway, event-driven, data-mesh
- Create `level-300-operations-knowledge-check.md` covering: observability, devsecops, disaster-recovery

### Step 7: Create visual assets index page at `docs/resources/visual-assets-index.md`

- Organize 70+ SVGs by level with thumbnails and descriptions
- Link to Python source scripts for regeneration
- Document Azure color palette and usage patterns

### Step 8: Add interactive SVG features via `docs/assets/js/interactive-diagrams.js`

- Hover tooltips, click-to-zoom, pan for geographic map
- Register in `docs/_includes/head_custom.html`

### Step 9: Audit and fix navigation structure ✅ COMPLETED

**Completed:** 2025-01-XX — Fixed 16 Level-300 files with incorrect parent references.

Review and correct YAML front matter across all documentation files:

**Navigation order (`nav_order`):**

- Ensure sequential `nav_order` values within each level (no gaps or duplicates)
- Verify parent pages have `has_children: true`
- Check child pages have correct `parent:` values matching parent's `title:`

**Required front matter fields:**

- `layout: default`
- `title:` (must match parent references exactly)
- `nav_order:` (sequential integer)
- `parent:` (for child pages)
- `grand_parent:` (for grandchild pages)
- `description:` (SEO and accessibility)
- `nav_exclude: true` (for supplementary pages that shouldn't appear in sidebar)

**Files to audit:**

- `docs/level-50/*.md` — Module pages and sub-pages
- `docs/level-100/*.md` — 5 modules with deep-dive topics
- `docs/level-200/*.md` — 6 modules with labs
- `docs/level-300/*.md` — 5 modules with supplementary content (many need `nav_exclude`)
- `docs/resources/*.md` — Resource pages

**Validation script:**

```bash
# Check for missing front matter fields
grep -L "^nav_order:" docs/**/*.md
grep -L "^parent:" docs/level-*/[^R]*.md  # Exclude README files
```

---

### Step 10: Update all module durations (~50% reduction)

Reduce module completion times across all levels to reflect more realistic estimates:

| Level | Current Total | New Total | Reduction |
|-------|---------------|-----------|-----------|
| **Level 50** | 2-4 hours | 1-2 hours | 50% |
| **Level 100** | 4-12 hours | 2-6 hours | 50% |
| **Level 200** | 43-58 hours | 22-30 hours | ~50% |
| **Level 300** | ~50 hours | ~25 hours | 50% |

**Files to update (~65 duration references):**

- `docs/level-50/README.md` — Total duration, module durations, learning path times
- `docs/level-50/module-01-cloud-computing.md` — 45-60 min → 25-30 min
- `docs/level-50/module-02-security-compliance.md` — 45-60 min → 25-30 min
- `docs/level-50/module-03-azure-intro.md` — 60-90 min → 30-45 min
- `docs/level-100/README.md` — Total duration, 5 module durations
- `docs/level-100/module-01-digital-sovereignty.md` — 1-2 hrs → 30-60 min
- `docs/level-100/module-02-cloud-models.md` — 2-3 hrs → 1-1.5 hrs
- `docs/level-100/module-03-azure-local.md` — 2-3 hrs → 1-1.5 hrs
- `docs/level-100/module-04-azure-arc.md` — 1.5-2 hrs → 45-60 min
- `docs/level-100/module-05-edge-rag.md` — 2-2.5 hrs → 1-1.25 hrs
- `docs/level-200/README.md` — Total duration, 6 module durations, roadmap table
- `docs/level-200/module-01-azure-local.md` — 6-8 hrs → 3-4 hrs
- `docs/level-200/module-02-arc.md` — 7-9 hrs → 3.5-4.5 hrs
- `docs/level-200/module-03-edge-rag.md` — 8-10 hrs → 4-5 hrs
- `docs/level-200/module-04-presales.md` — 5-7 hrs → 2.5-3.5 hrs
- `docs/level-200/module-05-compliance.md` — 6-8 hrs → 3-4 hrs
- `docs/level-300/README.md` — 5 module durations
- `docs/level-300/*.md` — Individual module and lab durations
- `docs/README.md` — Summary table durations
- `docs/index.md` — Total program duration
- `docs/introduction.md` — Weekly time commitments

---

### Step 11: Content density & UX improvements

Reduce scrolling and improve scanability across all documentation:

**11a. Expand collapsible sections (high priority)**

- Make all `<details>` blocks collapsed by default (remove `open` attribute)
- Wrap code blocks (50+ lines) in collapsible sections
- Collapse "Deep Dive" and "Best Practices" subsections
- Keep only essential content expanded

**11b. Split long pages into overview + sub-pages**

- Identify pages over 500 lines
- Extract detailed sections to linked sub-pages
- Keep main module page as concise overview with navigation links
- Pattern: "For implementation details, see [Topic Deep Dive →](link)"

**11c. Add executive summary cards**

Add visual card at top of each module:

```markdown
{: .note }
> ⏱️ **Time:** 30 min | 🎯 **Objectives:** 3 key skills | 📋 **Prerequisites:** [Link]
```

**11d. Convert bullet lists to tables**

Replace verbose bullet lists with compact comparison tables:

| Before | After |
|--------|-------|
| `- **Feature:** Description` (5 items) | 5-row table |
| Vertical space: ~15 lines | Vertical space: ~7 lines |

**11e. Replace prose with decision diagrams**

Convert "When to use X vs Y" text explanations to:

- Flowchart SVGs (aligns with visual assets work)
- Decision tables with checkmarks
- "Choose this if..." callout boxes

**11f. Move long code examples to linked files**

- Show 5-10 line excerpt inline
- Link to full example: "📄 [View complete script →](../examples/script.py)"
- Create `docs/examples/` folder for reference code

**11g. Consolidate repetitive content**

- Create single "Best Practices" reference page per level
- Create single "Common Pitfalls" page instead of repeating per module
- Link from modules: "See [Level 200 Best Practices](best-practices.md)"

**Files requiring density improvements (priority order):**

1. `docs/level-100/azure-local-overview.md` (~1100 lines)
2. `docs/level-100/azure-arc-intro.md` (~1100 lines)
3. `docs/level-100/edge-rag-concepts.md` (~700 lines)
4. `docs/level-200/edge-rag-implementation.md` (~800 lines)
5. `docs/level-200/compliance-security-patterns.md` (~700 lines)
6. `docs/level-300/zero-trust.md` (~600 lines)

---

## Mermaid → Python Conversion Matrix (20 diagrams)

| Level | Source File | Diagram Description | Python Library | Output SVG |
|-------|-------------|---------------------|----------------|------------|
| L50 | `cloud-computing-primer.md` | Traditional IT vs Cloud | `matplotlib` | `traditional-vs-cloud.svg` |
| L50 | `cloud-computing-primer.md` | Cloud characteristics mindmap | `matplotlib` | `cloud-characteristics-mindmap.svg` |
| L50 | `cloud-service-models.md` | Shared responsibility | `matplotlib` | `shared-responsibility-model.svg` |
| L50 | `cloud-service-models.md` | Service model layers | `diagrams` | `service-model-layers.svg` |
| L50 | `security-compliance-basics.md` | CIA Triad | `matplotlib` | `cia-triad.svg` |
| L50 | `security-compliance-basics.md` | Defense in Depth | `matplotlib` | `defense-in-depth.svg` |
| L50 | `security-compliance-basics.md` | Encryption states | `diagrams` | `encryption-states.svg` |
| L50 | `cloud-deployment-models.md` | Deployment decision tree | `matplotlib` | `deployment-decision-tree.svg` |
| L100 | `azure-local-connected-mode.md` | Connected mode flow | `diagrams` | `connected-mode-flow.svg` |
| L100 | `azure-local-disconnected-mode.md` | Disconnected sync flow | `diagrams` | `disconnected-sync-flow.svg` |
| L100 | `azure-arc-kubernetes.md` | GitOps workflow | `diagrams` | `gitops-flux-workflow.svg` |
| L100 | `sovereign-cloud-models.md` | Selection decision tree | `matplotlib` | `sovereign-selection-tree.svg` |
| L200 | `edge-rag-implementation.md` | RAG flowchart | `diagrams` | `rag-implementation-flow.svg` |
| L200 | `azure-local-ha-patterns.md` | Failover sequence | `diagrams` | `failover-sequence.svg` |
| L200 | `arc-policy-and-governance.md` | Policy hierarchy | `diagrams` | `policy-hierarchy.svg` |
| L200 | `encryption-key-management.md` | Key lifecycle | `matplotlib` | `key-lifecycle-states.svg` |
| L300 | `azure-local-air-gapped.md` | Air-gapped updates | `diagrams` | `air-gapped-update-flow.svg` |
| L300 | `edge-rag-mlops.md` | MLOps loop | `diagrams` | `mlops-continuous-loop.svg` |
| L300 | `zero-trust-monitoring.md` | Compliance loop | `diagrams` | `compliance-monitoring-loop.svg` |
| L300 | Troubleshooting | Troubleshooting flowchart | `matplotlib` | `troubleshooting-flowchart.svg` |

---

## Diagram Regeneration Script

```python
# scripts/regenerate-diagrams.py
"""Regenerate all SVG diagrams from Python sources."""
import subprocess
from pathlib import Path

DIAGRAM_DIRS = [
    "docs/assets/diagrams/src/level-50",
    "docs/assets/diagrams/src/level-100",
    "docs/assets/diagrams/src/level-200",
    "docs/assets/diagrams/src/level-300",
]

def regenerate_all():
    for dir_path in DIAGRAM_DIRS:
        for script in Path(dir_path).glob("*.py"):
            print(f"Generating: {script.name}")
            subprocess.run(["python", str(script)], check=True)

if __name__ == "__main__":
    regenerate_all()
```

---

## Package.json Script

```json
{
  "scripts": {
    "diagrams": "python scripts/regenerate-diagrams.py",
    "diagrams:level50": "python scripts/regenerate-diagrams.py --level 50"
  }
}
```

---

## Further Considerations

1. **Conversion batching**: Recommend completing Level-50 first (Step 2), as these are foundational pages with highest traffic, then sequential L100→L200→L300.

2. **Knowledge check grouping**: Plan proposes 4 grouped knowledge check files (architecture, industry, patterns, operations) rather than 13 individual files — reduces file count while maintaining coverage. Confirm this approach?

3. **Interactive features priority**: Recommend implementing zoom/tooltips after all Mermaid conversions complete (Step 8 last) to avoid rework if diagram structure changes.

4. **Duration updates (Step 10)**: Should be executed as a batch operation using sed or multi-replace to ensure consistency. Consider updating introduction.md weekly commitments proportionally (e.g., "1-2 hours/week" → "30-60 min/week").

5. **Navigation audit (Step 9)**: Run this early to catch issues before other changes. Use `bundle exec jekyll build` to validate — Jekyll will warn about missing parent references.

6. **Content density (Step 11)**: This is a significant undertaking. Recommend prioritizing the 6 longest files first, then applying patterns learned to remaining content. Consider creating a reusable "module template" for consistency.
