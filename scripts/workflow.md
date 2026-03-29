# Scripts Notebook Workflows

This file summarizes the workflow and outputs of each notebook in `scripts/`.

## 1) `eda_univariate_bivariate.ipynb`

### Workflow (compressed)
1. Resolve repo root and set paths (`data/raw/ILPD.csv`, figure output folders).
2. Load ILPD with explicit schema/column names.
3. Basic prep:
   - fill missing `Albumin_Globulin_Ratio` with median,
   - map target labels (1/2 → disease/no disease),
   - cast `Gender` as categorical.
4. Univariate EDA:
   - target and gender countplots,
   - numeric histograms (+KDE),
   - numeric boxplots,
   - summary stats (`describe`).
5. Bivariate EDA:
   - gender vs target countplot,
   - feature vs target boxplots,
   - selected pairplot,
   - correlation heatmap (including encoded gender/target).

### Output
- Saved figures under:
  - `reports/figures/eda/univariate/`
  - `reports/figures/eda/bivariate/`
- Typical artifacts include:
  - `categorical_distributions.png`
  - `numeric_histograms.png`
  - `numeric_boxplots.png`
  - `gender_vs_target.png`
  - `numeric_vs_target_boxplots.png`
  - `selected_pairplot.png`
  - `correlation_heatmap.png`

---

## 2) `preprocessing_pipeline.ipynb`

### Workflow (compressed)
1. Resolve repo root; initialize output directories in `data/` and `produced_reports/docs/`.
2. Load raw ILPD and standardize schema.
3. Data-quality checks:
   - missing-value snapshot,
   - invalid/infinite checks,
   - `Gender` encoding,
   - duplicate removal.
4. Missing-value treatment:
   - `Albumin_and_Globulin_Ratio` imputed using group-wise median by `Gender` + global fallback.
5. Outlier assessment report:
   - IQR, Z-score, Modified Z-score counts and percentages (no row deletion).
6. Clinical capping (winsorization-style) using pre-defined plausible limits.
7. Robust scaling for model-ready features (`Result` retained unscaled).
8. Persist datasets and metadata manifest.

### Output
- Data artifacts:
  - `data/interim/ILPD_cleaned.csv`
  - `data/processed/ILPD_clinically_capped.csv`
  - `data/processed/ILPD_robust_scaled_features.csv`
- QA/Governance artifacts:
  - `produced_reports/docs/ILPD_outlier_report.csv`
  - `produced_reports/docs/ILPD_preprocessing_metadata.json`
