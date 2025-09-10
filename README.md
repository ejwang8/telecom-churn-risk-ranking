# Telecom Churn Risk Ranking

Time-boxed churn modeling case study for a telecom dataset. Goal: produce an **actionable ranked list** of customers by churn risk and show impact with **PR-AUC** and **precision@K**.

describe challenge here [help]
include the markdown table with columns and general descriptions

## Results (headline)
- PR-AUC improved from 0.258 → 0.277  
- Precision@10% improved from 26.1% → 31.0% (recall ~39%)  
- Ranked list enables ~3.9× lift at top 10% vs. random outreach  
(Details in slides.)  [Slides: `mock-deliverable.pdf`]

## What’s inside
- `pipeline.ipynb` — cleaned pipeline, model selection, eval
- `mock-deliverable.pdf` — 4 slide PowerPoint for non-technical audience
- (Dataset not included.)

## Scope & timebox
Completed in a day. Emphasis on clarity, explainability, and business action. Extended ideas are in **Next steps**.

## Approach
- Fix leakage & missingness; encode Unknowns; drop redundant features
- Compare simple baselines; select interpretable model (e.g., Logistic Regression)
- Optimize for PR-AUC and **precision@K**; deliver ranked list for outreach

## Next steps
- Calibration & probability thresholds for ops
- Hyperparameter tuning (e.g., XGBoost/CatBoost)
- Drift monitoring; monthly rescoring
