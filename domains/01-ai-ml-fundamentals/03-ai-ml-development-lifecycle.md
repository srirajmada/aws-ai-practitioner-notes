# Task 1.3 — Describe the AI/ML development lifecycle

> Domain 1 (20%). Covers the ML pipeline end to end, where FMs come from, how models reach production, MLOps, and — heavily tested — **model metrics vs. business metrics**.

---

## 1. Components of an AI/ML pipeline

```
 1. Business      2. Data         3. Data          4. Feature
    problem   →      collection →    preparation →    engineering
    framing                          (EDA, clean)     (transform, select)
                                                            ↓
 8. Monitoring  ←  7. Deployment ←  6. Evaluation  ←  5. Model
    & retraining                                        training
        │                                               & tuning
        └──────────── feedback loop ────────────────────────┘
```

| Stage | What happens | AWS services |
|---|---|---|
| **1. Business problem framing** | Define the goal, success criteria, and *whether ML is even appropriate*. Translate business goal → ML problem type. | — |
| **2. Data collection** | Gather and centralize raw data; establish a data lake. | S3, Glue, Kinesis, Data Exchange, Lake Formation |
| **3. Data preparation / preprocessing** | Clean, deduplicate, handle missing values, EDA, split into train/validation/test. Label if supervised. | SageMaker Data Wrangler, Glue DataBrew, SageMaker Ground Truth (labeling) |
| **4. Feature engineering** | Create, transform, select the input variables the model learns from. Store and reuse them. | SageMaker Feature Store, Data Wrangler |
| **5. Model training & tuning** | Choose an algorithm, train on the training set, tune hyperparameters. | SageMaker Training, Automatic Model Tuning, JumpStart |
| **6. Evaluation** | Measure on the held-out **test** set. Check accuracy *and* bias. Compare against business criteria. | SageMaker Experiments, Clarify, Bedrock Model Evaluation |
| **7. Deployment** | Ship the model to serve predictions. | SageMaker endpoints (real-time/serverless/async), Batch Transform, Bedrock |
| **8. Monitoring & retraining** | Watch live quality, detect drift, feed results back, retrain. | SageMaker Model Monitor, CloudWatch, Model Dashboard |

### The three data splits — know these

| Split | Purpose | Typical share |
|---|---|---|
| **Training set** | The model learns its parameters from this | ~70–80% |
| **Validation set** | Tune hyperparameters and select between models *during* development | ~10–15% |
| **Test set** | Final, unbiased estimate of real-world performance. Used **once**, at the end. | ~10–15% |

> **Trap:** evaluating on the training set. High training accuracy proves nothing — it's the test set that reveals overfitting.

> **Data quality note:** garbage in, garbage out. Most real ML effort is stages 2–4, not stage 5.

### Cross-validation and k-fold splitting

A single train/validation split has a weakness: **you might get an unlucky split.** If the validation slice happens to be unrepresentative, your measurement is misleading — and with a small dataset, that's likely.

**K-fold cross-validation** fixes it. Split the data into `k` equally-sized **folds**. Train on `k−1` of them, validate on the one left out. Repeat `k` times so **every fold takes a turn as the validation set**. Average the `k` scores into one estimate.

```
k = 4     ■ = train     □ = validate

Round 1:  □ ■ ■ ■
Round 2:  ■ □ ■ ■
Round 3:  ■ ■ □ ■
Round 4:  ■ ■ ■ □
                        → average the 4 validation scores
```

| Why use it | Detail |
|---|---|
| **Reduces overfitting and selection bias** | The model is judged on every part of the data, not one arbitrary slice |
| **Makes full use of limited data** | Critical when you have few training instances |
| **More reliable estimate** | Averaging k measurements beats trusting one |

**Cost:** it trains the model `k` times. AWS notes cross-validation increases training time by **~20% on average**.

**How SageMaker Autopilot uses it:** in HPO mode it applies k-fold automatically for small datasets (**50,000 or fewer training instances**) with **k = 5**. In ensembling mode it cross-validates regardless of dataset size, and defaults to an **80/20** train/validation split if you don't supply your own.

> **Exam cue:** "small dataset", "risk of an unrepresentative split", "make the most of limited data" → **k-fold cross-validation**.

---

## 2. Sources of foundation models

Three routes, in increasing order of cost and effort:

| Source | What it is | Effort / cost | When to choose |
|---|---|---|---|
| **Consume a pre-trained proprietary FM via API** | Use a provider's model as a managed service (Amazon Nova, Anthropic Claude, Meta Llama, Mistral, etc. through Amazon Bedrock) | Lowest — no training, pay per token | Almost always the default. Fast, no infrastructure. |
| **Open-source pre-trained model** | Download openly available weights (Llama, Mistral, Falcon), host or fine-tune yourself | Medium — you own hosting, ops, scaling | You need weight-level control, on-prem/VPC hosting, custom modification, or license flexibility |
| **Train a custom FM from scratch** | Pre-train your own model on your own corpus | **Extremely high** — millions of dollars, huge data, GPU clusters, specialist team | Almost never for the exam. Only for organizations with unique domain data and deep resources. |

**Customization spectrum** (cheapest → most expensive):
`prompt engineering → RAG → fine-tuning → continued pre-training → pre-training from scratch`

> **Exam heuristic:** if a question asks for the *most cost-effective* way to adapt an FM, the answer is almost never "train from scratch" and rarely "fine-tune." Start at prompt engineering and RAG. Detail lives in Domain 3.

---

## 3. Methods to use a model in production

| Method | Description | Pros | Cons |
|---|---|---|---|
| **Managed API service** | Provider hosts the model; you call an endpoint (Amazon Bedrock, Comprehend, Rekognition) | No infrastructure, instant scale, pay per use, always patched | Less control, per-token/per-call cost, model choice limited to catalog |
| **Self-hosted API** | You deploy the model on your own compute (SageMaker endpoint, EC2, ECS/EKS) | Full control over model, weights, data path, network isolation | You own scaling, patching, GPU capacity, and cost of idle |

> **Cue:** "minimal operational overhead" / "no infrastructure to manage" → managed API.
> "Full control of model weights" / "must run inside our VPC on our hardware" → self-hosted.

---

## 4. AWS services by pipeline stage

The objective explicitly names **Amazon Bedrock, Amazon Quick, Kiro, and SageMaker AI** — the last two are recent additions, so expect them.

| Service | What it is | Where it fits |
|---|---|---|
| **Amazon Bedrock** | Fully managed, serverless access to foundation models from multiple providers through one API. Adds Knowledge Bases, Guardrails, Agents, Model Evaluation, Prompt Management. | GenAI build + deploy — no infrastructure |
| **Amazon SageMaker AI** | End-to-end platform to build, train, tune, deploy, and monitor **custom** ML models. | Every stage of the traditional ML pipeline |
| **Amazon SageMaker JumpStart** | Hub of pre-trained models and pre-built solution templates, deployable in a few clicks. | Fast start on training/deployment |
| **Amazon Quick** (Quick Suite) | The evolution of Amazon QuickSight into a generative-AI-powered BI and productivity suite — **Quick Sight** (dashboards/BI), **Quick Research**, **Quick Flows**, **Quick Automate**, **Quick Index**, accessed through a chat interface. Rebranded October 2025. | Data exploration, visualization, natural-language insight, workflow automation |
| **Kiro** | An **agentic IDE** built on Code OSS that uses **spec-driven development**: you write a spec, and the agent generates and maintains code against it. Includes hooks that fire on repo events. Works across stacks and clouds. | Development tooling — building AI applications |
| **Strands Agents** | Open-source SDK for building AI agents. | Agent development |
| **Amazon Bedrock AgentCore** | Managed runtime and services for deploying agents at production scale (identity, memory, tools, observability). | Agent deployment |

> **Careful with Amazon Quick.** It is *not* an ML training service. It's the BI/analytics/productivity layer — think "explore data and build dashboards, now with GenAI," plus workflow automation. If a question describes visualizing data or asking questions of a dataset in natural language, that's Quick.

> **Careful with Kiro.** It is a developer tool (an IDE), not a model-serving or data service. If the question is about *writing software with an AI agent using specs*, that's Kiro.

---

## 5. MLOps

**MLOps = DevOps principles applied to machine learning**, extended to handle data and models as first-class versioned artifacts.

### Why ML needs more than DevOps
Traditional software has code. ML has **code + data + model**, and all three change independently. A model can degrade without a single line of code changing, simply because the world moved.

### Core MLOps concepts (from the objective)

| Concept | Meaning |
|---|---|
| **Experimentation** | Systematically track runs, datasets, hyperparameters, and results so findings are comparable and reproducible |
| **Repeatable processes** | Automated, versioned pipelines — not hand-run notebooks. Same inputs → same outputs |
| **Scalable systems** | Infrastructure that handles growing data and request volume |
| **Managing technical debt** | ML debt accrues in data dependencies, dead features, glue code, and unmaintained pipelines |
| **Production readiness** | Testing, monitoring, rollback, security, and CI/CD before the model ever serves users |
| **Model monitoring** | Track live quality, latency, and data/model drift |
| **Model retraining** | Refresh the model on new data when performance decays — triggered by schedule or by drift alarm |

### Drift — the reason monitoring exists

| Type | Definition | Example |
|---|---|---|
| **Data drift** | The distribution of the *input* data changes | Customers skew younger than the training population |
| **Concept drift** | The *relationship* between input and target changes | What counts as "fraudulent behavior" evolves |
| **Model drift / decay** | The net effect: accuracy degrades over time in production | Model that was 94% accurate is now 78% |

> Response to drift: retrain on fresh data. **Model Monitor** detects it; a retraining pipeline fixes it.

### Key AWS MLOps services
SageMaker Pipelines (orchestration), Model Registry (versioning/approval), Experiments, Model Monitor (drift), Model Cards (documentation), Model Dashboard, plus CloudWatch and CloudTrail.

> 📍 **Full SageMaker component map with interaction flows** — every component by lifecycle stage, four end-to-end flows (no-code Canvas path, code-first Studio path, JumpStart, Autopilot), and the ten pairs the exam confuses: **[services/sagemaker-ai.md](../../services/sagemaker-ai.md)**
>
> **The hand-off to remember:** **Canvas pushes models → Model Registry → Studio users access the same registry** to review and approve. Least operational overhead. And **you cannot push your own models into JumpStart** — it's a consume-only hub.

---

## 6. Model performance metrics

### 🎯 Metric vs. ML problem type — the master table

**Match the metric to the problem type FIRST, before evaluating any individual metric.** Picking a classification metric for a regression problem (or vice versa) is an instant wrong answer — and it's a favourite exam trap.

| Problem type | Output | Valid metrics | **Never** use |
|---|---|---|---|
| **Regression** (continuous number) | A quantity: price, volume, temperature, demand | **MAE, MAPE, MSE, RMSE, R²** | Accuracy, precision, recall, F1, AUC |
| **Binary classification** | One of two classes | Accuracy, **precision, recall, F1**, AUC-ROC, balanced accuracy, log loss | MAE, RMSE, R² |
| **Multiclass classification** | One of 3+ classes | Accuracy, **F1macro, PrecisionMacro, RecallMacro**, log loss, confusion matrix | MAE, RMSE, R² |
| **Clustering** (unsupervised) | Discovered groups | Silhouette score, Davies-Bouldin index, inertia (WCSS) | Anything needing ground-truth labels |
| **Anomaly detection** | Outlier / normal | Precision, recall, F1 **on the rare class**, AUC | Plain accuracy (the class imbalance makes it meaningless) |
| **Ranking / recommendation** | Ordered list | Precision@k, Recall@k, NDCG, MRR, hit rate | Plain accuracy |
| **Text generation (LLM)** | Generated text | **ROUGE** (summarization), **BLEU** (translation), **BERTScore** (semantic), perplexity, LLM-as-a-judge | Accuracy, RMSE |
| **RAG applications** | Grounded answer | Context relevance & coverage (retrieval); **faithfulness**, correctness, completeness (generation) | Accuracy alone |
| **Agents** | Completed task | **Task completion rate**, tool selection accuracy, steps, cost per task | ROUGE/BLEU |

> **The decision rule:** ask *"what shape is the output?"*
> **A number** → regression metrics (MAE/MAPE/MSE/RMSE/R²).
> **A label** → classification metrics (accuracy/precision/recall/F1/AUC).
> **Generated text** → ROUGE/BLEU/BERTScore.

> 🚨🚨 **The single most common metric mistake: reaching for a traditional ML metric on a problem that isn't traditional ML.** It shows up in several disguises:
>
> | The task | Metric people reach for | Actually correct | Why the instinct fails |
> |---|---|---|---|
> | Predict **annual sales volume** → *regression* | Accuracy, Precision | **MAE, MAPE** | **Classification** metrics on a continuous prediction — no class labels exist |
> | **Text summarization** → *generation* | F1 score | **ROUGE-N** | **Classification** metric on generated text |
> | **LLM question answering** → *generation* | RMSE | **Semantic robustness** | **Regression** metric on generated text |
>
> **Note the third goes the OTHER direction** — a *regression* metric on text, not a classification one. So the rule isn't "stop picking classification metrics." It's bigger than that:
>
> ### 🔑 THE RULE THAT FIXES ALL THREE
>
> > **RMSE, MAE, MAPE, R², accuracy, precision, recall, F1, and AUC are ALL traditional-ML metrics.**
> > **NONE of them can evaluate an LLM or any generated text.** Not one.
>
> When the subject is an **LLM / FM / generated text**, the entire traditional metric family is a distractor block — the exam puts three or four of them in the options precisely because they look familiar. The real answer will come from a **completely different vocabulary**:
>
> **ROUGE · BLEU · BERTScore · perplexity · semantic robustness · toxicity · factual knowledge · prompt stereotyping · faithfulness · LLM-as-a-judge**
>
> **A 10-second check for any metric question:**
> 1. **Is this an LLM / generated text?** → the answer is from the GenAI vocabulary above. **Eliminate every traditional metric immediately.**
> 2. Otherwise, **is the output a number?** → MAE / MAPE / MSE / RMSE / R²
> 3. Otherwise, **is the output a label?** → accuracy / precision / recall / F1 / AUC

### Classification — start with the confusion matrix

| | **Predicted Positive** | **Predicted Negative** |
|---|---|---|
| **Actual Positive** | True Positive (TP) | False Negative (FN) — *missed it* |
| **Actual Negative** | False Positive (FP) — *false alarm* | True Negative (TN) |

| Metric | Formula | Plain meaning | Optimize when… |
|---|---|---|---|
| **Accuracy** | (TP+TN) / all | Share of all predictions that were right | Classes are balanced. **Classification only — never regression** |
| **Precision** | TP / (TP+FP) | Of the ones I flagged, how many were real? | **False positives are costly.** Classification only |
| **Recall** (sensitivity, TPR) | TP / (TP+FN) | Of the real ones, how many did I catch? | **False negatives are costly.** Classification only |
| **F1 score** | 2 × (P×R) / (P+R) | Harmonic mean of precision and recall | You need balance, or classes are imbalanced |
| **AUC-ROC** | Area under TPR-vs-FPR curve | Ability to discriminate across all thresholds. 1.0 perfect, 0.5 random | Comparing models overall |

**The precision/recall tradeoff — the most-tested idea in this section:**

- **Maximize recall** when *missing* a positive is the expensive error: cancer screening, fraud detection, security threat detection, content moderation of dangerous material. Better to over-flag than to miss one.
- **Maximize precision** when a *false alarm* is the expensive error: spam filtering (don't quarantine real mail), blocking legitimate customer transactions, expensive automated interventions.

> **The accuracy trap.** With 99.9% legitimate transactions, a model that predicts "legitimate" every single time is **99.9% accurate and completely useless** — it catches zero fraud. On imbalanced data, accuracy is misleading; use **precision, recall, F1, or AUC**. Expect this question.

#### Worked example — 1,000 transactions, 100 actually fraudulent

The model flags 80 transactions. 60 of those are genuinely fraudulent.

| | Predicted Fraud | Predicted Legitimate |
|---|---|---|
| **Actually Fraud** | TP = 60 | FN = 40 |
| **Actually Legitimate** | FP = 20 | TN = 880 |

- **Accuracy** = (60 + 880) / 1000 = **94%** ← looks great
- **Precision** = 60 / (60 + 20) = **75%** — three-quarters of our fraud alerts were real
- **Recall** = 60 / (60 + 40) = **60%** — but we **missed 40% of actual fraud**
- **F1** = 2 × (0.75 × 0.60) / (0.75 + 0.60) = **67%**

The 94% accuracy conceals the real problem. For fraud, the 60% recall is what matters — and pushing recall up (flagging more aggressively) will lower precision. That's the tradeoff, in numbers.

> **One more metric to recognize:specificity** (true negative rate) = TN / (TN + FP) = 880/900 = 97.8%. It's recall's mirror image, measured on the negative class.

### More classification metrics (as AWS defines them)

SageMaker Autopilot exposes these, and AWS's own definitions are the ones to learn:

| Metric | Definition | When it matters |
|---|---|---|
| **Balanced accuracy** | `0.5 × ((TP/P) + (TN/N))` — accuracy after normalizing for class sizes | The honest accuracy on **imbalanced** data (e.g. 1% spam) |
| **Log loss** (cross-entropy) | Judges the quality of the predicted **probabilities**, not just the final label. Range 0 → ∞, lower is better | Punishes being *confidently* wrong — a model that says 99% certain and is wrong is penalized hard |
| **F1macro / PrecisionMacro / RecallMacro** | Compute the metric per class, then average across classes | **Multiclass** problems |
| **Inference latency** | Time from request to prediction from a real-time endpoint | Production performance, not predictive quality |

> **Balanced accuracy is the direct fix for the accuracy trap.** On the 99.9% legitimate transactions example, always predicting "legitimate" gives 99.9% accuracy but only ~50% balanced accuracy — which correctly reads as "no better than a coin flip."

### Regression metrics

| Metric | Definition | Range | Note |
|---|---|---|---|
| **MAE** (Mean Absolute Error) | Average of the absolute errors | 0 → ∞, lower better | Treats every unit of error equally; robust to outliers |
| **MAPE** (Mean Absolute **Percentage** Error) | Mean of \|actual − predicted\| ÷ actual, as a **percentage** | 0% → ∞, lower better | **Scale-independent** — "we're off by 8% on average" is comparable across products, regions, and datasets in a way RMSE is not. The natural metric for **sales and demand forecasting**. Breaks down when actual values are zero or near zero |
| **MSE** (Mean Squared Error) | Average of the **squared** errors | 0 → ∞, lower better | Punishes large errors disproportionately. Units are squared, so not human-readable |
| **RMSE** (Root Mean Squared Error) | Square root of MSE | 0 → ∞, lower better | Same units as the target, so interpretable — while still punishing big misses. **Scale-dependent**: don't compare across different datasets |
| **R²** (coefficient of determination) | How much of the target's variance the model explains | **1 → −1**, higher better | 0 ≈ explains nothing. **Negative means worse than a constant horizontal line** — i.e. worse than always guessing the average |

> **MAE vs. MAPE — why both can be correct.** Both are absolute-error measures, so both are valid for regression. The difference is the **unit**: MAE reports the error in the target's own units ("off by 400 units"), MAPE reports it as a **percentage** ("off by 8%"). MAPE is preferred when you need to compare error across items of very different scale — forecasting a product selling 200 units a year against one selling 2,000,000.

### 🍕 Analogy — The food delivery ETA

You run a delivery service. Every order gets a promised arrival time, and every delivery has an **error**: promised 7:00, arrived 7:06 → you were off by 6 minutes. The metrics are just different ways of summarizing a month of those misses.

**MAE — "on average, how many minutes are we off?"**
Add up every miss, ignore whether early or late, divide by the number of deliveries. Six minutes. Honest, simple, in minutes a human understands. Every minute of lateness counts the same, whether it's the first or the sixtieth.

**MSE — "how badly does a disaster hurt?"**
Square each miss before averaging. A 3-minute slip contributes 9. A 30-minute catastrophe contributes 900 — **a hundred times worse**, not ten times. This matches how customers actually feel: ten deliveries six minutes late is a shrug; one delivery an hour late is a lost customer and a bad review. The problem is the unit — MSE comes out in *minutes squared*, which means nothing to anyone.

**RMSE — "put that back into minutes"**
Take the square root of MSE. You keep the heavy punishment for catastrophes but get a number back in real minutes that you can put on a slide.

**Here's the whole point, in one comparison. Ten deliveries, two different months:**

| | Month A: consistently sloppy | Month B: mostly perfect, one disaster |
|---|---|---|
| The misses | Ten deliveries, each **6 min** late | Nine **on time**, one **60 min** late |
| **MAE** | **6 minutes** | **6 minutes** ← identical! |
| **RMSE** | **6 minutes** | **19 minutes** ← three times worse |

MAE says the two months were equally bad. RMSE says Month B was far worse. **Neither is wrong — they answer different questions.** If your business dies from one catastrophic failure, use RMSE. If every late minute costs you the same regardless of how it's distributed, use MAE.

**R² — "are we even beating the dumbest possible guess?"**
The lazy baseline is telling every customer "about 30 minutes" — the average — and never thinking again. R² asks how much better than *that* your clever model is.

- **R² = 1** → perfect predictions.
- **R² = 0.7** → explains 70% of the variation in delivery times.
- **R² = 0** → no better than always saying "30 minutes."
- **R² negative** → **worse than the lazy baseline.** Your model would improve if you deleted it and posted the average on the wall.

| At the delivery company | In machine learning |
|---|---|
| Promised time vs. actual arrival | Predicted value vs. actual value |
| "On average we're 6 minutes out" | **MAE** |
| "One 60-minute disaster counts 100× a 6-minute slip" | **MSE** |
| That same harsh score, converted back into minutes | **RMSE** |
| "Are we beating just saying *30 minutes* every time?" | **R²** |

> **Exam cue:** *"large errors are especially costly"* or *"sensitive to outliers"* → **MSE/RMSE**. *"treat all errors equally"* or *"robust to outliers"* → **MAE**. *"how much variance does the model explain"* → **R²**.

### Generative AI metrics (preview — Domain 3 detail)
**ROUGE** (summarization, recall-oriented overlap), **BLEU** (translation, precision-oriented overlap), **BERTScore** (semantic similarity via embeddings), **LLM-as-a-judge**, and human evaluation.

---

## 7. Business metrics

The other half of the objective — and the half candidates neglect. **A technically excellent model that doesn't move a business number is a failed project.**

| Business metric | What it measures |
|---|---|
| **Cost per user / per interaction / per inference** | Unit economics of running the model |
| **Development cost** | Build investment: data, labeling, training, engineering time |
| **Return on investment (ROI)** | (Gain − cost) / cost — the executive question |
| **Customer feedback / satisfaction (CSAT, NPS)** | Do users actually find it better? |
| **Conversion rate** | Share of users who take the desired action |
| **Average revenue per user (ARPU)** | Revenue impact per customer |
| **Customer lifetime value (CLV)** | Long-term value effect |
| **Efficiency / time saved** | Hours of manual work removed, throughput gained |
| **Task completion rate** | Share of user goals actually achieved (key for agents) |
| **Adoption / engagement** | Are people using it? |

> **Exam framing:** model metrics answer *"is the model good?"* Business metrics answer *"is the project worth it?"* Both are required. If a question asks how to judge whether an ML initiative **succeeded for the business**, pick ROI/cost/satisfaction — not F1.

---

## Exam traps for this task

1. **Validation set ≠ test set.** Validation tunes hyperparameters during development; the test set is touched once, at the end.
2. **Accuracy is the wrong metric on imbalanced data.** Reach for precision/recall/F1/AUC.
3. **Recall for missed-positive costs; precision for false-alarm costs.** Decide by asking which error hurts more.
4. **Model metrics vs. business metrics** — know which the question is asking for.
5. **Amazon Quick is BI/analytics/automation**, not model training. It's the renamed and expanded QuickSight.
6. **Kiro is an agentic IDE** for spec-driven development — a developer tool, not a data or hosting service.
7. **Managed API vs. self-hosted** — "minimal operational overhead" vs. "full control."
8. **Training from scratch is almost never the right answer** for adapting an FM. Cost questions favor prompt engineering and RAG.
9. **Drift means retrain**, and Model Monitor is what detects it.
10. **MLOps ≠ DevOps.** The addition is versioning and monitoring *data and models*, not just code.
