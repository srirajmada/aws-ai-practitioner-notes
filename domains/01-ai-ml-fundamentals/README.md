# Domain 1 — Fundamentals of AI and ML (20%)

**~10 of the 50 scored questions.** The foundation domain: terminology, use-case matching, and the ML lifecycle. Lower conceptual difficulty than Domains 2–3, but it carries a lot of definitional questions that are free marks if you've drilled the vocabulary — and silly losses if you haven't.

## Contents

| File | Task statement | Focus |
|---|---|---|
| [01-ai-concepts-terminology.md](01-ai-concepts-terminology.md) | 1.1 | Vocabulary, AI/ML/DL/GenAI/agentic relationships, learning types, data types, inference types |
| [02-practical-use-cases.md](02-practical-use-cases.md) | 1.2 | When AI fits (and when it doesn't), technique selection, AWS AI service capabilities, traditional ML vs. FMs |
| [03-ai-ml-development-lifecycle.md](03-ai-ml-development-lifecycle.md) | 1.3 | ML pipeline stages, FM sources, deployment methods, MLOps, model + business metrics |
| [flashcards.md](flashcards.md) | all | Rapid recall drilling |

Official objectives: [exam-guide/01-domain-ai-ml-fundamentals.md](../../exam-guide/01-domain-ai-ml-fundamentals.md)

## The ten things most likely to be tested

1. **AI ⊃ ML ⊃ Deep Learning ⊃ GenAI** — the nesting, in that order. Agentic AI is a pattern layered on top, not another ring.
2. **Supervised (labeled) vs. unsupervised (unlabeled) vs. reinforcement (reward)** — pick from the data description.
3. **Classification (discrete label) vs. regression (continuous number) vs. clustering (discover groups).**
4. **Overfitting shows as a train/test gap** — great on training data, poor on new data.
5. **Inference types:** real-time (low latency, always on) / serverless (spiky, scales to zero) / asynchronous (large payloads, long runs, queued) / batch (offline, whole dataset).
6. **When NOT to use ML** — when a specific deterministic outcome is required rather than a prediction.
7. **Precision vs. recall** — precision when false positives cost; recall when false negatives cost. Accuracy is a trap on imbalanced data.
8. **The AI service pipeline:** Transcribe → Comprehend → Translate → Polly, plus Rekognition (images), Textract (documents), Lex (bots).
9. **Traditional ML vs. FMs** — explainability and regulatory need push you toward traditional ML.
10. **Model metrics vs. business metrics** — "is the model good?" vs. "was the project worth it?" (ROI, cost per user, CSAT).

## Newer content to watch

The current version of the exam guide added material that older study resources won't cover:

- **Agentic AI** appears in Tasks 1.1 and 1.2 — definitions, and agentic applications as a real-world use case.
- **Amazon Quick** — the October 2025 evolution of Amazon QuickSight into a GenAI-powered BI and automation suite. It is *not* a model-training service.
- **Kiro** — AWS's spec-driven agentic IDE, named in the pipeline-services objective. A developer tool, not a data or hosting service.
- **Amazon Nova** — AWS's own foundation model family, now on the in-scope services list.
- **Amazon Q is absent from the published in-scope services list — but IS tested.** ⚠️ Practice material treats **Amazon Q Business as a correct answer** (summarize PDFs in S3 and answer questions, no coding experience). The published list says it is "non-exhaustive," and Q is clearly under-represented in it. **Study Amazon Q Business.**

Treat this as the general warning for the whole repo: AIF-C01's guide has been refreshed, and third-party material lags it.

## Analogies in this domain

- 📦 **Training a new Amazon delivery driver** — supervised, unsupervised, reinforcement, semi-supervised, and self-supervised learning ([01](01-ai-concepts-terminology.md))
- 🍕 **The food delivery ETA** — MAE, MSE, RMSE, and R² ([03](03-ai-ml-development-lifecycle.md))

## Study approach

1. Read 1.1 first and drill the vocabulary until definitions are automatic — everything downstream depends on it.
2. For 1.2, practice reading a scenario and naming the technique *and* the service before looking at the options.
3. For 1.3, focus hardest on the metrics section; it produces more questions than the pipeline diagram does.
4. Finish with [flashcards.md](flashcards.md), then move to Domain 2.
