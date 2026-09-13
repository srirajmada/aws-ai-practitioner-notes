# Domain 4 — Guidelines for Responsible AI (14%)

**~7 of the 50 scored questions.** The most reasoning-friendly domain on the exam — much of it yields to careful common sense. But three things must be memorized outright: **AWS's eight dimensions**, **the six Guardrails policy types**, and **transparency vs. explainability**.

## Contents

| File | Task statement | Focus |
|---|---|---|
| [01-responsible-ai-development.md](01-responsible-ai-development.md) | 4.1 | The eight dimensions, bias sources and effects, dataset characteristics, Guardrails, legal risks, sustainable model choice |
| [02-transparency-explainability.md](02-transparency-explainability.md) | 4.2 | Transparency vs. explainability, Model Cards and AI Service Cards, the accuracy/interpretability tradeoff, human-centred design |
| [flashcards.md](flashcards.md) | both | Rapid recall drilling |

Official objectives: [exam-guide/04-domain-responsible-ai.md](../../exam-guide/04-domain-responsible-ai.md)

## The nine things most likely to be tested

1. **AWS's eight dimensions:** fairness, explainability, privacy and security, safety, controllability, veracity and robustness, governance, transparency.
2. **Fairness is the goal; bias is the defect.**
3. **Bias enters through data most often** — sampling, historical, and labeling bias.
4. **Aggregate accuracy hides subgroup harm** → the fix is **subgroup analysis**.
5. **Guardrails' six policy types:** content filters, denied topics, word filters, sensitive information filters, contextual grounding checks, Automated Reasoning checks.
6. **Contextual grounding checks = the hallucination control.**
7. **SageMaker Clarify does both bias detection and explainability (SHAP).**
8. **Transparency (what the system is) ≠ explainability (why this output).**
9. **The most accurate model is usually the least interpretable** — regulated decisions favour interpretability.

## Analogy in this domain

- 🔧 **Two mechanics** — interpretable vs. black box, the accuracy tradeoff, and the transparency/explainability distinction ([02](02-transparency-explainability.md))

## Cross-domain connections

Responsible AI is examined here but appears throughout:

- **Guardrails** also serves Domain 5 (securing AI systems) and Domain 3 (prompt-injection defence).
- **Hallucination** is defined in Domain 2, mitigated in Domain 3 (RAG), controlled here, and detected in Domain 5 (grounding techniques).
- **Bias and variance** were introduced in Domain 1 — this domain connects the statistics to demographic harm.
- **Model Cards** reappear in Domain 5 for data lineage and documentation.

## Study approach

1. Memorize the eight dimensions and the six Guardrails policy types — those are the pure-recall items.
2. Everything else rewards reasoning: if an option would harm a group, hide a failure, or mislead a user, it's wrong.
3. Watch for the "95% overall accuracy but one group at 60%" pattern — it appears in several forms.
