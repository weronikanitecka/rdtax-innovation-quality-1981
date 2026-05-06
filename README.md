# Do R&D Tax Credits Promote High-Quality Innovation?
**Evidence from US Patents, Citations, and Technology Proximity around the introduction of the 1981 R&D Tax Credit**

[![Python 3.10+](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Platform: Colab](https://img.shields.io/badge/Platform-Google%20Colab-F9AB00?logo=googlecolab&logoColor=white)](https://colab.research.google.com/)
[![Data: Kaggle](https://img.shields.io/badge/Data-Kaggle%20USPTO%201963--1999-20BEFF?logo=kaggle&logoColor=white)](https://www.kaggle.com/datasets/appetukhov/patents-and-patent-citations-19631999/data)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Overview

This repository contains replication code for an event-study analysis of the **1981 US federal R&D tax credit** (ERTA). Using a panel of 1.5M USPTO patents (1975–1994) and 16.5M citation edges, it estimates the credit's effect on innovation **quantity** (patent counts) and **quality** (5-year forward citations) via Two-Way Fixed Effects (TWFE) designs. A logistic regression classifier for high-impact patents is also included.

📄 Full paper in `/paper/nitecka_et_al_2025.pdf`

---

## Research Questions

1. Did the credit increase citation intensity beyond its effect on raw patent counts?
2. What is the temporal response pattern — immediate, or lagged 4–10 years?

---

## Methodology

Two complementary TWFE event-study designs around 1981:

- **Design 1 — Territorial:** US corporate assignees (treated) vs. foreign corporate assignees (control)
- **Design 2 — Intensity:** High pre-1981 patenting US firms (top 25th percentile) vs. low pre-1981 patenting US firms

**Specification:**
```
Y_it = Σ_{k ≠ -1} β_k · 𝟙{t − 1981 = k} · Treat_i  +  α_i  +  γ_t  +  ε_it
```
- Patent counts: `log(1 + n)` · Citations: `asinh(mean_fwd_cites_5y)` · SEs clustered at assignee level · Reference year: `k = −1`

---

## Key Results

| | Quantity | Quality |
|---|---|---|
| US vs. foreign | No significant relative gain for US firms | No significant relative gain for US firms |
| High vs. low exposure | Low-exposure firms drove volume growth | High-exposure firms show significant citation gains from ~Year 2 |
| Timing | | Significant effects emerge 4–10 years post-1981 |

The credit broadened the innovator base but primarily stimulated experimental development rather than shifting the technological frontier. Full results, figures, and regression tables in the paper.

---

## Replication

### Data
Download `apat63_99.csv` and `cite75_99.csv` from [Kaggle](https://www.kaggle.com/datasets/appetukhov/patents-and-patent-citations-19631999/data) (free account required).

### Run
**Google Colab (recommended):** upload both CSVs to session root, open `notebooks/analysis.ipynb`.  
**Local:**
```bash
git clone https://github.com/weronikanitecka/rdtax-innovation-quality-1981.git
pip install -r requirements.txt
jupyter notebook notebooks/analysis.ipynb
```
Adjust `PATENTS_PATH` and `CITATIONS_PATH` at the top of the notebook if needed.  
Expected runtime: ~20–40 min (dominated by the citation join over 16M edges).
