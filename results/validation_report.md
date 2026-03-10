# Egyptian Retail Credit Dataset — Validation Report

## Dataset Overview
- **Total Records:** 120,000
- **Time Period:** Q1-2008 to Q4-2024
- **Variables:** 51 total (50 features + 1 target: Default)
- **Overall Default Rate:** 17.86%

## Default Rates by Period
| Period | Loans | Default Rate | Target | Status |
|--------|-------|--------------|--------|--------|
| 2008-2010 | 23,376 | 14.92% | 7-16% | ✅ |
| 2011-2013 | 14,028 | 29.42% | 16-30% | ✅ |
| 2014-2015 | 17,144 | 16.98% | 12-22% | ✅ |
| 2016-2017 | 12,464 | 25.23% | 18-32% | ✅ |
| 2018-2019 | 20,256 | 10.33% | 8-20% | ✅ |
| 2020-2021 | 10,912 | 13.90% | 10-22% | ✅ |
| 2022-2023 | 14,024 | 17.29% | 10-22% | ✅ |
| 2024 | 7,796 | 22.08% | 8-24% | ✅ |

## Key Correlations
- DTI × Default: 0.259 (target >0.25)
- CreditScore × Default: -0.172 (target <-0.10)
- Young (<30) default rate: 25.73% vs Others: 16.40%
- Government sector default rate: 10.73%
- Informal sector default rate: 32.73%

## Missing Data Summary (Temporal Patterns)
| Variable | Overall Missing | 2008 | 2015 | 2020 | 2022 | 2024 |
|----------|-----------------|------|------|------|------|------|
| Credit_Score_I_Score | 78.3% | 99% | 87% | 72% | 59% | 42% |
| Avg_Monthly_Transactions | 41.5% | 100% | 5% | 5% | 5% | 5% |
| Fraud_Risk_Score | 72.7% | 100% | 100% | 0% | 0% | 0% |
| Estimated_PD | 29.0% | 100% | 0% | 0% | 0% | 0% |
| Employer_Rating | 39.8% | 100% | 3% | 2% | 2% | 2% |
| Profitability_Score | 38.3% | 100% | 0% | 0% | 0% | 0% |

## Notes on Temporal Missing Patterns
- **Credit Score**: I-Score started July 2008 at 0.5% coverage → 60% by 2024. This is NOT a data quality issue — it reflects the actual rollout of the Egyptian credit bureau.
- **Transaction data**: Banks did not have systematic transaction analytics before 2015. Variables are legitimately 100% missing pre-2015.
- **Fraud scoring**: Fraud ML models were not deployed before 2020.
- **Estimated PD**: Internal PD models began limited rollout in 2011, widespread post-2015.

## Validation: 23/23 Checks Passed ✅

## Calibration Notes
- Period adjustments use economic forward-knowledge (loan stress over lifetime) rather than contemporaneous NPL (a lagging indicator)
- 2016-2017 devaluation risk is captured by period adjustment (+0.80/+0.90 logit) not spot NPL
- GDP stored in %-units (e.g. 7.2 = 7.2%) consistent with source Excel
- I-Score coverage peaks at 60% in 2024 per master data (not 75% as sometimes cited)