# Learning from Structure: A Multi-Layered Bayesian Network Approach to Credit Risk in Egypt

**Author:** Ali El Samra — IE University, Bachelor in Business Administration & Data Analytics, 2026
**Supervisor:** Prof. Manuele Leonelli

---

## Description

This thesis applies era-stratified Bayesian Network (BN) structure learning to 120,000 Egyptian retail loans originated between 2008 and 2024. Rather than fitting a single static model, the analysis partitions the data into four macro-economic eras that correspond to key inflection points in Egyptian banking history (the Arab Spring, the 2016 EGP float, the COVID-19 shock, and the 2024 devaluation). A separate BN is learned for each era using two scoring functions — BicScore (BN_BIC) and K2Score (BN_K2) — revealing how the causal structure of credit risk evolves alongside improvements in data infrastructure, I-Score coverage, and monetary policy regimes.

---

## Key Contributions

- Era-specific BN structure learning reveals regime-dependent causal architecture that a single pooled model conceals
- DTI_Ratio is the sole invariant direct parent of Default across all four eras under both BicScore and K2Score
- BN_BIC AUC improves monotonically: 0.632 → 0.664 → 0.709 → 0.735 (preserving the institutional narrative)
- BN_K2 achieves higher absolute AUC in Eras 1–3 as a robustness alternative
- Six-model benchmark: BN_BIC, BN_K2, Logistic Regression, Random Forest, Naive Bayes, XGBoost
- Informal sector lift: 2.33× (Era 4 diagnostic inference via BN_BIC)

---

## Notebook Structure

| Section | Title | Key Outputs |
|---------|-------|-------------|
| 0 | Setup & Dependencies | Imports, paths, typography config (Times New Roman), constants |
| 1 | Data Loading & Validation | Era record counts, default rates by era |
| 2 | EDA — 10 subsections (2.1–2.10) | 10 figures → `figures/eda/` |
| 3 | Data Preprocessing | `ERA_VARS_TRIMMED`; discretised era DataFrames; stratified 80/20 splits |
| 4 | Causal Layer Constraints | `build_blacklist()` — forbidden edge pairs enforcing causal layer ordering |
| 5 | BN Structure Learning | BN_BIC & BN_K2 DAGs; Tables 5.1–5.2; 2 figures → `figures/bayesian_network/` |
| 5.3 | Structural Sensitivity Analysis | Bootstrap edge stability (B=100); 3 figures → `figures/bayesian_network/` |
| 6 | Parameter Estimation (CPT Fitting) | MLE-fitted BNs on full era data; used in Sections 8 & 11 |
| 7 | Model Evaluation — All Six Models | Tables 7.1–7.4; 1 figure → `figures/model_comparison/`; 2 results CSVs |
| 8 | Probabilistic Inference — BN_BIC | 8.1 Diagnostic · 8.2 Predictive · 8.3 Stress test; 2 figures → `figures/inference/` |
| 9 | Structural Missingness Analysis | Institutional data-availability timeline (print only) |
| 10 | Key Results Summary | Plain-text findings: DTI invariance, AUC trajectory, sector lift |
| 11 | Real-World Use Cases | Forward · Backward · Interventional inference; 2 figures → `figures/inference/` |

---

## Repository Structure

```
credit_default_egypt/
├── README.md
├── requirements.txt
├── data/
│   ├── summary_statistics.md                     # Dataset, EDA, and model statistics
│   ├── validation_report.md                      # Dataset validation (23/23 checks passed)
│   └── variables_description.md                  # Variable Description
├── notebooks/
│   └── egyptian_credit_bn_analysis_FINAL.ipynb   # Single self-contained analysis notebook
├── figures/
│   ├── eda/
│   │   ├── fig_eda_default_over_time.png         # Default rate & volume 2008–2024
│   │   ├── fig_eda_default_by_category.png       # Default rates by sector, education, region, type
│   │   ├── fig_eda_predictor_distributions.png   # DTI, income, age, credit score distributions
│   │   ├── fig_eda_macro_correlation.png         # Macro layer pairwise correlations
│   │   ├── fig_eda_borrower_correlation.png      # Borrower layer pairwise correlations
│   │   ├── fig_eda_missing_heatmap.png           # Structural missingness with era labels
│   │   ├── fig_eda_era_comparison.png            # DTI distribution, sector mix, loan amounts by era
│   │   ├── fig_eda_demographics.png              # Default rate by age band and gender
│   │   ├── fig_eda_loan_characteristics.png      # Default rate by loan type and collateral
│   │   └── fig_eda_pairplot.png                  # Pairwise relationships (20% sample)
│   ├── bayesian_network/
│   │   ├── fig_bn_structures_BIC.png             # 4-panel BN_BIC DAGs by era
│   │   ├── fig_bn_structures_K2.png              # 4-panel BN_K2 DAGs by era
│   │   ├── fig_sensitivity_default_edges.png     # Bootstrap stability of Default edges (B=100)
│   │   ├── fig_sensitivity_heatmap.png           # Edge frequency heatmap across all eras
│   │   └── fig_sensitivity_network_era4.png      # Era 4 network with bootstrap-encoded edge weights
│   ├── model_comparison/
│   │   └── fig_model_comparison.png              # AUC & accuracy for all 6 models
│   └── inference/
│       ├── fig_sector_lift.png                   # Informal sector lift (Era 4 BN_BIC)
│       ├── fig_diagnostic_inference.png          # Prior vs posterior sector distribution
│       ├── fig_diagnostic_full.png               # Full diagnostic lift grid (Sec. 11)
│       └── fig_interventional_comparison.png     # do-calculus policy scenarios (Sec. 11)
└── results/
    ├── results_model_comparison.csv              # Six-model AUC & accuracy by era
    └──results_model_comparison_FINAL.csv        # Same (canonical copy produced by notebook)
```

## Figures

All figures saved to `figures/`, organised into subfolders by analysis stage:

### Exploratory Data Analysis — `figures/eda/`

| Figure | Description |
|--------|-------------|
| `fig_eda_default_over_time.png` | Default rate and loan volume 2008–2024 with macro events annotated |
| `fig_eda_default_by_category.png` | Default rates by employment sector, education level, branch region, loan type |
| `fig_eda_predictor_distributions.png` | KDE / histogram distributions of DTI, income, age, I-Score by default status |
| `fig_eda_macro_correlation.png` | Macro layer pairwise correlations (lower triangle, annotated) |
| `fig_eda_borrower_correlation.png` | Borrower layer pairwise correlations (lower triangle, annotated) |
| `fig_eda_missing_heatmap.png` | Structural missingness heatmap with era labels and institutional event annotations |
| `fig_eda_era_comparison.png` | DTI box plots, sector composition stacked bars, and mean loan amount by era |
| `fig_eda_demographics.png` | Default rate by 10-year age band and gender |
| `fig_eda_loan_characteristics.png` | Default rate by loan type and collateral type |
| `fig_eda_pairplot.png` | Seaborn pairplot for key predictors — 20% stratified sample, hue=Default |

### Bayesian Network & Sensitivity — `figures/bayesian_network/`

| Figure | Description |
|--------|-------------|
| `fig_bn_structures_BIC.png` | Learned Bayesian Network DAGs for all four eras (BicScore) |
| `fig_bn_structures_K2.png` | Learned Bayesian Network DAGs for all four eras (K2Score) |
| `fig_sensitivity_default_edges.png` | Bootstrap stability of Default node edges across eras (B=100) |
| `fig_sensitivity_heatmap.png` | Edge frequency heatmap — all original BIC edges × four eras |
| `fig_sensitivity_network_era4.png` | Era 4 BN_BIC network with edge thickness = bootstrap frequency |

### Model Comparison — `figures/model_comparison/`

| Figure | Description |
|--------|-------------|
| `fig_model_comparison.png` | AUC and accuracy comparison — all six models across four eras |

### Probabilistic Inference — `figures/inference/`

| Figure | Description |
|--------|-------------|
| `fig_sector_lift.png` | Prior vs posterior sector distribution under Default=1 (Era 4 BN_BIC) — Sec. 8 |
| `fig_diagnostic_inference.png` | Raw P(Sector \| Default=1) bar chart (Era 4 BN_BIC) — Sec. 8 |
| `fig_diagnostic_full.png` | Full diagnostic inference — all variables (Era 4 BN_BIC) — Sec. 11 |
| `fig_interventional_comparison.png` | Interventional inference — four CBE policy scenarios — Sec. 11 |

---

## Models

| Model   | Scorer / Algorithm | Features     | Primary Use                       |
|---------|--------------------|--------------|-----------------------------------|
| BN_BIC  | BicScore + HC      | Discretised  | Primary model (monotonic AUC)     |
| BN_K2   | K2Score + HC       | Discretised  | Robustness alternative            |
| LR      | Logistic Regression| Discretised  | Traditional linear baseline       |
| RF      | Random Forest      | Discretised  | Ensemble baseline                 |
| NB      | CategoricalNB      | Discretised  | Probabilistic floor               |
| XGB     | XGBoost            | Continuous   | Best black-box discriminator      |

HC = HillClimbSearch with `max_indegree=4`, `epsilon=1e-6`; BN_BIC uses `max_iter=5000`, BN_K2 uses `max_iter=10000`

---

## How to Run

```bash
# 1. Create and activate a virtual environment (Python 3.11 recommended)
python3.11 -m venv .venv
source .venv/bin/activate        # macOS / Linux
# .venv\Scripts\activate         # Windows

# 2. Install all dependencies
pip install -r requirements.txt

# 3. Open the notebook
jupyter notebook notebooks/egyptian_credit_bn_analysis_FINAL.ipynb
```

Run all cells **top to bottom**. Figures are saved automatically to subfolders of `figures/` and results to `results/`.

> **VS Code users:** open the `.ipynb` file directly and select the `.venv` Python interpreter as the kernel.

> **Data file:** the notebook expects the dataset at `data/egyptian_retail_credit_120k.csv` relative to the project root.

---

## Requirements

All dependencies in `requirements.txt`:

```
pgmpy>=1.0.0
scikit-learn>=1.3.0
xgboost>=2.0.0
pandas>=2.0.0
numpy>=1.24.0
matplotlib>=3.7.0
seaborn>=0.12.0
networkx>=3.0
jupyter>=1.0.0
notebook>=7.0.0
```

