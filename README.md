# Learning from Structure: A Multi-Layered Bayesian Network Approach to Credit Risk in Egypt

**Author:** Ali El Samra — IE University, Bachelor in Business Administration & Data Analytics, 2026

---

## Description

This thesis applies era-stratified Bayesian Network (BN) structure learning to 120,000 Egyptian retail loans originated between 2008 and 2024. Rather than fitting a single static model, the analysis partitions the data into four macro-economic eras that correspond to key inflection points in Egyptian banking history (the Arab Spring, the 2016 EGP float, the COVID-19 shock, and the 2024 devaluation). A separate BN is learned for each era using the Hill-Climbing algorithm, revealing how the causal structure of credit risk evolves alongside improvements in data infrastructure, I-Score coverage, and monetary policy regimes.

Key contributions:
- Era-specific structure learning captures structural change that a single pooled model cannot
- Informal-sector employment emerges as a high-lift risk signal (2.33× among defaulters)
- BN AUC improves monotonically across eras (0.632 → 0.735) as predictor quality improves
- DTI_Ratio is the single direct parent of Default in all four learned graphs

---

## Repository Structure

```
credit_default_egypt/
├── README.md
├── data/
│   └── egyptian_retail_credit_120k.csv   # Synthetic dataset (120k rows × 51 cols)
├── notebooks/
│   └── egyptian_credit_bn_analysis.ipynb # Full analysis: EDA → BN learning → evaluation
├── figures/
│   └── *.png                             # All generated figures
└── results/
    ├── validation_report.md              # Dataset validation (23/23 checks passed)
    ├── results_model_comparison.csv      # AUC / F1 / precision / recall by model & era
    └── summary_statistics.md            # High-level dataset and model statistics
```

---

## Requirements

```
pgmpy
scikit-learn
pandas
numpy
matplotlib
seaborn
networkx
```

Install all dependencies:

```bash
pip install pgmpy scikit-learn pandas numpy matplotlib seaborn networkx
```

---

## How to Run

```bash
# 1. Activate your virtual environment
source .venv/bin/activate   # macOS / Linux

# 2. Install requirements
pip install pgmpy scikit-learn pandas numpy matplotlib seaborn networkx

# 3. Open the notebook
jupyter notebook notebooks/egyptian_credit_bn_analysis.ipynb
```

Run all cells from top to bottom. Figures are saved automatically to `figures/` and results to `results/`.
