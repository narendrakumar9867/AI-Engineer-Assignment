# Internal Understanding Document
## AI Engineer Assignment: Agentic ML/DL Experiment Copilot (Research-Grade)

## Context

This document captures our internal intent behind the assignment and how reviewers should evaluate submissions consistently.

The assignment is meant to identify candidates who can operate as **AI Engineers in a product context**, not only as ML model trainers.

## What This Assignment Is Actually Testing

We are evaluating five core dimensions:

1. **Problem Framing Quality**
   - Can candidate translate natural language goals into valid ML problem definitions?
   - Are assumptions explicit and reasonable?

2. **Agentic System Thinking**
   - Is there a true planner/validator/executor/reflection loop?
   - Is behavior dynamic, evidence-driven, and self-correcting?

3. **ML + DL Research Depth**
   - Does candidate compare classical ML and DL appropriately?
   - Are choices aligned with data regime, constraints, and risk profile?

4. **Mathematical and Statistical Rigor**
   - Are metric/loss/model decisions justified mathematically?
   - Is statistical testing used for model comparison rather than raw metric claims?

5. **Engineering + Product Readiness**
   - Is the system reproducible, explainable, and reviewer-friendly?
   - Are limitations and residual risks communicated honestly?

## Why This Fits ANDLAKE

This task mirrors realistic ANDLAKE workflows:

- natural language objective to structured experimentation
- robust experimentation and explainability in one interface
- agentic decision behavior with transparent traces
- safe and reliable recommendations with escalation logic

## Reviewer Expectations

A strong candidate submission should include:

- non-linear agent behavior
- explicit validation checks before training
- at least 2 classical ML + 1 DL comparison
- mathematically defended metric/model selection
- statistical comparison (with method + interpretation)
- at least one self-correction event
- at least one meaningful RAG-influenced decision
- interpretable outputs for both technical and non-technical reviewers
- reproducible setup and run instructions

## Minimum Pass Requirements (Gate Checks)

Fail if any of these are missing:

- planner + validator
- ML vs DL comparison
- explainability output
- self-correction/re-plan evidence
- reproducible run instructions
- honest limitations section

## Scoring Rubric (100)

| Category | Weight | Reviewer Question |
|---|---:|---|
| Agentic autonomy and adaptability | 25 | Is the system truly adaptive and self-correcting? |
| ML/DL technical depth and correctness | 20 | Are models selected/trained/evaluated correctly? |
| Mathematical/statistical rigor | 20 | Are claims quantitatively justified? |
| RAG + infrastructure quality | 15 | Is retrieval useful and system reproducible? |
| Robustness, safety, failure handling | 10 | Are risks identified and guarded? |
| Communication quality | 10 | Is the output clear and decision-usable? |

## Good Signs

- Candidate states hypothesis before experiments
- Agent decisions are logged with rationale
- Metric choice matches task risk (e.g., imbalance-aware)
- Statistical comparison used before “best model” claim
- RAG retrieval influences at least one major choice
- Clear failure analysis with practical guardrails
- UI clearly communicates what happened and why

## Red Flags

- “Agentic” label but fixed hardcoded flow
- No re-plan/self-correction behavior
- No statistical method, only direct metric ranking
- DL added without rationale
- RAG included only for appearance
- Missing leakage checks
- Overconfident claims, weak evidence
- Non-reproducible code or unclear setup

## Interview Follow-up Questions (Suggested)

Use these prompts to probe depth:

1. “Show one point where the agent changed strategy. What triggered it?”
2. “Why is your primary metric the right one for this business risk?”
3. “When would you prefer classical ML over DL here?”
4. “What are the top failure modes still unresolved?”
5. “How would you productionize this safely in a real platform?”

## Decision Guidance

Select candidates who demonstrate:
- sound scientific reasoning
- robust system design
- honest risk communication
- practical engineering quality

Prioritize **quality of reasoning and reliability** over cosmetic UI polish or small metric gains.

## Notes for Consistent Review

- Reward transparent thinking and traceability.
- Reward strong validation and failure handling.
- Penalize unsupported claims.
- Penalize missing reproducibility.
- Penalize lack of explanation for model behavior.
