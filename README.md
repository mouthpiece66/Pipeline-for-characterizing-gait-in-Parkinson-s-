# Gait Characterization in Parkinson's Disease

Pipeline for characterizing gait in Parkinson's disease (PD) using trunk-worn wearable
sensors (Axivity AX3), under both **controlled** (short, protocol-based walking) and
**free-living** (multi-day, naturalistic) recording conditions.

Developed as part of the LASIGE Summer Research Program.

## Overview

This project uses [`scikit-digital-health`](https://github.com/PfizerRD/scikit-digital-health)
(`skdh`) to detect walking bouts and extract stride-level and bout-level gait parameters
(pace, rhythm, variability, asymmetry, stability/smoothness) from raw trunk accelerometer
data, and compares these parameters:

- Between individuals with PD and healthy controls, under a short controlled walking
  protocol ("Walking slow", ~2 minutes).
- Across free-living recordings spanning several days per participant, including a
  breakdown by walking-bout duration (5–15 s, 15–30 s, 30–60 s, >60 s).
- Across disease stage (Hoehn & Yahr) and MDS-UPDRS scores, where clinical data was
  available.

**No patient data, identifiable information, or clinical records are included in this
repository.** Only the analysis code is shared — see [Data](#data) below.

## Repository structure

```
.
├── notebooks/
│   ├── Script2.ipynb                      # Main pipeline: controlled PD vs Control dataset
│   ├── Group_descriptive_stats.ipynb      # Group-level descriptive statistics + plots
│   ├── Analisis_FreeLiving.ipynb          # Free-living analysis (CSV/basic .cwa input)
│   ├── FreeLiving_Matched_Analysis.ipynb  # Free-living analysis matched to clinical severity
│   └── FreeLiving_descriptive_stats.ipynb # Free-living descriptive statistics + plots
├── src/
│   └── gait_lib.py                        # Shared functions (I/O, gait-detection wrapper,
│                                           #   bout-duration categorization, download helper)
├── requirements.txt
├── .gitignore
└── README.md
```

## Methods summary

- **Gait detection:** `skdh.gait_old.Gait`, with sensor-orientation correction, a
  minimum bout duration of 6 s, maximum inter-segment separation of 0.25 s, maximum
  stride time of 2.25 s, and a 4th-order 20 Hz low-pass filter.
- **Controlled dataset:** gait extracted from the annotated "Walking slow" interval
  (±5 s margin) of each recording.
- **Free-living dataset:** gait extracted from the full multi-day recording; walking
  bouts additionally categorized by duration (5–15 s / 15–30 s / 30–60 s / >60 s).
- **Quality control:** outliers identified via a ±2 SD / ±4 SD criterion and inspected
  individually (raw-signal review, and cross-validation with an alternative
  gait-detection algorithm, `skdh.gait.GaitLumbar`, where relevant) before exclusion.
- **Statistics:** descriptive only (mean, SD, median) — no inferential tests were
  applied in this phase.

## Data

The datasets used in this project include real participant data (movement recordings
and/or clinical records) and are **not included in this repository**. The notebooks
expect the data to be organized locally (or in a private Google Drive, if run in Colab)
with paths configured at the top of each notebook — see the configuration cell in each
file for the expected folder layout.

## Requirements

```
pip install -r requirements.txt
```

See [`requirements.txt`](requirements.txt). Notebooks were developed and run in Google
Colab; `gait_lib.py` only depends on the packages listed there.

## Acknowledgments

Mentor: Diogo Branco — LASIGE.
