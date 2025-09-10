# Telecom Churn Risk Ranking

**Context.** I inherited a **basic gradient-boosting baseline** for a telecom churn dataset and had **one day** to:
1) harden the **data pipeline** (joins, leakage/missingness, encodings),
2) improve **model quality** with interpretable baselines and calibration, and
3) translate scores into an **actionable business plan** (ranked top-K outreach + ops workflow).

**Goal.** Produce a **ranked list** of customers by churn risk that an ops team can act on immediately, and evaluate impact with **PR-AUC** and **precision@K** (which map to finite outreach capacity).

---

## Results (headline)
- **PR-AUC:** 0.258 → **0.277**  
- **Precision@10%:** 26.1% → **31.0%** (recall ~39%)  
- **Business effect:** ~**3.9×** lift in churn capture at the top 10% vs. random outreach  
Details in slides → `Wang_DS_Challenge.pdf`.

---

## What’s inside
- `Wang_DS_Challenge.ipynb` — cleaned pipeline, baselines vs. GBM, eval & calibration
- `slides/exec-summary.pdf` (or `Wang_DS_Challenge.pdf`) — 3–5 slides for non-technical audience
- **Note:** Source dataset is not included.

---

## Scope & timebox
Completed in **~1 day**. Emphasis on clarity, explainability, and business action. Production hardening & extensions are listed in **Next steps**.

---

## Approach (one-day cut)
**Pipeline**
- Join “Demographics” + “Services” on customer ID; check row loss & duplicates.
- Fix **leakage** (drop post-outcome columns), handle **missingness**, standardize categoricals (Unknown bucket), and remove redundant features.
- Train/validation split with **stratification**; repeated k-fold for stability where feasible.

**Modeling**
- Establish interpretable **baseline** (Logistic Regression with regularization & class weights).
- Compare to inherited **GBM**; perform light tuning within time budget.
- Emphasize **PR-AUC** and **precision@K**; generate **calibrated probabilities** for trustworthy triage.
- Deliver **ranked top-K** list tied to outreach capacity (e.g., 5–15% of book).

**Evaluation for Ops**
- Report PR curves, precision@K table, and confusion metrics at selected thresholds.
- Sanity checks for **segment fairness** and **site/manager confounds** where relevant.

---

## Action plan (how a team would use this tomorrow)
1. **Weekly scoring:** score all active customers; write a `top_k.csv` with `customer_id`, `risk_score`, and key reasons (e.g., tenure, billing deltas, add-on churn signals).
2. **Triage queue:** route top-K to retention agents; show **calibrated risk** + reason codes.
3. **Interventions:** targeted plan/offer, fixable service issues, fee adjustments, contract match.
4. **Measure uplift:** A/B at the **list boundary** (treated vs. holdout) to estimate incremental retention.
5. **Feedback loop:** record outreach outcomes → retrain monthly; monitor drift & calibration.

---

## Data & reproducibility
**No source data is redistributed.** To run the notebook yourself:
1. Provide a CSV/XLSX with the schema below (same column names).  
2. Set `DATA_PATH` at the top of the notebook.  
3. Re-run all cells.

---

## Data schema (neutral)
> Column names listed for clarity; descriptions are generic and not copied from any third-party materials.

### Demographics

| Field | What it represents |
|---|---|
| **Customer ID** | Unique identifier for a customer record. |
| **Gender** | Reported gender
