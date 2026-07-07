# AI Engineer Assignment: Agentic ML/DL Experiment Copilot (Research-Grade)

## Context

ANDLAKE is moving toward AI-assisted workflows where users can explore data, run experiments, understand model behavior, and receive useful recommendations through an assistant-style interface.

This assignment is designed for AI engineers with strong ML/DL foundations. It evaluates more than model training. It checks whether the engineer can think like an AI engineer in a product platform:

- understand a data problem from a natural language objective
- design and validate an experiment plan
- compare classical ML and deep learning approaches
- justify key decisions mathematically and statistically
- build an agentic workflow with reflection and self-correction
- use retrieval (RAG) to improve decisions
- deliver a working UI and communicate limitations clearly

The assignment must run locally. It must not require production credentials, production data, or changes to the existing ANDLAKE codebase.

## Problem Statement

Build an **Agentic ML/DL Experiment Copilot**.

The copilot should accept:
1. a dataset (CSV), and
2. a natural language experiment goal,

then autonomously plan and execute an ML experiment, evaluate outcomes, self-correct when needed, and generate an explainable final report.

Example goals:

```text
Train a model to predict customer churn and explain which features are driving churn.
```

```text
Train a model to detect fraudulent transactions and show risky records with reasons.
```

The copilot should behave like an adaptive agent:

```text
User goal
        |
        v
Understand task and constraints
        |
        v
Inspect dataset and quality
        |
        v
Generate plan
        |
        v
Validate plan
        |
        v
Select tools and models dynamically
        |
        v
Run experiments (ML + DL)
        |
        v
Evaluate + statistical comparison
        |
        v
Critique outcomes
        |
        v
Self-correct / re-plan if needed
        |
        v
Explain behavior + produce UI report
```

## Why This Fits ANDLAKE

This assignment aligns with ANDLAKE because it combines:

- **Analytics Agent**: natural language goal to structured analytical action.
- **Notebook workflows**: exploration, experimentation, and reporting.
- **ML platform workflows**: training, evaluation, comparison, and explainability.
- **Agentic systems**: planner, validator, dynamic tool use, reflection, and final synthesis.
- **Reliable AI behavior**: guardrails, uncertainty handling, and escalation logic.

This is stronger than a simple model training assignment because it evaluates technical depth, system thinking, and product usability together.

## Dataset

Candidate may use public or synthetic tabular datasets (multimodal is optional).

Recommended options:
- customer churn prediction
- fraud transaction detection
- loan default prediction
- support ticket priority prediction
- sales demand prediction

Dataset should include:
- at least 500 rows
- numeric columns
- categorical columns
- one clear target column
- some missing/noisy values

Production data should not be used.

## Required User Flow

The demo should support:

1. User uploads or selects a dataset.
2. User enters an experiment goal in natural language.
3. Copilot inspects schema and data quality.
4. Copilot proposes the ML task:
   - classification or regression
   - target column
   - feature columns
   - ignored columns
   - metrics and candidate models
5. Copilot validates the plan.
6. Copilot trains at least 2 classical ML models and 1 DL model.
7. Copilot compares model results.
8. Copilot applies statistical comparison for key model choices.
9. Copilot explains why one model is selected.
10. Copilot shows explanations for feature impact and one prediction.
11. UI displays metrics, charts, risky records/predictions, and final summary.
12. Copilot shows at least one self-correction or re-planning event.

## Example Questions The Copilot Should Handle

After training, user should be able to ask:

- Which model performed better and why?
- Why did you choose these metrics?
- Which features are most important?
- Show confusion matrix and error slices.
- Show risky fraud transactions with explanations.
- Explain this prediction.
- What data quality issues did you find?
- Where can this agent be wrong?
- What should improve before production use?
- Summarize experiment results for a manager.

## Required Deliverables

### 1) Research and Design Note

Candidate should submit a technical note explaining:

- selected dataset
- prediction problem and relevance
- target and feature choices
- preprocessing approach
- models compared (ML + DL)
- evaluation metrics and rationale
- explainability method
- limitations and future improvements

The note should be understandable to both ML and product engineers.

### 2) Agentic Planner

Implement a planner (rule-based, LLM-based, or hybrid) that outputs a structured plan.

Example:

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

Planner must also explain why target/features/models/metrics were selected.

### 3) Plan Validator

Validate plan before training.

Validation must check:

- target column exists
- feature columns exist
- target not used as feature
- ID or leakage-prone columns excluded
- classification target has valid classes
- numeric/categorical columns handled correctly
- missing values handled
- selected models supported
- metric-task compatibility valid

If invalid, show clear errors and suggested fixes.

### 4) Training Pipeline

Train at least:
- **2 classical ML models**
- **1 deep learning model**

Pipeline should include:

- train/validation/test split
- preprocessing
- model training
- metric calculation
- model comparison
- best model selection with reasoning

For classification, include:
- accuracy
- precision
- recall
- F1
- confusion matrix
- AUROC or PR-AUC (as appropriate)

For regression, include:
- MAE
- RMSE
- R²

### 5) Explainability

Minimum:
- tree model feature importance
- linear model coefficients (if used)
- explanation of one selected prediction

Optional:
- SHAP
- permutation importance
- local explanation for selected row

Explanation must be understandable and decision-useful.

### 6) RAG Integration

Include a meaningful RAG component:

- index internal references (e.g., metric guides, model cards, failure notes)
- retrieve context before key decisions
- log retrieval query + evidence
- show at least one case where retrieval changed a decision

### 7) Reflection and Self-Correction

System must show:
- at least one critique step
- at least one re-plan/self-correction event
- rationale for strategy change
- optional human escalation trigger based on confidence/risk

### 8) UI Demo

UI should show:
- dataset preview
- schema and data types
- generated plan
- validation result
- experiment trace
- model comparison table
- selected best model + reason
- metrics and charts
- feature importance
- prediction explanations
- final summary + limitations

UI can be built using:
- React
- Next.js
- Streamlit
- Gradio

UI does not need to match ANDLAKE production design, but it should be clean and demoable.

## Suggested UI Layout

```text
Agentic ML/DL Experiment Copilot

1. Dataset Upload
2. Experiment Goal
3. Generated Plan
4. Validation Result
5. Experiment Trace
6. Model Comparison (ML vs DL)
7. Explainability
8. Prediction/Risk Explorer
9. Final Summary and Limitations
```

## Suggested Architecture

```text
Frontend UI
        |
        v
Backend API / Orchestrator
        |
        v
Planner --> Validator
   |          |
   v          v
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

## Evaluation Criteria

| Area | What To Check |
|---|---|
| Research thinking | Is the problem framed correctly with clear tradeoffs? |
| Agentic design | Planner + validator + tool execution + reflection loop present? |
| ML vs DL judgment | Are model choices and comparisons justified? |
| Mathematical rigor | Are key decisions mathematically/statistically defended? |
| Data handling | Missing values, leakage, schema robustness handled? |
| Explainability | Can system explain global and local behavior clearly? |
| RAG quality | Is retrieval meaningful and decision-impactful? |
| UI quality | Is demo understandable and reviewer-friendly? |
| Code quality | Is code readable, modular, and reproducible? |
| Honesty | Are limitations and residual risks clearly stated? |

## Good Signs

A strong submission shows:

- clear experiment objective
- non-linear agent behavior
- plan validation before execution
- at least 2 ML + 1 DL model comparison
- statistically supported decision-making
- at least one self-correction event
- at least one meaningful RAG-influenced decision
- clean UI and readable outputs
- reproducible setup and clear documentation
- honest limitations

## Red Flags

Warning signs:

- hardcoded model metrics
- no true train/validation/test rigor
- only one model or one paradigm only
- no validation layer
- no statistical test
- no explanation of one specific prediction
- no run instructions
- production secrets required
- inflated claims without evidence

## Optional Stretch Work

If candidate finishes early:

- multi-agent setup (Planner/Critic/Executor)
- model artifact versioning and reload
- experiment history dashboard
- follow-up chat Q&A over experiment outputs
- drift simulation and retraining trigger
- uncertainty-aware abstention policy
- manager-ready natural language report
- notebook-based reproducibility report

## What Not To Do

Candidate should not:

- modify production ANDLAKE code
- use production credentials
- depend on inaccessible private services
- skip validation/guardrails
- optimize only UI and ignore ML rigor
- hide model limitations

## Expected Time

Recommended time: **2 days**.

## Minimum Acceptable Version

- one dataset
- planner
- validator
- at least 2 ML + 1 DL models
- metrics and comparison
- one explainability output
- one self-correction event
- basic UI
- clear README with run steps

## Strong Version

- robust agent trace
- strong statistical rigor
- meaningful RAG decision impact
- practical failure-mode analysis
- polished demo and communication

## Final Submission Format

```text
project/
  README.md
  README_TASK.md
  README_UNDERSTANDING.md
  data/
  backend/ or app/
  frontend/ or ui/
  research-note.md
  report/
```

README should include:

- setup steps
- run commands
- dataset details
- example goals
- screenshots
- model explanation
- known limitations

## Why This Is A Strong Assignment

This assignment is strong because it checks both research depth and product-grade AI engineering.

It verifies whether the candidate can:

- design a sound ML experiment
- compare ML and DL approaches honestly
- validate and correct AI-generated plans
- justify decisions mathematically
- explain model behavior clearly
- build an agentic workflow with reliability features
- communicate outcomes and limitations responsibly

This directly reflects AI engineering needs in ANDLAKE: not only training models, but converting model workflows into reliable assistant-driven product experiences.
