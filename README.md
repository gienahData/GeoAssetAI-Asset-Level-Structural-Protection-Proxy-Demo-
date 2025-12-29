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
---
.
├── bootstrap_env.py              # Environment setup & validation script
├── environment_report.txt        # Auto-generated environment diagnostics
├── data/
│   ├── geoassets.geojson         # Synthetic asset locations
│   ├── reports/                  # Synthetic company disclosures
│   └── ground_truth.json         # Synthetic evaluation labels
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
multi-faceted geospatial approach. The objective is **not** to determine whether an
asset is definitively protected, but to infer the **likelihood of structural protection**
based on spatial context where direct disclosure is absent.

**Why a geospatial proxy is required**
- Authoritative facility-level registries (e.g. **E-PRTR**) provide precise asset
  locations and activity metadata.
- These registries **do not disclose structural flood protection measures** at the
  asset level.
- This absence is **systematic rather than incidental**.

As a result, structural protection must be inferred from the **spatial context of
flood defense systems**, rather than from textual disclosures.

**Input data**
- **Asset locations:** European Pollutant Release and Transfer Register (E-PRTR)
- **Flood defense infrastructure:** OpenStreetMap (OSM) dikes, levees, embankments,
  flood walls, dams, sluices, and pumping stations
- **Coordinate reference systems:**
  - Metric analysis: **EPSG:28992 (Amersfoort / RD New)**
  - Storage and visualization: **EPSG:4326 (WGS84)**

**Geospatial signals**
1. **Proximity to nearest flood defense**
   - Distance (meters) to the closest mapped defense feature

2. **Defense density within 1 km**
   - Count of defense features within a 1 km radius

3. **Soft containment within 500 m**
   - Boolean indicator of whether the asset lies within a 500 m buffer of any defense
   - Used as a contextual boost, not a hard rule

**Scoring and labels**
The final proxy score is computed as:

```
defense_score =
  min(1.0,
      0.6 * proximity_score +
      0.4 * density_score +
      buffer_boost)
```

Where:
- `density_score = min(1.0, count_within_1000m / 3)`
- `buffer_boost = 0.3` if within 500 m of a defense feature

Assets are labeled as:
- **Likely** (≥ 0.70)
- **Possible** (0.35 – 0.70)
- **Unlikely** (< 0.35)

These labels are **heuristic and indicative**, not deterministic.

---

## Results

Outputs produced by the pipeline:
- `data/real_assets_with_geo_props.geojson` — asset geometries enriched with distance,
  density, scores, and labels.
- `data/real_proxy_scores.csv` — tabular summary.

The demonstration dataset (Netherlands sample) showed:
- No textual disclosures of structural protection in the facility registry.
- Defense-proxy signals driven by OSM-derived defenses (distance + density).

---

## Limitations and Intended Use

- This proxy does **not** confirm the presence, design standard, or effectiveness
  of flood protection at an asset.
- OpenStreetMap completeness varies by region and feature type.
- Buffer distances and scoring weights are heuristic and chosen for transparency
  and interpretability.

Intended as:
- an upstream data-enrichment layer,
- a QA/context signal for analysts and modelers,
- a complement to issuer-level adaptation measures (not a replacement).

---

## Data Sources and Attribution

- **E-PRTR** — European Pollutant Release and Transfer Register.  
- **OpenStreetMap** — flood defense features via Overpass API. © OpenStreetMap contributors (ODbL).

All data in this repository are publicly available or synthetic for demonstration purposes.
