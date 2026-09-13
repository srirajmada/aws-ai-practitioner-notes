# Domain 3 — Applications of Foundation Models (28%)

**~14 of the 50 scored questions — the largest domain on the exam.** It's also the most practical: given a scenario, pick the right approach, the right parameter, the right service, and the right metric. Combined with Domain 2, this is **52% of your score**.

## Contents

| File | Task statement | Focus |
|---|---|---|
| [01-design-considerations.md](01-design-considerations.md) | 3.1 | Model selection, inference parameters, **RAG**, vector stores, **the customization cost spectrum**, agents |
| [02-prompt-engineering.md](02-prompt-engineering.md) | 3.2 | Prompt constructs, zero/one/few-shot, chain-of-thought, injection and jailbreaking, Prompt Management |
| [03-training-fine-tuning.md](03-training-fine-tuning.md) | 3.3 | Pre-training, continued pre-training, fine-tuning, distillation, RLHF, training data quality |
| [04-fm-evaluation.md](04-fm-evaluation.md) | 3.4 | ROUGE/BLEU/BERTScore, LLM-as-a-judge, RAG and agent evaluation, business metrics |
| [flashcards.md](flashcards.md) | all | Rapid recall drilling |

Official objectives: [exam-guide/03-domain-foundation-model-applications.md](../../exam-guide/03-domain-foundation-model-applications.md)

## The twelve things most likely to be tested

1. **The customization cost ladder, in order:** prompt engineering → RAG → fine-tuning → continued pre-training → pre-training from scratch.
2. **RAG for facts, fine-tuning for behaviour.** The most-tested distinction in the whole exam.
3. **Fine-tuning does not add current knowledge** — changing data means RAG.
4. **RAG = retrieve at query time, no weight changes, citations included.**
5. **Top K is a fixed count; Top P is cumulative probability; temperature re-weights** the distribution.
6. **Low temperature does not prevent hallucination** — it makes the model wrong *consistently*.
7. **Fine-tuning uses labeled data; continued pre-training uses unlabeled domain text.**
8. **Distillation = teacher/student**, for cutting inference cost at volume.
9. **ROUGE = summarization (recall). BLEU = translation (precision). BERTScore = semantic.**
10. **Evaluate RAG's retrieval and generation stages separately.**
11. **Injection redirects, jailbreaking bypasses safety, leaking extracts, poisoning corrupts the data.**
12. **Agents act; RAG only retrieves.** Match to whether the scenario requires something *done*.

## Analogies in this domain

- 🍟 **Getting a new hire up to speed at a burger franchise** — prompt engineering vs. RAG vs. fine-tuning vs. continued pre-training vs. distillation ([01](01-design-considerations.md))
- 🎬 **Picking what to watch tonight** — temperature, Top K, and Top P ([01](01-design-considerations.md))
- 🍽️ **The party-catering analogy** — a deeper cut on *why* top-p adapts to the model's confidence and top-k can't ([01](01-design-considerations.md))

## Study approach

1. Start with **3.1** and don't leave it until the customization ladder and RAG are automatic. They generate questions throughout the exam, not just here.
2. **3.2** is mostly recognition — drill the technique names and the four attack types until you can tell them apart from a one-line description.
3. **3.3** hinges on one distinction: labeled (fine-tuning) vs. unlabeled (continued pre-training).
4. **3.4** — memorize the three acronyms first, then the RAG two-stage evaluation logic.
5. This domain assumes Domain 2's vocabulary (tokens, embeddings, chunking). Go back if any of it is shaky.
