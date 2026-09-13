# Task 2.2 — Capabilities and limitations of GenAI for business problems

> Domain 2 (24%). The judgement task: what GenAI is genuinely good at, where it fails, how to choose a model, and how the business measures whether it worked. Expect scenario questions where the *right* answer is recognizing a limitation.

---

## 1. Advantages of GenAI

| Advantage | What it means | Why it matters commercially |
|---|---|---|
| **Adaptability** | One model handles many tasks — summarize, classify, draft, translate — with no retraining | Avoids building and maintaining a separate model per task |
| **Responsiveness** | Answers immediately, at any hour, at any volume | Round-the-clock service without headcount |
| **Conversational capability** | Natural multi-turn dialogue, understands intent and context | Non-technical users can self-serve; no query language to learn |
| **Content generation** | Produces genuinely new text, images, code, video | Automates creative and drafting work outright |
| **Simplicity / speed to market** | No labeled dataset, no training run — call an API | Weeks to prototype instead of quarters |
| **Personalization at scale** | Tailors output per user | 1:1 experience without 1:1 cost |
| **Handles unstructured data** | Reads documents, transcripts, images natively | Unlocks the ~80–90% of enterprise data that's unstructured |
| **Broad/cross-domain knowledge** | Useful across many subject areas out of the box | One capability serves many departments |

---

## 2. Disadvantages and limitations

The four the objective names, first — these are the most-tested:

### Hallucination
The model produces **confident, fluent, plausible output that is factually wrong**. Not a bug to be patched — a consequence of how the model works: it predicts likely next tokens, and *likely* is not the same as *true*. It has no built-in notion of truth and no way to signal "I don't know."

**Mitigations:** RAG grounding, citations to source, lower temperature, output validation, confidence scoring, human review, and guardrails with contextual grounding checks.

### Interpretability / explainability
You cannot trace *why* the model produced a specific output. Billions of parameters, no auditable decision path. That's disqualifying in contexts requiring justification — credit decisions, medical diagnosis, legal rulings.

### Inaccuracy
Beyond outright hallucination: outdated knowledge (a **training cutoff** means no awareness of recent events), subtle factual drift, arithmetic errors, and weak spots in niche domains.

### Nondeterminism
The **same prompt can produce different outputs on different calls**. Great for creative variety, painful for testing, reproducibility, auditing, and regression suites. Lowering temperature reduces variability but doesn't eliminate it.

### The rest of the list

| Limitation | Detail |
|---|---|
| **Knowledge cutoff** | No knowledge of events after training. Fix: RAG or tool use |
| **Context window limit** | Finite input size; long documents must be chunked |
| **Cost at scale** | Per-token pricing grows linearly with usage; long contexts multiply it |
| **Latency** | Slower than traditional ML inference — often hundreds of ms to seconds |
| **Bias** | Reproduces and can amplify bias present in training data |
| **Security exposure** | Prompt injection, jailbreaking, data leakage through prompts |
| **IP and licensing risk** | Uncertainty over training data provenance and output ownership |
| **Privacy** | Sensitive data in prompts may be logged or retained |
| **No true reasoning or maths** | Pattern completion, not calculation — use a tool for arithmetic |

> **Highest-yield trap in this task:** if the scenario needs a **guaranteed correct, reproducible, auditable, explainable** answer, GenAI alone is the wrong choice. Ground it, constrain it, add human review — or use traditional ML/deterministic code.

---

## 3. Selecting a GenAI model

The objective lists the factors; here's what each actually decides.

| Factor | Questions to ask | Effect |
|---|---|---|
| **Model type / modality** | Text only, or image/video/audio in or out? | Rules models in or out immediately |
| **Capabilities** | Reasoning depth, coding, tool use, multi-lingual, instruction-following | Match to task difficulty |
| **Performance requirements** | What accuracy/quality bar must it clear? | Bigger models for harder tasks |
| **Latency** | Interactive chat or overnight batch? | Small models are dramatically faster |
| **Cost** | Token price × expected volume | Often the deciding factor at scale |
| **Model size / complexity** | Bigger = more capable, slower, pricier | The core tradeoff |
| **Context window** | How much input must fit at once? | Long-document work needs a big window |
| **Customization** | Does it support fine-tuning, distillation, RAG? | Constrains the adaptation path |
| **Compliance / licensing** | Data residency, regulated industry, model licence terms | Can veto an otherwise ideal model |
| **Constraints** | Region availability, deployment model, existing contracts | Practical blockers |

**The standard method:** define the quality bar → try the **smallest, cheapest model** first → measure against your evaluation set → escalate only if it fails. Don't start at the biggest model.

> **Multi-model routing** is normal and good practice: cheap fast model for the easy 80%, large model for the hard 20%. Bedrock Intelligent Prompt Routing does this automatically.

---

## 4. Business value and metrics

The objective names these explicitly. Split them into the two questions they answer.

### "Is the model any good?" — quality metrics

| Metric | Meaning |
|---|---|
| **Accuracy** | Correctness of outputs against ground truth |
| **Cross-domain performance** | How well it holds up across *different* subject areas — a key FM advantage over narrow models |
| **Relevance / groundedness** | Are answers supported by the provided sources? |
| **Task completion rate** | Share of user goals actually achieved (the key agent metric) |

### "Was it worth doing?" — business metrics

| Metric | Meaning |
|---|---|
| **ROI** | (Gain − cost) / cost — the executive summary number |
| **Efficiency / productivity** | Time saved, throughput gained, manual steps removed |
| **Conversion rate** | Share of users taking the desired action |
| **Average revenue per user (ARPU)** | Revenue effect per customer |
| **Customer lifetime value (CLV)** | Long-term value impact |
| **Cost per interaction / per user** | Unit economics — critical with token pricing |
| **Customer satisfaction (CSAT / NPS)** | Do users actually prefer it? |
| **Deflection rate** | Share of support contacts resolved without a human |
| **Adoption / engagement** | Is anyone using it? |

> **Exam framing:** a question asking how to evaluate whether a GenAI *initiative* succeeded wants **business** metrics (ROI, CSAT, conversion, cost per interaction) — not ROUGE or BLEU. A question about whether the *model* is performing wants quality metrics. Read which one is being asked.

> **Cross-domain performance** is worth memorizing as a phrase — it's specific AWS vocabulary for an FM's ability to perform across multiple domains, and it appears verbatim in the objective.

---

## Exam traps for this task

1. **Hallucination is inherent, not a defect to be fixed.** You mitigate it (RAG, grounding, citations, validation, human review) — you don't eliminate it.
2. **Nondeterminism** means the same prompt can give different answers. Temperature reduces but doesn't remove it.
3. **Knowledge cutoff → RAG or tool use**, not fine-tuning. Fine-tuning teaches style and format, not fresh facts.
4. **Explainability requirements point away from GenAI** toward traditional ML or deterministic logic.
5. **Start with the smallest model**; escalate only when it misses the quality bar.
6. **Bigger model ≠ better answer** on the exam. Cost, latency, and sufficiency matter more.
7. **Business metrics vs. model metrics** — know which the question is asking for.
8. **GenAI can't do reliable arithmetic.** Give it a calculator tool.
9. **Adaptability is GenAI's headline advantage** — one model, many tasks, no retraining.
