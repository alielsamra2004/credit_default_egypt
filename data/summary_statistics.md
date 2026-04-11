# Summary Statistics — Egyptian Retail Credit Dataset
# (Egyptian public bank, 2008–2024, NDA-restricted)

## Dataset Overview
- **Records:** 120,000 loans
- **Period:** Q1-2008 – Q4-2024 (68 quarters)
- **Default rate:** 17.86%
- **Features:** 51 columns (2 identifiers + 48 features + 1 target)

## Era-Level Breakdown

| Era | Period | Records | Default Rate | % of Dataset |
|-----|--------|---------|-------------|--------------|
| Era 1: Pre-Bureau | 2008–2010 | 23,376 | 14.9% | 19.5% |
| Era 2: Bureau Rollout | 2011–2014 | 22,600 | 26.5% | 18.8% |
| Era 3: Digital Banking | 2015–2019 | 41,292 | 15.2% | 34.4% |
| Era 4: Advanced Analytics | 2020–2024 | 32,732 | 17.3% | 27.3% |

## Key EDA Findings

### Default Rate Over Time
- **Peak default year:** 2011 — annual rate 32.7%
- Era 2 (2011–2014) drove the overall peak; coincides with Arab Spring political disruption

### Employment Sector
- **Highest default rate sector:** Unregistered/Unemployed - 49.6%
- **Informal sector (code 3) default rate:** 32.73% 
- **Government sector (code 1) default rate:** 10.73% 
- **Lowest default rate sector:** Government — 10.73%

### Demographics
- **Under-30 default rate:** 25.73%  vs  overall 17.86%  (1.44× uplift)
- **Male default rate:** 18.9%  |  **Female default rate:** 17.2%

### Correlations
- **DTI × Default:** 0.259  (target >0.25) 
- **I-Score × Default:** −0.172  (target <−0.10)

### Structural Missingness — I-Score Trajectory
- 2008: 99% missing → 2024: 42% missing
- Reflects I-Score (Egyptian credit bureau) rollout from 0.5% (2008) to ~60% (2024) coverage

### Era-Level Statistics

| Era | Mean DTI | Mean Loan Amount (nominal EGP) | Informal Sector Share |
|-----|----------|--------------------------------|----------------------|
| 1 | 0.273 | 141,960 | 15.3% |
| 2 | 0.254 | 186,460 | 15.0% |
| 3 | 0.249 | 343,619 | 14.9% |
| 4 | 0.247 | 623,932 | 14.9% |

> Note: loan amounts are nominal EGP — the rise from Era 1 to Era 4 partly reflects
> EGP depreciation (2016 float, 2023–2024 devaluation cycles).

## Model Performance Summary

### AUC by Era

| Model   | Era 1  | Era 2  | Era 3  | Era 4  |
|---------|--------|--------|--------|--------|
| BN_BIC  | 0.6191 | 0.6635 | 0.7085 | 0.7352 |
| BN_K2   | 0.6389 | 0.6775 | 0.7260 | 0.7227 |
| LR      | 0.6863 | 0.6860 | 0.7386 | 0.7258 |
| RF      | 0.6375 | 0.6247 | 0.6856 | 0.6801 |
| NB      | 0.6879 | 0.6859 | 0.7154 | 0.7213 |
| XGB     | 0.6880 | 0.7005 | 0.7427 | 0.7363 |

- **BN_BIC AUC trajectory:** 0.6191 → 0.6635 → 0.7085 → 0.7352 (monotonically increasing — primary model)
- **BN_K2 AUC trajectory:** 0.6389 → 0.6775 → 0.7260 → 0.7227 (robustness alternative)
- **XGBoost** achieves the highest absolute AUC in Era 4 but cannot support probabilistic inference queries
- **DTI_Ratio** is a direct parent of Default in all 4 eras under both BN_BIC and BN_K2 (invariant structural predictor)

## Inference Results (Era 4 BN_BIC)

| Inference Type | Query | Result |
|----------------|-------|--------|
| Baseline | P(Default) unconditional | 0.1644 |
| Diagnostic | Informal sector lift P(Informal \| Default=1) / P(Informal) | 2.33× |
| Diagnostic | DTI high bin lift | 3.16× |
| Diagnostic | Government sector lift | 0.52× |
| Interventional | do(DTI ≤ 0.4) — forced DTI cap | −17.0% relative default reduction |
| Interventional | do(CBE rate hike) | −1.5% relative default reduction |
| Interventional | do(financial inclusion — forced informal lending) | +133.3% relative increase |
