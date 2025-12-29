# GeoAssetAI – Asset-Level Structural Protection Proxy (Demo)

## Overview
GeoAssetAI is a **demonstration project** that explores how AI-assisted text analysis and geospatial
signals can be combined to infer **asset-level structural protection proxies** against physical risks
(e.g. flooding).

This project is designed as a **methodology and engineering demo**, not a production risk model.
All data used are **synthetic or publicly reproducible**.

The motivation is a documented client need to understand whether **individual assets** may have
structural protective measures, a dimension that is typically considered only at **issuer or regional
level** in existing physical risk solutions.

---

## Problem Statement
Financial institutions increasingly ask questions such as:

> *“Does this specific asset have structural protection against physical risks (e.g. flood defenses)?”*

In many industry-standard models, adaptation is incorporated at:
- regional level (e.g. flood protection standards), or
- issuer level (company-wide adaptation assumptions),

but **not at individual asset level**.

This demo explores a **proxy-based, explainable approach** to bridge that gap without claiming
ground truth.

---

## What This Project Does
For a small set of synthetic assets, the pipeline:
1. Extracts **adaptation-related signals** from company disclosures (text).
2. Evaluates **geospatial context**, such as:
   - floodplain intersection,
   - relative elevation,
   - proximity to protective infrastructure.
3. Combines these signals into an **interpretable proxy score** indicating whether
   structural protection is *Likely*, *Possible*, or *Unlikely*.
4. Produces **explainability artifacts** showing *why* a proxy score was assigned.

---

## What This Project Does NOT Do
- ❌ It does not provide verified adaptation data.
- ❌ It does not replace physical risk or financial impact models.
- ❌ It does not produce investable signals.

All outputs are **decision-support indicators**, intended to support QA, analyst review,
and model governance discussions.

---

## Repository Structure
```text
.
├── bootstrap_env.py              # Environment setup & validation script
├── environment_report.txt        # Auto-generated environment diagnostics
├── data/
│   ├── geoassets.geojson         # Synthetic asset locations
│   ├── reports/                 # Synthetic company disclosures
│   └── ground_truth.json        # Synthetic evaluation labels
├── notebooks/
│   ├── 01_data_generation.ipynb
│   ├── 02_text_extraction.ipynb
│   ├── 03_geospatial_checks.ipynb
│   └── 04_scoring_and_evaluation.ipynb
├── src/
│   ├── extract.py               # Text extraction logic
│   ├── geochecks.py             # Spatial validation logic
│   └── score.py                 # Scoring & explainability
└── README.md

## Methodology

This project derives an **asset-level structural flood protection proxy** using a
multi-faceted geospatial approach. The objective is not to determine whether an
asset is definitively protected, but to infer the **likelihood of structural
protection** based on spatial context where direct disclosure is absent.

### Why a geospatial proxy is required
Authoritative facility-level registries (e.g. E-PRTR) provide precise asset
locations and activity metadata but **do not disclose structural flood protection
measures** at the asset level. This absence is systematic rather than incidental.

As a result, structural protection must be inferred from the **spatial context
of flood defense systems**, rather than from textual disclosures.

### Input data
- **Asset locations:** European Pollutant Release and Transfer Register (E-PRTR),
  filtered to a small Netherlands sample for demonstration.
- **Flood defense infrastructure:** OpenStreetMap (OSM) features including dikes,
  levees, embankments, flood walls, dams, sluices, and pumping stations.
- **Coordinate systems:**
  - All distance and buffer calculations are performed in **EPSG:28992
    (Amersfoort / RD New)** to ensure metric accuracy.
  - Outputs are stored in **EPSG:4326 (WGS84)** for interoperability.

### Geospatial signals
Three complementary spatial signals are computed for each asset:

1. **Proximity to nearest flood defense**
   - Distance (in meters) to the closest mapped flood defense feature.
   - Captures local, direct protection.

2. **Defense density within 1 km**
   - Count of flood defense features within a 1 km radius.
   - Captures system-level protection typical of polder-based flood management.

3. **Soft containment within 500 m**
   - Boolean indicator of whether the asset lies within a 500 m buffer of any
     defense feature.
   - Used as a contextual boost rather than a hard rule.

### Scoring
The final proxy score is computed as:

