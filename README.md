# High-Dimensional Analysis of the UCI Human Activity Recognition Dataset

A High-Dimensional Probability course project analysing the structure, intrinsic
dimensionality, and geometry of the UCI "Human Activity Recognition Using
Smartphones" dataset (561 features, 10,299 observations). This is **not** a
classification project — the emphasis is on understanding the dataset's
high-dimensional geometry: variance concentration (PCA), intrinsic dimension
estimation, the curse of dimensionality, manifold learning (t-SNE/UMAP),
correlation structure, random projection and the Johnson–Lindenstrauss lemma,
and multivariate outlier detection.

## Project overview

| | |
|---|---|
| Dataset | [UCI HAR Dataset](https://archive.ics.uci.edu/dataset/240/human+activity+recognition+using+smartphones) — 10,299 rows × 561 features, 6 activities, 30 subjects |
| Report | `report/HAR_High_Dimensional_Analysis_Report.docx` / `.pdf` (20 pages) |
| Notebook | `notebooks/HAR_High_Dimensional_Analysis.ipynb` |
| Figures | `figures/` (14 publication-quality PNGs) |
| Slides | `slides/HAR_Presentation.pptx` |
| Source code | `src/` (modular, one file per analysis stage) |

## Installation

```bash
python3 -m venv venv
source venv/bin/activate       # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

Python 3.11+ recommended (developed and tested on 3.12).

## Dataset

The dataset is downloaded automatically by `src/data_acquisition.py` from the
official UCI Machine Learning Repository:

```
https://archive.ics.uci.edu/static/public/240/human+activity+recognition+using+smartphones.zip
```

If your network blocks that host, download the zip manually from the URL
above, save it as `data/raw/har.zip`, and re-run the script — it will detect
the local file and skip the download step.

## Reproduction steps

Run the pipeline in order from the `src/` directory:

```bash
cd src
python3 data_acquisition.py      # download + extract raw UCI files
python3 data_cleaning.py         # merge train/test, validate, build data/processed/har_full.csv
python3 eda.py                   # Figures 1-5, Tables 1-4
python3 pca_analysis.py          # Figures 6-8, Tables 5-6
python3 intrinsic_dimension.py   # Figures 9-10, Tables 7-9
python3 dimensionality_reduction.py   # Figure 11, Table 10
python3 random_projection.py     # Figure 12, Table 11
python3 outlier_detection.py     # Figures 13-14, Tables 12-14
```

All figures are written to `figures/`, all tables to `report/tables/`. Random
seed is fixed at 42 throughout for reproducibility. Or simply open and run
`notebooks/HAR_High_Dimensional_Analysis.ipynb` end to end, which walks
through the same pipeline with inline narrative and figures.

## Repository structure

```
har_project/
├── data/
│   ├── raw/            # UCI HAR files as downloaded (features.txt, X_train.txt, ...)
│   └── processed/      # har_full.csv (tidy, merged), feature_families.csv
├── notebooks/
│   └── HAR_High_Dimensional_Analysis.ipynb
├── figures/             # 14 publication-quality PNG figures
├── src/                  # modular, documented analysis pipeline
│   ├── utils.py
│   ├── data_acquisition.py
│   ├── data_cleaning.py
│   ├── eda.py
│   ├── pca_analysis.py
│   ├── intrinsic_dimension.py
│   ├── dimensionality_reduction.py
│   ├── random_projection.py
│   └── outlier_detection.py
├── report/
│   ├── HAR_High_Dimensional_Analysis_Report.docx
│   ├── HAR_High_Dimensional_Analysis_Report.pdf
│   ├── build_report.js
│   └── tables/           # 14 CSV summary tables cited in the report
├── slides/
│   └── HAR_Presentation.pptx
├── AI_USAGE.md
├── requirements.txt
├── README.md (this file)
└── LICENSE
```

## Key results (summary)

- 80% of total variance is explained by just **27 of 561** principal components.
- Five independent intrinsic-dimension estimators place the manifold's true
  dimensionality between **~1 and ~15**, with the neighbourhood-scaling
  methods (MLE, TwoNN) converging near 14-15 — one to two orders of
  magnitude below the ambient dimension.
- Distance concentration is clearly visible at low random-projection
  dimensionality but plateaus by ~50 dimensions, consistent with the low
  intrinsic dimension.
- t-SNE and UMAP preserve local neighbourhood structure (trustworthiness
  0.97 / 0.95) noticeably better than linear PCA (0.88).
- The Johnson–Lindenstrauss worst-case bound (k≈6,862 for n=3,000, ε=0.1)
  is far more conservative than what empirically suffices (k=300 already
  preserves 98.9% of pairwise distances within ±10%).
- Outlier detection methods disagree substantially in absolute counts but
  agree that **WALKING_DOWNSTAIRS** is disproportionately associated with
  atypical feature vectors (45.8% flagged by at least one method).

Full methodology, figures, tables, and discussion are in the report.

## Citation

Reyes-Ortiz, J., Anguita, D., Ghio, A., Oneto, L., & Parra, X. (2012).
Human Activity Recognition Using Smartphones [Dataset]. UCI Machine Learning
Repository. https://doi.org/10.24432/C54S4K
