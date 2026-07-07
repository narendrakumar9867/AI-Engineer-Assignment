# AI Engineer Assignment: Agentic ML/DL Experiment Copilot


## Context

ANDLAKE is building AI-assisted workflows where users can explore data, run experiments, understand model behavior, and receive recommendations through an assistant-style interface.

This assignment must run **fully locally** — no production credentials, no production data, no changes to the existing ANDLAKE codebase.

---

## Problem Statement

Build an **Agentic ML/DL Experiment Copilot**.

The copilot accepts:
1. A dataset (CSV)
2. A natural language experiment goal

It then autonomously plans and runs an ML experiment, evaluates the outcome, self-corrects if needed, and produces an explainable final report.

**Example goals:**
```text
Train a model to predict customer churn and explain which features are driving churn.
Train a model to detect fraudulent transactions and show risky records with reasons.
```

**Agent flow:**
```text
User goal
   |
   v
Understand task & constraints -> Inspect dataset & quality
   |
   v
Generate plan -> Validate plan
   |
   v
Select tools/models dynamically -> Run experiments (ML + DL)
   |
   v
Evaluate + statistical comparison -> Critique outcomes
   |
   v
Self-correct / re-plan if needed
   |
   v
Explain behavior + UI report
```

---

## Dataset

Public or synthetic tabular dataset (multimodal optional):
- ≥ 500 rows
- Numeric + categorical columns
- One clear target column
- Some missing/noisy values

Suggestions: customer churn, fraud detection, loan default, ticket priority, sales demand. **No production data.**

---

## Required User Flow

1. User uploads/selects a dataset
2. User enters a natural language goal
3. Copilot inspects schema and data quality
4. Copilot proposes the plan: task type, target, features, ignored columns, metrics, candidate models
5. Copilot validates the plan
6. Copilot trains **at least 2 classical ML models + 1 deep learning model**
7. Copilot compares results and applies a statistical test for key model choices
8. Copilot explains why the best model was selected
9. Copilot explains feature impact and one individual prediction
10. UI displays metrics, charts, risky records/predictions, and a final summary
11. Copilot demonstrates **at least one self-correction or re-planning event**

---

## Example Questions the Copilot Should Handle

- Which model performed better, and why?
- Why were these metrics chosen?
- Which features matter most?
- Show the confusion matrix / error slices.
- Show risky transactions with explanations.
- Explain this specific prediction.
- What data quality issues were found?
- Where could this agent be wrong?
- What should improve before production use?
- Summarize the results for a manager.

---

## Required Deliverables

### 1. Research & Design Note
Dataset choice, prediction problem and relevance, target/feature reasoning, preprocessing, models compared (ML + DL), metric rationale, explainability method, limitations, future improvements. Written for both ML and product engineers.

### 2. Agentic Planner
Rule-based, LLM-based, or hybrid. Outputs a structured plan with reasoning for target/feature/model/metric choices.

```json
{
  "task_type": "classification",
  "target_column": "is_churned",
  "feature_columns": ["monthly_spend", "support_tickets", "usage_days", "plan_type"],
  "ignored_columns": ["customer_id"],
  "models_to_train": ["LogisticRegression", "RandomForestClassifier", "TabTransformer"],
  "metrics": ["f1", "roc_auc", "pr_auc"],
  "explainability": "feature_importance_and_local_explanations"
}
```

### 3. Plan Validator
Checks: target/feature columns exist, target not reused as a feature, ID/leakage-prone columns excluded, valid target classes, correct handling of numeric/categorical types, missing values addressed, models supported, metric-task compatibility. Invalid plan → clear error + suggested fix.

### 4. Training Pipeline
At least **2 classical ML models + 1 deep learning model**. Includes train/validation/test split, preprocessing, training, metrics, comparison, and a justified best-model selection.

- Classification: accuracy, precision, recall, F1, confusion matrix, AUROC/PR-AUC
- Regression: MAE, RMSE, R²

### 5. Explainability
Minimum: feature importance (tree) or coefficients (linear), plus explanation of one selected prediction. Optional: SHAP, permutation importance. Must be understandable and decision-useful, not just raw numbers.

### 6. RAG Integration
Index a small set of internal references (e.g., metric guides, model cards, failure notes). Retrieve relevant context before at least one key decision, log the query + evidence used, and show at least one case where retrieval changed the decision.

### 7. Reflection & Self-Correction
At least one critique step and one re-plan/self-correction event, with stated rationale for the change. Optional: a human-escalation trigger based on confidence/risk.

### 8. UI Demo
Dataset preview → schema/types → generated plan → validation result → experiment trace → model comparison (ML vs DL) → explainability → prediction/risk explorer → final summary and limitations.

Build with React, Next.js, Streamlit, or Gradio — doesn't need to match ANDLAKE's production design, but should be clean and demoable.

---

## Suggested Architecture

```text
Frontend UI
     |
     v
Backend API / Orchestrator
     |
     v
Planner --> Validator
     |
     v
Tool Router (EDA / Training / Evaluation / Explainability / RAG)
     |
     v
Reflection + Self-Correction Loop
     |
     v
Structured Result + Artifacts + Report
     |
     v
UI
```

---

## Expected Structured Response

```json
{
  "experiment_goal": "Predict customer churn",
  "plan": {
    "task_type": "classification",
    "target_column": "is_churned",
    "feature_columns": ["monthly_spend", "support_tickets", "usage_days", "plan_type"],
    "models_to_train": ["LogisticRegression", "RandomForestClassifier", "TabTransformer"],
    "metrics": ["f1", "roc_auc", "pr_auc"]
  },
  "validation": {
    "status": "passed",
    "warnings": ["Class imbalance detected; threshold tuning recommended."]
  },
  "experiments": [
    { "name": "LogisticRegression", "f1": 0.76, "roc_auc": 0.84 },
    { "name": "RandomForestClassifier", "f1": 0.82, "roc_auc": 0.89 },
    { "name": "TabTransformer", "f1": 0.81, "roc_auc": 0.90 }
  ],
  "statistical_comparison": {
    "method": "paired bootstrap",
    "result": "RandomForest significantly better than LogisticRegression at alpha=0.05"
  },
  "selected_model": {
    "name": "RandomForestClassifier",
    "reason": "Best F1/recall tradeoff with strong stability and lower complexity than DL baseline."
  },
  "self_correction_log": [
    "Switched primary metric from accuracy to F1/PR-AUC after imbalance detection."
  ],
  "rag_evidence_used": [
    "Retrieved metric guidance recommending PR-AUC for imbalanced binary classification."
  ],
  "summary": "Random Forest performed best overall. Support tickets and usage days were key churn signals."
}
```

---

## Evaluation Criteria

| Area | What To Check |
|---|---|
| Research thinking | Is the problem framed correctly with clear tradeoffs? |
| Agentic design | Planner + validator + tool execution + reflection loop present? |
| ML vs DL judgment | Are model choices and comparisons justified? |
| Mathematical rigor | Are key decisions statistically defended? |
| Data handling | Missing values, leakage, schema robustness handled? |
| Explainability | Can the system explain global and local behavior clearly? |
| RAG quality | Is retrieval meaningful and decision-impactful? |
| UI quality | Is the demo understandable and reviewer-friendly? |
| Code quality | Readable, modular, reproducible? |
| Honesty | Are limitations and residual risks clearly stated? |

---

## Good Signs
Clear experiment objective · non-linear agent behavior · plan validated before execution · ≥2 ML + 1 DL model comparison · statistically supported decisions · at least one self-correction event · at least one meaningful RAG-influenced decision · clean UI · reproducible setup · honest limitations.

## Red Flags
Hardcoded metrics · no real train/validation/test rigor · only one model or paradigm · no validation layer · no statistical test · no explanation of a specific prediction · no run instructions · production secrets required · inflated claims without evidence.


## What Not To Do
Modify production ANDLAKE code · use production credentials · depend on inaccessible private services · skip validation/guardrails · optimize only the UI while ignoring ML rigor · hide model limitations.


## Time & Submission

**Recommended time: 2 days.**

**Minimum acceptable version:** one dataset, planner, validator, ≥2 ML + 1 DL models, metrics and comparison, one explainability output, one self-correction event, basic UI, clear README.

**Strong version:** robust agent trace, strong statistical rigor, meaningful RAG decision impact, practical failure-mode analysis, polished demo and communication.

```text
project/
  README.md
  research-note.md
  data/
  backend/ or app/
  frontend/ or ui/
  report/
```

README should include: setup steps, run commands, dataset details, example goals, screenshots, model explanation, known limitations.
