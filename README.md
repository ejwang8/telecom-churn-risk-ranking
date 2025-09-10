# Telecom Churn Risk Ranking

I inherited a **basic gradient-boosting baseline** for a telecom churn dataset and had a day to:
1) harden the **data pipeline** (joins, leakage/missingness, encodings),
2) improve **model quality** with interpretable baselines and calibration, and
3) translate scores into an **actionable business plan** (ranked top-K outreach).

**Goal.** Produce a **ranked list** of customers by churn risk that an ops team can act on immediately, and evaluate impact with **PR-AUC** and **precision@K** (which map to finite outreach capacity).

---

## Results (headline)
- **PR-AUC:** 0.258 → **0.277**  
- **Precision@10%:** 26.1% → **31.0%** (recall ~39%)  
- **Business effect:** ~**3.9×** lift in churn capture at the top 10% vs. random outreach  
Details in slides → `mock-deliverable.pdf`.

---

## What’s inside
- `pipeline.ipynb` — cleaned pipeline, baselines vs. GBM, eval & calibration
- `mock-deliverable.pdf` — 4 slide PowerPoint for non-technical audience
- **Note:** Source dataset is not included.

---

## Scope & timebox
Completed in a day. Emphasis on clarity, explainability, and business action. Production hardening & extensions are listed in **Next steps**.

---

## Approach
**Pipeline**
- Join “Demographics” + “Services” on customer ID; check row loss & duplicates.
- Fix **leakage** (drop post-outcome columns), handle **missingness**, standardize categoricals (Unknown bucket), and remove redundant features.
- Train/validation split with **stratification**; repeated k-fold for stability where feasible.

**Modeling**
- Establish interpretable **baseline** (Logistic Regression with regularization & class weights).
- Compare to inherited **GBM**; perform light tuning within the one-day budget.
- Emphasize **PR-AUC** and **precision@K**; generate **calibrated probabilities** for trustworthy prioritization.
- Deliver a **ranked top-K list** aligned with weekly contact capacity (e.g., 5–15% of the customer base).

**Evaluation (decision-focused)**
- Report **PR curves**, a **precision@K table** (for K = 5%, 10%, 15%), and **confusion metrics** at selected thresholds.
- Include a brief **calibration check** (binned predicted vs. observed churn) to confirm probabilities are usable for prioritizing outreach.
- Sanity checks: leakage guards, class balance.

---

## Action plan (how a team would use this tomorrow)
1. **Weekly scoring:** score all active customers; write a `top_k.csv` with `customer_id`, `risk_score`, and key reasons (e.g., tenure, billing deltas, add-on churn signals).
2. **Triage queue:** route top-K to retention agents; show **calibrated risk** + reason codes.
3. **Interventions:** targeted plan/offer, fixable service issues, fee adjustments, contract match.
4. **Measure uplift:** A/B at the **list boundary** (treated vs. holdout) to estimate incremental retention.
5. **Feedback loop:** record outreach outcomes → retrain monthly; monitor drift & calibration.

---
## Data & reproducibility
**No source data is redistributed.** This notebook expects a local file with the schema listed below.

To run on your machine:
1) Place your CSV/XLSX anywhere locally.
2) In the first path cell of `pipeline.ipynb`, change the hard-coded path
   (`/Users/...`)
   to point to your local file.
3) Run all cells.

---

## Data schema
> Column names listed for clarity with generic descriptions.

### Demographics

| Field | What it represents |
|---|---|
| **Customer ID** | Unique identifier for a customer record (joins to Demographics). |
| **Month** | Calendar month associated with the record (e.g., 1–12). |
| **Tenure in Months** | Months since the customer started service. |
| **Offer** | Marketing/plan offer the customer accepted, if any. |
| **Offer_ID** | Coded identifier for the accepted offer. |
| **Phone Service** | Whether the customer has phone service with the company. |
| **Multiple Lines** | Whether the account includes multiple phone lines. |
| **Number of Lines** | Count of active phone lines on the account. |
| **Internet Service** | Whether the customer has internet service with the company. |
| **Internet Type** | Type of internet connection (e.g., DSL/Fiber/None). |
| **Online Security** | Subscription to an online security add-on. |
| **Online Backup** | Subscription to an online backup add-on. |
| **Device Protection Plan** | Subscription to a device protection plan. |
| **Premium Tech Support** | Subscription to premium technical support. |
| **Unlimited Data** | Whether the plan includes unlimited data. |
| **Contract** | Contract term category (e.g., month-to-month, 1-year, 2-year). |
| **Paperless Billing** | Whether the customer uses paperless billing. |
| **Payment Method** | How the customer pays (e.g., bank transfer, credit card, check). |
| **MRC_Phone_Base** | Monthly recurring charge (MRC) for base phone service. |
| **MRC_MultipleLine** | MRC associated with multiple-line phone service. |
| **MRC_InternetService** | MRC for internet service. |
| **MRC_OnlineSecurity** | MRC for online security add-on. |
| **MRC_OnlineBackUp** | MRC for online backup add-on. |
| **MRC_Protection** | MRC for device protection plan. |
| **MRC_Tech** | MRC for premium technical support. |
| **MRC_Data** | MRC for data/unlimited data component. |
| **MRC_SubTotal** | Sum of component MRCs before discounts/adjustments. |
| **MRC_TotalCharge_ContractDiscount** | Total monthly charge after contract-based discounts. |
| **MRC_TotalCharge_PaymentTypeDiscount** | Total monthly charge after payment-type discounts. |
| **MRC_TotalCharge** | Final total monthly charge billed to the customer. |

> **Target label:** The churn outcome used for modeling exists in the source data but is not redistributed here. When running locally, include a churn label (Yes/No) or adapt the notebook to your label.

