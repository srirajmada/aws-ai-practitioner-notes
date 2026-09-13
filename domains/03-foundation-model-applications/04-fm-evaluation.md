# Task 3.4 — Methods to evaluate FM performance

> Domain 3 (28%). The acronyms (**ROUGE, BLEU, BERTScore**) are near-guaranteed marks if you memorize which is which. The rest is judging whether the *application* — not just the model — actually works.

---

## 1. Approaches to evaluation

| Approach | How it works | Strengths | Weaknesses |
|---|---|---|---|
| **Automatic / programmatic** | Compare outputs to reference answers using algorithmic metrics (ROUGE, BLEU, BERTScore, F1, exact match) | Fast, cheap, repeatable, scales | Misses nuance, tone, and helpfulness; needs reference answers |
| **Human evaluation** | People rate outputs against criteria | The gold standard for quality, tone, safety, and subjective judgement | Slow, expensive, harder to reproduce |
| **LLM-as-a-judge** | A strong model scores another model's output against a rubric | Much cheaper and faster than humans; correlates well; scales | Inherits the judge's own biases; needs a well-written rubric |
| **Benchmark datasets** | Standard public test sets (MMLU, HELM, BIG-bench, GLUE, TruthfulQA) | Comparable across models; no dataset to build | Generic — may not reflect *your* task; risk of benchmark contamination |

> **Human-in-the-loop (HITL)** appears in the objective by name: humans review or approve model output as part of the workflow. Essential for high-stakes decisions, and it doubles as a source of feedback data.

### Amazon Bedrock Evaluations
The managed service for this. It supports:

- **Automatic evaluation** — programmatic metrics including **BERTScore, F1, and exact-match**, over built-in or custom datasets.
- **Human evaluation** — bring your own work team or use an AWS-managed team.
- **LLM-as-a-judge** — for both **model evaluation** and **RAG evaluation**.
- **Built-in metrics**: correctness, completeness, **faithfulness (hallucination detection)**, plus responsible-AI metrics such as **answer refusal, harmfulness, and stereotyping**.
- **Custom metrics** — write your own judge prompt and rating scale.
- **RAG evaluation** — **retrieval** metrics (context relevance, coverage) and **generation** metrics (correctness, faithfulness, completeness), evaluated separately.

Task types include general text generation, summarization, question answering, and classification.

### 📐 The FMEval evaluation dimensions

**These are LLM-specific metrics with no traditional-ML equivalent.** Learn the names — the exam asks for them by name, with RMSE/F1/AUC sitting alongside as distractors.

| Dimension | What it measures | Question cue |
|---|---|---|
| **Semantic robustness** | How much the output **changes when the input is perturbed in ways that don't change its meaning** — keyboard typos, random uppercase, added/deleted whitespace. A robust model gives the same answer despite cosmetic input changes | "**how quality changes with small adjustments in the input**", "typos", "input variations" |
| **Factual knowledge** | How well the model encodes **factual** information | "does the model know facts correctly" |
| **Prompt stereotyping** | Probability the model **encodes bias** — race, gender, sexual orientation, religion, age, nationality, disability, appearance, socioeconomic status | "measure stereotyping/bias in responses" |
| **Toxicity** | Harmful content in output, scored by toxicity detection models | "quantify toxicity", "ensure responses are unbiased and friendly" |
| **Accuracy** (task-specific) | Correctness for the given task — includes classification accuracy and summarization accuracy | "is the output correct" |

> **Semantic robustness maps directly onto AWS's responsible-AI dimension "veracity and robustness"** — *"achieving correct system outputs, even with unexpected or adversarial inputs."* Robustness is the **principle**; semantic robustness is how you **measure** it.

> 🚨 **A classic trap:** *"evaluate how the quality of the LLM's responses changes with **small adjustments in the input**"* → **semantic robustness**. The options offered **RMSE, AUC, and F1** — a regression metric and two classification metrics. **All three are traditional-ML metrics and none can evaluate an LLM at all.**

### SageMaker Clarify + FMEval — the other FM evaluation path

**Clarify is not only for traditional ML.** Alongside bias detection and SHAP explainability, it includes the **FMEval (Foundation Model Evaluation) library** for evaluating FMs:

- Compares **FM quality metrics** *and* **responsibility metrics** — including **bias and toxicity scores**.
- Works with **built-in benchmark datasets**, or **your own test dataset specific to your use case** (preferred — see the AI Service Cards caveat below).
- Runs **at scale**, automatically — no humans in the loop.

> **Exam cue:** *"identify which FM meets requirements for tone/toxicity/bias, at scale"* → **SageMaker Clarify (FMEval) quantifying toxicity on a test dataset.**

### ⚠️ Model selection vs. production review — the distinction that decides it

| | **Evaluating to SELECT a model** | **Reviewing PRODUCTION responses** |
|---|---|---|
| **When** | *Before* you choose — comparing candidate FMs | *After* deployment — checking live output |
| **How** | Automated metrics over a **test dataset** | **Humans** reviewing individual responses |
| **Scales?** | ✅ Yes | ❌ No — human-bound |
| **Tools** | **SageMaker Clarify / FMEval**, Bedrock Evaluations | **Amazon A2I**, human review workflows |

**Why A2I is wrong for model selection:** A2I provides a **human audit workflow for selected responses in production**. Human feedback on tone and toxicity is genuinely useful — but it reviews *responses already being produced*, so it can't tell you **which FM to pick in the first place**, and it doesn't work "**at scale**." The words **"at scale"** and **"identify foundation models"** both point away from human review.

### ⚠️ The limits of AWS AI Service Cards

A tempting option for "compare model performance," but it has two hard limitations:

1. **They only describe FMs developed by AWS AI services** — **not open-source or third-party models**. So they can't help you compare across a mixed candidate list.
2. Each provider decides what to disclose, and the figures are **sample benchmark data**. **You should test with a dataset specific to your use case**, not rely on published benchmarks.

> This is the same lesson as "benchmarks are generic" above — **your own golden dataset beats a published benchmark** for deciding whether a model fits *your* task.

---

## 2. The metrics — know these acronyms

| Metric | Full name | Measures | Used for |
|---|---|---|---|
| **ROUGE** | **R**ecall-**O**riented **U**nderstudy for **G**isting **E**valuation | **Recall**-oriented overlap: how much of the *reference* appears in the output | **Summarization** |
| **BLEU** | **B**i**l**ingual **E**valuation **U**nderstudy | **Precision**-oriented overlap: how much of the *output* appears in the reference | **Translation** |
| **BERTScore** | — | **Semantic** similarity using embeddings — not word overlap | Any generation where paraphrasing is valid. **The answer for "how similar is our output to expert/reference responses?"** |
| **Perplexity** | — | How "surprised" the model is by text; lower is better | Language modelling quality |
| **F1 / exact match** | — | Overlap or exact correctness against a reference | Question answering, classification |

**The memory hooks:**
- **ROUGE → R for Recall → summaRization.** Did the summary *capture* the source's key content?
- **BLEU → translation.** ("BLEU" ≈ *bilingual*.) Did the translation use the *right words*?
- **BERTScore → BERT is an embedding model → semantic meaning.** Catches correct paraphrases that ROUGE and BLEU would wrongly penalize.

### ROUGE variants

The exam uses the variant names, not just "ROUGE":

| Variant | What it compares |
|---|---|
| **ROUGE-N** | **N-gram overlap** between the generated summary and the reference summary. **ROUGE-1** = single words, **ROUGE-2** = word pairs |
| **ROUGE-L** | **Longest Common Subsequence** — rewards matching word order without requiring contiguity |

> ROUGE-N evaluates summary quality by comparing **n-gram overlaps** between the generated summary and reference summaries.

### ⚠️ "Recall" vs. "ROUGE" — the trap that catches people who half-know it

ROUGE stands for **Recall**-Oriented Understudy. So when a summarization question offers plain **"Recall"** as an option, it looks right. **It isn't.**

| | **Recall** (the classification metric) | **ROUGE** |
|---|---|---|
| Operates on | Class labels — TP / (TP + FN) | **Text** — n-gram overlap with a reference summary |
| Problem type | Classification | **Text generation / summarization** |
| Needs | Predicted vs. actual **labels** | Generated vs. **reference text** |

**Recall being "in the name" of ROUGE does not make bare recall a text-generation metric.** They measure completely different things on completely different data.

> **The limitation of ROUGE and BLEU:** they compare *words*, not *meaning*. "The film was excellent" vs. "The movie was superb" scores near zero on both, despite being the same statement. **BERTScore** and **LLM-as-a-judge** exist to fix exactly that.

---

## 3. Evaluating applications, not just models

A model can score well while the application still fails. The objective calls this out for RAG, agents, and workflows.

### Evaluating RAG
Evaluate the **two stages separately** — this is the key insight.

| Stage | Metrics | The question |
|---|---|---|
| **Retrieval** | Context relevance, context coverage/recall, precision@k | Did we fetch the right passages? |
| **Generation** | **Faithfulness/groundedness**, answer correctness, completeness, relevance | Did the answer actually follow from those passages? |

> **Diagnostic logic:** if the answer is wrong, find out *which stage* failed. Bad retrieval → fix chunking, embeddings, reranking, or the index. Good retrieval but a wrong answer → fix the prompt, the model, or grounding constraints.

**Faithfulness / groundedness** = is every claim supported by the retrieved context? This is the direct measure of **hallucination** in a RAG system.

### Evaluating agents

| Metric | Meaning |
|---|---|
| **Task completion rate** | Did the agent actually achieve the goal? **The headline agent metric** |
| **Tool selection accuracy** | Did it choose the right tool? |
| **Tool call correctness** | Were the parameters right? |
| **Number of steps / efficiency** | Did it get there without wandering? |
| **Cost and latency per task** | Agents make many model calls — this adds up fast |
| **Error recovery** | Does it handle a failed tool call gracefully? |

**AgentCore Evaluations** provides managed assessment of agent sessions, traces, and spans.

### Evaluating workflows
End-to-end success rate, per-stage failure attribution, latency budget across stages, and cost per completed workflow.

---

## 4. Does it meet business objectives?

Technical scores don't authorize a launch. The objective names these directly:

| Category | Metrics |
|---|---|
| **Productivity** | Time saved per task, throughput, manual steps eliminated |
| **User engagement** | Adoption, active users, sessions, retention |
| **Task engineering** | How well the work has been **decomposed into tasks the model actually performs well** — see below |
| **Business objective alignment** | **Task completion rate, user satisfaction (CSAT/NPS), cost per interaction** |
| **Financial** | ROI, conversion rate, ARPU, CLV, deflection rate |
| **Quality in production** | Accuracy on live traffic, escalation rate, complaint rate |

**On "task engineering":** AWS names it in the objective without defining it. The reasonable reading is **how well the business problem has been broken into tasks the FM can reliably do**. A model that scores poorly is often not the problem — the *task framing* is. "Handle this support ticket" is a badly engineered task; "extract the order number, classify the issue type, then draft a reply using the retrieved policy" is a well-engineered one. Measure it by per-step success rates and by whether decomposing a failing task fixes it.

> **Exam framing:** *"how do you know the model is good?"* → ROUGE/BLEU/BERTScore/human eval. *"how do you know the project was worth it?"* → ROI, CSAT, cost per interaction, task completion rate. The objective's phrase **"business objective alignment metrics"** signals the second kind.

**A sound evaluation practice:** build a **golden dataset** of representative inputs with known-good outputs, evaluate every model/prompt change against it, monitor live quality after release, and route human feedback back into improvement.

---

## Exam traps for this task

1. **ROUGE = summarization (recall). BLEU = translation (precision).** The single most-tested pair in this task.
2. **BERTScore measures semantic similarity**, so it credits valid paraphrases that ROUGE/BLEU penalize.
3. **Evaluate RAG retrieval and generation separately** — otherwise you can't tell which stage failed.
4. **Faithfulness/groundedness is the hallucination metric** for RAG.
5. **Task completion rate is the primary agent metric.**
6. **LLM-as-a-judge is the cost-effective middle ground** between programmatic metrics and human review — and Bedrock supports it for both model and RAG evaluation.
7. **Benchmarks are generic**; your own golden dataset is a better signal for your task.
8. **Human evaluation remains the gold standard** for tone, safety, and subjective quality.
9. **Model metrics ≠ business metrics.** Read which one the question wants.
10. **Bedrock Evaluations covers automatic, human, and LLM-as-a-judge**, including responsible-AI metrics (harmfulness, stereotyping, answer refusal).
