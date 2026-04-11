# Variable Descriptions — Egyptian Retail Credit Dataset
---

## Layer 0 — Identifiers

| Variable | Type | Description |
|----------|------|-------------|
| `Loan_ID` | string | Unique loan identifier. Format: `EGY-{YEAR}Q{Q}-{NNNNNN}` (e.g., `EGY-2015Q3-042301`). |
| `Origination_Quarter` | string | Calendar quarter of loan origination. Format: `YYYY-QQ` (e.g., `2015-Q3`). Covers Q1-2008 to Q4-2024 (68 quarters). |

---

## Layer 1 — Macro-Economic Context

Assigned deterministically from master data; identical for all loans in the same origination quarter.

| Variable | Type | Units / Range | Description |
|----------|------|---------------|-------------|
| `GDP_Growth_Rate` | float | % (e.g., 7.2 = 7.2%) | Annual real GDP growth rate sourced from CBE/World Bank. Forward-filled from Q1 to Q2–Q4 of the same year. |
| `Inflation_Rate_CPI` | float | Decimal (e.g., 0.118 = 11.8%) | Quarterly CPI headline inflation rate. Peaks above 0.30 in 2017 (post-float) and 2023. |
| `Exchange_Rate_EGP_USD` | float | EGP per USD | Spot exchange rate at quarter-end. ~5.4 in 2008, ~6.9 in 2015, ~18 post-2016 float, ~31 in 2023, ~48 in 2024. |
| `CBE_Interest_Rate` | float | Decimal (e.g., 0.11 = 11%) | Central Bank of Egypt overnight deposit policy rate sourced from the annual interest rate sheet. |
| `Unemployment_Rate` | float | % (e.g., 8.7 = 8.7%) | National unemployment rate from CBE/CAPMAS. Peaks ~13% in 2013–2014. |
| `Political_Stability_Index` | integer | 1–4 | Ordinal stability score. 1 = severe instability (2011 revolution), 2 = high uncertainty (2012–2013), 3 = recovering (2014–2015), 4 = stable (2016+). |

---

## Layer 2 — Branch Characteristics

| Variable | Type | Values | Description |
|----------|------|--------|-------------|
| `Branch_Region` | integer | 1–6 | Geographic region of originating branch. 1 = Greater Cairo (40%), 2 = Alexandria (15%), 3 = Nile Delta (25%), 4 = Upper Egypt (15%), 5 = Suez Canal (4%), 6 = Sinai (1%). Upper Egypt (4) has the highest default rate. |
| `Branch_Urban_Rural` | integer | 0 = Rural, 1 = Urban | Whether the branch is in an urban centre. ~70% urban. Rural branches (+0.10 logit) show slightly elevated default risk. |

---

## Layer 3A — Borrower Demographics

| Variable | Type | Values / Range | Missing | Description |
|----------|------|----------------|---------|-------------|
| `Age` | integer | 18–75 | None | Borrower age at origination. Truncated normal (μ=43, σ=15). Under-30 borrowers have a default rate of 25.7% vs 17.9% overall. |
| `Gender` | integer | 0 = Female, 1 = Male | None | Borrower gender. ~61% male. Male default rate ~18.9% vs female ~17.2%. |
| `Education_Level` | integer | 0–4 | None | Highest completed education level. 0 = Illiterate (5%), 1 = Primary (15%), 2 = Secondary (35%), 3 = University (40%), 4 = Postgraduate (5%). Higher education correlates with higher income. |
| `Marital_Status` | integer | 0–3 | None | 0 = Single (28%), 1 = Married (65%), 2 = Divorced (5%), 3 = Widowed (2%). Affects number of dependents. |
| `Number_of_Dependents` | float | 0–6 | ~2% | Number of financially dependent household members. Poisson-distributed by marital status. Each dependent adds +0.08 logit to default probability. |
| `Housing_Status` | float | 0–2 | ~1% | Current housing arrangement. 0 = Renting (30%), 1 = Owned (55%), 2 = Family-provided (15%). |
| `Job_Sector` | float | 0–5 | ~0.5% | Employment sector. 0 = Unregistered/Unemployed (2%), 1 = Government (30%), 2 = Private Formal (25%), 3 = Informal (15%), 4 = Self-Employed (20%), 5 = Retired (8%). Government workers show the lowest default rate (~10.7%); informal workers ~32.7%. |
| `Monthly_Income_EGP` | float | 2,000–250,000 EGP | None | Gross monthly income in nominal EGP. Inflation-adjusted upward over time; log-normal by sector, age, gender, and region. |

---

## Layer 3B — Financial Behaviour

| Variable | Type | Values | Missing | Description |
|----------|------|--------|---------|-------------|
| `Savings_Level` | float | 0–3 | ~10% | Ordinal savings tier relative to income. 0 = None, 1 = Low, 2 = Medium, 3 = High. Zero savings adds +0.35 logit to default risk. |
| `Checking_Account_Balance` | float | 0–3 | ~15% | Ordinal checking account balance tier. 0 = Near-zero, 1 = Low, 2 = Medium, 3 = High. Zero balance adds +0.25 logit to default risk. |
| `Years_With_Bank` | float | 0–40 | None | Duration of relationship with originating bank (years). Gamma-distributed. Each additional year reduces default logit by −0.04. |
| `Previous_Loans_Count` | integer | 0–5 | None | Number of prior loans at this bank. 0 = 40%, 1 = 30%, 2 = 20%, 3–5 = 10%. Used to calibrate I-Score base score. |

---

## Credit Bureau Variables (I-Score — Structurally Temporal)

Available only for borrowers covered by the Egyptian I-Score bureau. Coverage increases from ~0.5% (2008) to ~60% (2024), creating **structural missingness** that reflects institutional history, not data quality issues.

| Variable | Type | Range | Missing (overall) | Description |
|----------|------|-------|-------------------|-------------|
| `Credit_Score_I_Score` | float | 300–850 | ~78% | I-Score credit bureau score. Higher = better creditworthiness. Corr with Default = −0.172. Missing for ~99% of 2008 loans, ~42% of 2024 loans. |
| `Credit_Inquiry_Count_Last_6M` | float | 0–10 | ~78% | Number of credit inquiries at any bureau member bank in the prior 6 months. >3 inquiries adds +0.20 logit to default risk. |
| `Previous_Rejections_Count` | float | 0–5 | ~78% | Number of prior loan application rejections recorded by the bureau. Any rejection adds +0.45 logit — the strongest bureau signal after I-Score. |
| `Active_Loans_Count_Other_Banks` | float | 0–5 | ~78% | Number of currently active loans at other bureau-member banks. Each additional loan adds +0.12 logit. |
| `Total_Outstanding_Debt_EGP` | float | 0–500,000 EGP | ~78% | Total outstanding debt balance across all bureau-member banks in nominal EGP. Used to compute DTI. |

---

## Transaction Behaviour Variables (Available 2015+)

Derived from digital banking transaction analytics. **100% structurally missing before 2015** — banks did not have systematic transaction data prior to digital banking adoption. ~5% random missingness post-2015.

| Variable | Type | Range | Missing (overall) | Description |
|----------|------|-------|-------------------|-------------|
| `Avg_Monthly_Transactions` | float | 5–200 | ~41% | Average number of bank transactions per month. Higher activity correlates with lower default (−0.003 per transaction). |
| `Unique_ATM_Locations_Count` | float | 0–20 | ~41% | Number of distinct ATM locations used per month. Proxy for geographic mobility and economic activity. |
| `Grocery_Spending_Ratio` | float | 0–1 | ~41% | Share of total card/digital spending at grocery retailers. >0.70 adds +0.30 logit (high grocery share signals financial stress). |
| `Luxury_Mall_Spending_Ratio` | float | 0–1 | ~41% | Share of spending at luxury retail / shopping malls. Low values indicate constrained discretionary spending. |
| `Digital_Wallet_User` | float | 0 = No, 1 = Yes | ~41% | Whether the borrower actively uses a digital wallet (e-wallet/mobile payment). Adoption grows from ~10% (2015–2018) to ~50% (2022+). Digital wallet use reduces default logit by −0.15. |
| `Rent_vs_Own_Detected` | float | 0–2 | ~41% | Housing type inferred from transaction patterns. 0 = Rent detected, 1 = Ownership detected, 2 = Unclear. Corroborates `Housing_Status` for 80% of cases with available data. |

---

## Layer 4 — Employer Context (Available 2015+)

| Variable | Type | Range | Missing (overall) | Description |
|----------|------|-------|-------------------|-------------|
| `Employer_Rating` | float | 1–5 | ~40% | Quality/stability rating of the borrower's employer. 1 = Poor, 5 = Excellent. Government (always 5), Private Formal (3–4), Informal (1–2), Self-Employed (2–3). Null for unemployed/unregistered. **100% missing pre-2015**. Each rating point reduces default logit by −0.15. |
| `Years_At_Current_Employer` | float | 0–40 | ~1% | Tenure at current employer in years. Gamma-distributed by sector. Government workers have longer tenure. Each additional year reduces default logit by −0.02. |

---

## Layer 5 — Loan Characteristics

| Variable | Type | Range | Missing | Description |
|----------|------|-------|---------|-------------|
| `Loan_Type` | integer | 1–3 | None | Product category. 1 = Housing/Mortgage (70%), 2 = Consumer (25%), 3 = Auto (5%). |
| `Loan_Amount_EGP` | float | 5,000–3,000,000 EGP | None | Principal loan amount in nominal EGP. Log-normal, calibrated to annual average loan amounts from master data. Nominal amounts rise across eras due to EGP depreciation. |
| `Duration_Months` | integer | 12–360 | None | Contractual loan term in months. Housing: 60–360 months (mean ~180); Consumer: 12–84 months (mean ~36); Auto: 24–60 months (mean ~48). |
| `Interest_Rate_Annual` | float | 0.08–0.30 | None | Annualised contractual interest rate (decimal). Calibrated from CBE rate sheet by loan type; adjusted ±2–5% by I-Score band. |
| `Monthly_Installment_EGP` | float | Positive EGP | None | Fixed monthly repayment computed via standard annuity formula: `P × r(1+r)^n / ((1+r)^n − 1)`. |
| `Loan_Purpose` | integer | 1–11 | None | Detailed purpose code. Housing: 1=Purchase, 2=Construction, 3=Renovation. Consumer: 4=Education, 5=Medical, 6=Electronics, 7=Travel, 8=Wedding, 9=Other. Auto: 10=New Vehicle, 11=Used Vehicle. |
| `LTV_Ratio` | float | 0.60–0.90 | ~75% | Loan-to-value ratio for housing loans only. NaN for consumer and auto loans. Values above 0.75 add +0.80 logit per unit to default risk. |
| `DTI_Ratio` | float | 0.20–0.60 | None | **Debt-to-income ratio** = (monthly installment + estimated debt service) / monthly income. The single strongest predictor of default (corr = +0.259; coefficient = +5.00 in logit model). Invariant direct parent of Default in all four era BN structures. |
| `Collateral_Type` | integer | 0–4 | None | Type of collateral pledged. 0 = None, 1 = Real Estate (all housing loans), 2 = Vehicle (all auto loans), 3 = Salary Assignment (~70% of consumer), 4 = Personal Guarantor (~20% of consumer). |

---

## Layer 6 — Risk Scoring & Origination Metadata

| Variable | Type | Range | Missing (overall) | Description |
|----------|------|-------|-------------------|-------------|
| `Estimated_PD` | float | 0.01–0.50 | ~29% | Bank's internal probability-of-default estimate at origination. **100% missing pre-2011** (no internal PD models existed); ~50% missing 2011–2014 (limited rollout); near-complete post-2015. Based on logistic model using age, sector, I-Score, DTI, and inflation. |
| `Fraud_Risk_Score` | float | 0–100 | ~73% | Output of bank's fraud detection ML model (0 = low risk, 100 = high risk). **100% missing pre-2020** — fraud ML systems were not deployed before 2020. ~5% of post-2020 loans have scores 50–100 (flagged). |
| `Income_Source_Verified` | float | 0 = No, 1 = Yes | ~10% | Whether the borrower's stated income was independently verified (payslip, tax record, etc.). Verification probability varies by sector: Government ~95%, Formal Private ~70%, Informal ~10%. ~60% missing pre-2015. |
| `Application_Channel` | integer | 1–4 | None | Channel through which the application was submitted. 1 = Branch (dominant pre-2015), 2 = Online/App (growing from 2015), 3 = Agent/Broker, 4 = Other/Partnership. |
| `Profitability_Score` | float | −0.05–0.10 | ~38% | Bank's expected net profitability as a ratio of loan amount, computed as `(interest income + fees − cost of funds − expected loss) / loan amount`. **100% missing pre-2015**. |
| `Approval_Decision_Internal` | integer | 1–2 | None | Internal credit decision tier. 1 = Straightforward Approval (government worker, high I-Score, or DTI < 0.35), 2 = Standard Committee Review. |

---

## Layer 7 — Target Variable

| Variable | Type | Values | Description |
|----------|------|--------|-------------|
| `Default` | integer | 0 = No Default, 1 = Default | Binary outcome: whether the loan defaulted (90+ days past due) over its lifetime. Overall rate = **17.86%**. Generated via logistic model with calibrated period adjustments to match Egyptian macro-economic cycles. |

---

## Structural Missingness Summary

Missingness is **by design**, reflecting real institutional and technological developments:

| Variable Group | Missing Rate | Available From | Reason |
|----------------|-------------|----------------|--------|
| I-Score bureau variables | ~78% overall | ~2008 (sparse) | Egyptian I-Score bureau launched July 2008 at 0.5% coverage; reaches 60% by 2024 |
| Transaction behaviour variables | ~41% overall | 2015 | Digital banking analytics not systematically available before 2015 |
| `Employer_Rating`, `Profitability_Score` | ~40%, ~38% | 2015 | Internal employer rating and profitability models deployed post-2015 |
| `Estimated_PD` | ~29% | 2011 (partial) | Internal PD models absent pre-2011; limited deployment 2011–2014 |
| `Fraud_Risk_Score` | ~73% | 2020 | Fraud ML systems deployed in 2020 |
| `Income_Source_Verified` | ~10% | 2008 (sparse) | Verification infrastructure limited pre-2015 (~60% missing); near-universal post-2015 |

---

## Era Labels

The dataset is partitioned into four modelling eras corresponding to structural breaks in Egyptian banking:

| Era | Years | Label | Key Event |
|-----|-------|-------|-----------|
| Era 1 | 2008–2010 | Pre-Bureau | Pre-revolution; limited credit infrastructure |
| Era 2 | 2011–2014 | Bureau Rollout | Arab Spring; I-Score expansion; political transition |
| Era 3 | 2015–2019 | Digital Banking | EGP float (2016); IMF programme; digital adoption |
| Era 4 | 2020–2024 | Advanced Analytics | COVID-19; 2022–2024 devaluation cycles; fraud ML |
