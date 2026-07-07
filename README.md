# AI Engineer Assignment: Agentic ML Experiment Copilot

## Context

ANDLAKE is moving toward AI-assisted workflows where users can work with data, run experiments, understand models, and get useful explanations through an assistant-style interface.

This assignment is for an AI engineer with machine learning and deep learning research background. The assignment should test more than model training. It should test whether the engineer can think like an AI engineer inside a product platform:

- understand a data problem
- design an ML experiment
- train and compare models
- explain model behavior
- build an agentic workflow
- show the result in a working UI
- communicate limitations clearly

The assignment should run locally. It should not require production credentials, production data, or changes to the existing ANDLAKE codebase.

## Problem Statement

Build an **Agentic ML Experiment Copilot**.

The copilot should take a dataset and a natural language experiment goal from the user, then plan and execute a small ML experiment.

Example user goal:

```text
Train a model to predict customer churn and explain which features are driving churn.
```

or:

```text
Train a model to detect fraudulent transactions and show risky records with reasons.
```

The copilot should not only train a model. It should behave like a small agent:

```text
User goal
        |
        v
Understand task
        |
        v
Inspect dataset
        |
        v
Choose target and features
        |
        v
Train baseline model
        |
        v
Train stronger model
        |
        v
Compare metrics
        |
        v
Explain model behavior
        |
        v
Generate UI report
```

## Why This Fits ANDLAKE

This assignment is aligned with ANDLAKE because it connects these product directions:

- **Analytics Agent**: natural language question to structured answer.
- **Notebook workflows**: data exploration, experiment execution, and result reporting.
- **ML platform workflows**: training, evaluation, comparison, and model explanation.
- **Agentic systems**: planner, validator, tool execution, and final response.

This is a stronger assignment than only training a model because it checks whether the engineer can build a usable AI workflow end to end.

## Dataset

The candidate can use any public or synthetic tabular dataset.

Recommended options:

- Customer churn prediction
- Fraud transaction detection
- Loan default prediction
- Support ticket priority prediction
- Sales demand prediction

The dataset should include:

- at least 500 rows
- numeric columns
- categorical columns
- one clear target column
- some missing or noisy values

Production data should not be used.

## Required User Flow

The demo should support this flow:

1. User uploads or selects a CSV file.
2. User enters an experiment goal in natural language.
3. The copilot inspects the dataset.
4. The copilot proposes the ML task:
   - classification or regression
   - target column
   - feature columns
   - ignored columns
5. The copilot validates the plan.
6. The copilot trains at least two models.
7. The copilot compares model results.
8. The copilot explains why one model is better.
9. The UI shows metrics, charts, risky records or predictions, and explanation.

## Example Questions The Copilot Should Handle

After training, the user should be able to ask:

- Which model performed better and why?
- Which features are most important?
- Show the confusion matrix.
- Show risky fraud transactions.
- Explain this prediction.
- What data quality issues did you find?
- What should we improve before production use?
- Summarize the experiment result for a manager.

## Required Deliverables

### 1. Research And Design Note

The candidate should write a short technical note explaining:

- the selected dataset
- the prediction problem
- why the problem matters
- selected target column
- selected feature columns
- preprocessing approach
- models compared
- evaluation metrics
- explainability method
- limitations
- future improvements

The note should be clear enough for both an ML engineer and a product engineer.

### 2. Agentic Planner

The candidate should implement a simple planner.

The planner can be rule-based, LLM-based, or hybrid. It should produce a structured plan.

Example:

```json
{
  "task_type": "classification",
  "target_column": "is_churned",
  "feature_columns": ["monthly_spend", "support_tickets", "usage_days", "plan_type"],
  "ignored_columns": ["customer_id"],
  "models_to_train": ["LogisticRegression", "RandomForestClassifier"],
  "metrics": ["accuracy", "precision", "recall", "f1"],
  "explainability": "feature_importance"
}
```

The planner should also explain why it selected the target and features.

### 3. Plan Validator

The candidate should validate the plan before training.

Validation should check:

- target column exists
- feature columns exist
- target column is not used as a feature
- ID columns are ignored
- classification target has valid classes
- numeric and categorical columns are handled correctly
- missing values are handled
- selected models are supported

If the plan is invalid, the system should show a clear error and suggest a fix.

### 4. Training Pipeline

The candidate should train at least two models:

- one baseline model
- one stronger model

Recommended minimum:

- Logistic Regression
- Random Forest

The pipeline should include:

- train/test split
- preprocessing
- model training
- metric calculation
- model comparison
- best model selection

For classification, show:

- accuracy
- precision
- recall
- F1 score
- confusion matrix

For regression, show:

- MAE
- RMSE
- R2 score

### 5. Explainability

The candidate should explain model behavior.

Minimum:

- feature importance for tree model
- coefficients for linear model
- explanation of one prediction

Optional:

- SHAP values
- permutation importance
- local explanation for a selected row

The explanation should be understandable. It should not only show raw numbers.

### 6. UI Demo

The UI should show:

- dataset preview
- detected columns and types
- generated experiment plan
- validation result
- model comparison table
- selected best model
- metrics
- charts
- feature importance
- prediction explanations
- final summary

The UI can be built using:

- React
- Next.js
- Streamlit
- Gradio

The UI does not need to match production ANDLAKE design exactly, but it should be clean and demoable.

## Suggested UI Layout

```text
Agentic ML Experiment Copilot

1. Dataset Upload
2. Experiment Goal
3. Generated Plan
4. Validation Result
5. Model Comparison
6. Feature Importance
7. Prediction Explorer
8. Final Summary
```

## Suggested Architecture

```text
Frontend UI
        |
        v
Backend API / Script
        |
        v
Planner
        |
        v
Validator
        |
        v
Training Pipeline
        |
        v
Evaluation + Explainability
        |
        v
Structured Result
        |
        v
UI Report
```

## Expected Structured Response

The backend or script should return structured output like:

```json
{
  "experiment_goal": "Predict customer churn",
  "plan": {
    "task_type": "classification",
    "target_column": "is_churned",
    "feature_columns": ["monthly_spend", "support_tickets", "usage_days", "plan_type"]
  },
  "validation": {
    "status": "passed",
    "warnings": ["Dataset is small, results may not generalize."]
  },
  "models": [
    {
      "name": "LogisticRegression",
      "accuracy": 0.82,
      "precision": 0.78,
      "recall": 0.74,
      "f1": 0.76
    },
    {
      "name": "RandomForestClassifier",
      "accuracy": 0.88,
      "precision": 0.84,
      "recall": 0.81,
      "f1": 0.82
    }
  ],
  "selected_model": {
    "name": "RandomForestClassifier",
    "reason": "It has the best F1 score and recall on the test set."
  },
  "feature_importance": [
    { "feature": "support_tickets", "importance": 0.34 },
    { "feature": "usage_days", "importance": 0.27 }
  ],
  "summary": "The Random Forest model performed best. Support tickets and usage days were the strongest churn signals."
}
```

## Evaluation Criteria

| Area | What To Check |
|---|---|
| Research thinking | Does the candidate explain the ML problem and tradeoffs clearly? |
| Agentic design | Is there a planner, validator, tool execution, and final response? |
| Data handling | Are missing values, categorical columns, IDs, and target columns handled properly? |
| Model training | Are at least two models trained correctly? |
| Model comparison | Are metrics compared and is the selected model justified? |
| Explainability | Can the system explain feature importance and one prediction? |
| UI quality | Can the reviewer run and understand the demo easily? |
| Code quality | Is the code readable, structured, and reproducible? |
| Honesty | Are limitations clearly explained? |

## Good Signs

A strong submission should show:

- clear experiment goal
- structured agent plan
- validation before training
- at least two models compared
- correct metrics
- feature importance
- one prediction explanation
- clean UI
- clear README
- honest limitations

## Red Flags

These are warning signs:

- hardcoded model results
- no real train/test split
- only one model
- no validation
- no explanation
- no UI
- no run instructions
- production secrets required
- unclear or inflated claims

## Optional Stretch Work

If the candidate finishes early, they can add:

- SHAP explanation
- model artifact save/load
- experiment history
- follow-up chat questions
- anomaly detection mode
- natural language summary for non-technical users
- notebook report
- chart recommendation for each output

## What Not To Do

The candidate should not:

- modify production ANDLAKE code
- use production credentials
- depend on private services
- train a very large model
- focus only on UI
- skip validation
- hide model limitations

## Expected Time

Recommended time: 3 to 5 days.

Minimum acceptable version:

- one dataset
- planner
- validator
- two trained models
- metrics
- best model selection
- feature importance
- simple UI
- README

Strong version:

- prediction explanation
- clean agent trace
- multiple charts
- good research note
- polished demo

## Final Submission Format

```text
project/
  README.md
  data/
  backend/ or app/
  frontend/ or ui/
  research-note.md
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

This assignment is strong because it checks both research and product engineering.

It shows whether the candidate can:

- design an ML experiment
- compare models honestly
- validate AI-generated plans
- explain model behavior
- build an agentic workflow
- create a working UI
- communicate results clearly

This matches the kind of AI engineering work needed in ANDLAKE: not only training models, but turning model workflows into useful assistant-driven product experiences.
