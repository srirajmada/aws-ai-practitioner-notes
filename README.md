# AWS Certified AI Practitioner (AIF-C01) — Study Notes

Structured study notes for the **AWS Certified AI Practitioner (AIF-C01)** exam, written against the five official exam domains.

These aren't a transcript of a course. They're built around the things that are genuinely hard to keep straight — the distinctions that look similar until you have a way to separate them — with everyday analogies for the concepts that resist memorisation.

---

## What's here

```
exam-guide/   Objectives transcribed from the official AWS exam guide
domains/      One folder per exam domain, one file per task statement
services/     Service-level reference notes
```

### The five domains

| Domain | Weight | Notes |
|---|---|---|
| 1. Fundamentals of AI and ML | 20% | [`domains/01-ai-ml-fundamentals/`](domains/01-ai-ml-fundamentals/) |
| 2. Fundamentals of Generative AI | 24% | [`domains/02-genai-fundamentals/`](domains/02-genai-fundamentals/) |
| 3. Applications of Foundation Models | 28% | [`domains/03-foundation-model-applications/`](domains/03-foundation-model-applications/) |
| 4. Guidelines for Responsible AI | 14% | [`domains/04-responsible-ai/`](domains/04-responsible-ai/) |
| 5. Security, Compliance, and Governance | 14% | [`domains/05-security-compliance-governance/`](domains/05-security-compliance-governance/) |

Each domain folder contains a `README.md` (what matters most, and why), one file per official task statement, and a `flashcards.md` for rapid recall.

---

## How to use it

1. **Start with the domain README** — it lists the highest-value items and a suggested order.
2. **Read the task files** for the concepts. Each ends with an *exam traps* section covering the distinctions that are easy to get backwards.
3. **Drill the flashcards** once the concepts are solid.
4. **Domains 2 and 3 are 52% of the exam** between them, and Domain 3 assumes Domain 2's vocabulary — do them in order.

### Analogies

Several concepts are explained through everyday analogies, because the abstract version rarely sticks:

| Concept | Analogy |
|---|---|
| Supervised / unsupervised / reinforcement / self-supervised learning | 📦 Training a new delivery driver |
| Overfitting vs. underfitting | 🚗 Learning to drive vs. memorising the test route |
| MAE, MSE, RMSE, R² | 🍕 The food-delivery ETA |
| Embeddings and vector search | 🛒 The supermarket that shelves by meaning |
| Model Context Protocol (MCP) | 🔌 The standard wall socket |
| Bedrock pricing modes | 🚚 Renting a van vs. leasing one |
| Prompt engineering vs. RAG vs. fine-tuning | 🍟 Getting a new hire up to speed |
| Temperature, Top K, Top P | 🎬 Picking what to watch tonight |
| Top-p vs. top-k, in depth | 🍽️ Party catering |
| Interpretability vs. black-box models | 🔧 Two mechanics |
| Model weights | 🎛️ A sound engineer's mixing desk |
| The four Bedrock API endpoints | 🍽️ The restaurant |

---

## Scope and accuracy

- Written against the **AIF-C01** exam guide and public AWS documentation.
- AWS services change often. Where a specific figure or limit is quoted, **verify it against current AWS documentation** before relying on it — several claims in this repo needed correcting during writing as AWS changed behaviour.
- The published in-scope services list is explicitly *"non-exhaustive."* Several services absent from it are still worth knowing.

## Contributing

Corrections are welcome — particularly where AWS has changed something. Open an issue or a pull request.

---

## Disclaimer

This project is **not affiliated with, endorsed by, or sponsored by Amazon Web Services, Inc.**
"AWS", "Amazon Web Services", "AWS Certified AI Practitioner", and related marks are trademarks of Amazon.com, Inc. or its affiliates.

**No exam content is reproduced here.** These notes are written from the publicly available
[AWS Certified AI Practitioner exam guide](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01.html)
and public AWS documentation. They contain no questions, answer options, or other material from any AWS certification exam or practice test.

Provided as-is, with no warranty of accuracy or fitness for exam preparation.

## Licence

Content licensed under [CC BY 4.0](LICENSE) — use it, adapt it, share it, with attribution.
