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
