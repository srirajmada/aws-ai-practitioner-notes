# Domain 4 — Flashcards

Cover the right column and work down. Anything you miss twice, go back to the source note.

---

## Task 4.1 — Responsible AI development

| Prompt | Answer |
|---|---|
| **AWS's eight dimensions of responsible AI** | Fairness, explainability, privacy and security, safety, controllability, veracity and robustness, governance, transparency |
| AWS's definition of fairness | Considering impacts on different groups of stakeholders |
| AWS's definition of controllability | Having mechanisms to monitor and steer AI system behaviour |
| AWS's definition of veracity and robustness | Achieving correct system outputs, even with unexpected or adversarial inputs |
| Bias vs. fairness | **Bias is the defect** (systematic unfair skew); **fairness is the goal** (equitable outcomes) |
| What is inclusivity | The system works for and represents diverse users |
| Name six sources of bias | Data/sampling, historical, labeling, algorithmic, confirmation, selection |
| Which source of bias is most common | **Data** — unrepresentative training data |
| What is historical bias | The data accurately records a biased past (e.g. decades of discriminatory hiring) |
| How does overfitting cause demographic harm | The model memorizes the majority it saw well and **fails on under-represented groups** it barely saw |
| How does underfitting show up | Model too simple — poor for everyone, and especially misses minority patterns |
| Six characteristics of a responsible dataset | Inclusivity, diversity, balance, curated sources, accurate labeling, documentation |
| Four ways to rebalance a dataset | Oversample the minority, undersample the majority, synthetic generation (SMOTE), class weights |
| **The technique that exposes hidden group harm** | **Subgroup analysis** — measure performance per demographic group, not just in aggregate |
| Why is aggregate accuracy dangerous | 95% overall can hide 99% for one group and 60% for another |
| What does SageMaker Clarify do | **Both**: bias detection (pre-training in data, post-training in predictions) **and** explainability via **SHAP** |
| Name three Clarify bias metrics | DPL (Difference in Positive Proportions in Labels), DPPL (in predicted labels), Accuracy Difference |
| What detects bias drift in production | **SageMaker Model Monitor** — with CloudWatch alarms past a threshold |
| Two non-automated bias detection methods | Human audits and label quality analysis (inter-annotator agreement) |
| **The six Bedrock Guardrails policy types** | Content filters, denied topics, word filters, sensitive information filters, contextual grounding checks, Automated Reasoning checks |
| The six content filter categories | Hate, Insults, Sexual, Violence, Misconduct, **Prompt Attack** |
| Which Guardrails policy blocks off-limits subjects | **Denied topics** |
| Which Guardrails policy redacts SSNs | **Sensitive information filters** (PII + custom regex) — can block or mask |
| Which Guardrails policy detects hallucination | **Contextual grounding checks** — flags responses not grounded in the source or irrelevant to the question |
| Which Guardrails policy validates against logical rules | **Automated Reasoning checks** — also suggests corrections and highlights unstated assumptions |
| What does the `ApplyGuardrail` API let you do | Evaluate text **without invoking a model** — so guardrails can protect non-Bedrock and self-managed models |
| Name six legal/business risks of GenAI | IP infringement, biased outputs, hallucinations, loss of customer trust, end user risk, privacy violations (also regulatory non-compliance, defamation) |
| **What does "infringement protection capabilities" mean?** | **IP indemnification** — a legal/contractual protection where the provider **defends you and covers costs/damages** if a third party sues claiming the model's **output** infringes their IP |
| What does AWS offer, and for which models? | **Uncapped IP indemnity** (no dollar ceiling) on the **output** of generally available **Amazon-developed models — Titan and Nova** — via Amazon Bedrock |
| **What can void the indemnity?** | Irresponsible use — inputting infringing data, or **disabling the content filtering features** |
| Do Hugging Face / JumpStart / open-source models carry AWS IP indemnity? | **No** — only Amazon-developed models. Third-party models on Bedrock have **separate terms** |
| **"No infrastructure management + infringement protection"** → | **Amazon Titan (or Nova) FM in Amazon Bedrock** — the only option satisfying both filters |
| 🔑 Technique for two-requirement questions | **Check every option against BOTH filters.** Here: "no infra management" kills DLC/JumpStart/SageMaker; "infringement protection" kills open-source/third-party. Only one survives both |
| Responsible model selection — the sustainability answer | Use the **smallest sufficient model**; prefer distillation, caching, and shared managed infrastructure |
| Which Amazon models watermark generated content | **Nova Canvas** (images) and **Nova Reel** (video) |

---

## Task 4.2 — Transparency and explainability

| Prompt | Answer |
|---|---|
| **Transparency vs. explainability** | Transparency = *what is this system and how was it built* (system level, up front). Explainability = *why did it produce **this** output* (decision level, after the fact) |
| **"Observe the INNER MECHANICS of a model / understand exactly HOW it generates a prediction"** → | **INTERPRETABILITY** (not explainability) |
| **Interpretability vs. explainability** | **Interpretability** = see **inside** the model, understand the **mechanism** (weights, features). **Explainability** = explain the **behaviour** in human terms, **post-hoc**, without seeing inside |
| The question each one answers | Interpretability: **"How does the model work?"** Explainability: **"What else can the model tell me?"** |
| How do you achieve each? | Interpretability → use an **inherently understandable** model. Explainability → **post-hoc** techniques (SHAP, LIME, partial dependence plots, surrogate models) on a black box |
| The tradeoff between them | **High interpretability usually costs performance.** Want performance *and* understanding? → give up interpretability, use **explainability** techniques |
| Why aren't Guardrails or Model evaluation the answer? | **Guardrails** = safety controls (filter harmful content). **Model evaluation** = measures *how well* it performs. Neither tells you *how* the model works |
| AWS's definition of transparency | Enabling stakeholders to make informed choices about their engagement with an AI system |
| AWS's definition of explainability | Understanding and evaluating system outputs |
| Name three interpretable model types | Linear/logistic regression, decision trees, rule-based systems |
| **Which model type has the HIGHEST interpretability?** | **Linear regression** — its **coefficients directly show each feature's influence** |
| **Rank interpretability, most to least** | **Linear regression → decision trees → ensemble methods → neural networks → LLMs (lowest)** |
| **What is linear regression?** | A straight-line formula: `prediction = intercept + (coef₁ × feature₁) + (coef₂ × feature₂)…` The whole model is one readable equation |
| Why is linear regression so interpretable? | **The coefficients ARE the explanation** — "each extra sq ft adds £200" — no SHAP needed, you read the mechanism directly |
| **Are ensemble models interpretable?** | **No — LOW.** Individual models may be, but **aggregating many models obscures the overall decision process**. "Made of interpretable parts ≠ interpretable" |
| Why do neural networks have low interpretability? | **Multilayered structure + non-linear transformations** — hard to trace how inputs led to a prediction |
| Why do LLMs have the LOWEST interpretability? | Transformer architectures with **billions of parameters** |
| 🔑 The direction to remember | **HIGHEST interpretability → SIMPLEST model.** Sophistication and interpretability run in **opposite** directions |
| Name three black-box model types | Deep neural networks, LLMs, large ensembles |
| **The core tradeoff** | The most accurate models are usually the **least interpretable** |
| When do you choose interpretability over accuracy | Regulated decisions — credit, insurance, hiring, medical, criminal justice — where a decision must be justified |
| What are SageMaker Model Cards | Documented record of **your** model: intended use, training data, performance, limitations, risk rating, approval status |
| What are AWS AI Service Cards | **AWS's** published transparency docs for **its own** AI services — intended use, limitations, responsible AI design choices |
| Model Cards vs. AI Service Cards | Model Cards document *your* models. AI Service Cards document *AWS's* services |
| **What's IN an AWS AI Service Card?** | Intended use cases, **use cases it's NOT for**, limitations, responsible AI design choices, performance best practices, sometimes sample benchmark data |
| **Three limits of AI Service Cards** | ① Only **AWS-developed** models — not open-source/third-party ② **Don't cover YOUR custom models** ③ **Sample benchmark data only** — test on your own dataset |
| Do you write AI Service Cards? | **No** — AWS publishes them for you to read. You write **SageMaker Model Cards** for your own models |
| **"Healthcare co. builds its OWN algorithm, needs transparency into training and performance for compliance audits"** → | **SageMaker Model Cards + SageMaker Clarify** (multi-select). **AI Service Cards is wrong** — it documents AWS's services, not your custom algorithm |
| Why isn't Amazon A2I an audit tool? | A2I does **human review of individual predictions** — it is **not a reporting tool for system-level compliance audits** |
| Why isn't Ground Truth an audit tool? | It **labels data** — gives no visibility into model training or performance |
| **"Evaluate models AND explain model predictions"** → | **SageMaker Clarify** — the only one that explains predictions. Model Monitor detects drift, Ground Truth labels, Studio is an IDE |
| 🔑 Any question with "explain model predictions" / "feature importance" → | **SageMaker Clarify** |
| **Which practices align with responsible AI?** | **Monitor responses for bias** and **test responses for data leakage** |
| Why is "use all available customer data" wrong? | It includes **PII that should be redacted**. Using the full dataset unredacted **increases privacy risk** — data minimization is the responsible choice |
| Why is "train on raw historical data" wrong? | Historical data **under-represents groups** and carries **label bias from outdated unfair practices** — training on it raw injects that bias |
| Why is "use only open source models" wrong? | Transparency is good, but it **doesn't address** fairness for your use case, fine-tunability, or explainability |
| 🔑 The responsible-AI reasoning rule | Responsible AI means **actively mitigating** — redact, test, monitor, review. **"Raw" and "all available" are red-flag words** |
| What technique explains which features drove a prediction | **SHAP** — via SageMaker Clarify. Local (per prediction) and global (overall) |
| A practical explainability mechanism for GenAI | **RAG citations** — every claim traceable to a source document |
| The safety vs. transparency tradeoff | Publishing exact filter thresholds and system prompts helps attackers jailbreak. Disclose purpose, limitations, and data handling — keep specific defensive parameters confidential |
| Cost of generating explanations | SHAP is computationally expensive — adds latency and cost, so explanations are often produced offline or on demand |
| Name five human-centred design principles | AI decision transparency, user feedback mechanisms, trust calibration, contestability/recourse, audience-appropriate explanations (also human oversight, progressive disclosure, graceful failure) |
| What is trust calibration | Helping users trust the system *as much as it deserves* — neither blind faith nor blanket rejection |
| Is more explanation always better | **No** — overwhelming users is its own failure. Explanations must be actionable for the recipient |

---

## Rapid-fire

| Prompt | Answer |
|---|---|
| Domain 4 weighting | 14% (~7 of 50 scored questions) |
| "Overall accuracy is fine but one group is harmed" → | **Subgroup analysis** |
| "Document our model for audit and governance" → | **SageMaker Model Cards** |
| "Detect bias and explain predictions" → | **SageMaker Clarify** |
| "Block harmful content and redact PII at runtime" → | **Bedrock Guardrails** |
| "Make sure the RAG answer sticks to the sources" → | **Contextual grounding checks** |
| "Regulator requires we justify each decision" → | **Interpretable model**, not the most accurate one |
