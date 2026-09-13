# Task 4.2 — Transparent and explainable models

> Domain 4 (14%). Small task, but it owns a distinction the exam tests directly: **transparency ≠ explainability**, and the **accuracy/interpretability tradeoff**.

---

## 1. Transparency vs. explainability

These are related but genuinely different, and questions exploit the confusion.

| | **Transparency** | **Explainability** |
|---|---|---|
| **Question it answers** | *What is this system, and how was it built?* | *Why did it produce **this** output?* |
| **Scope** | The system and its provenance | An individual decision |
| **Covers** | Training data, architecture, licence, intended use, known limitations, that AI is being used at all | Which inputs drove this prediction and how much |
| **Delivered by** | Model cards, AI Service Cards, documentation, open weights, published licence | SHAP/feature attribution, decision paths, citations, reasoning traces |

> AWS's own framing: **transparency** = "enabling stakeholders to make informed choices about their engagement with an AI system." **Explainability** = "understanding and evaluating system outputs."
>
> A common working definition: **the ability to understand how a model arrives at a prediction.** Note it is about **how a prediction is reached** — that framing is the tell.

### ⚖️ Interpretability vs. Explainability — a distinction AWS tests directly

**These are NOT synonyms**, and AWS has a whitepaper page devoted to separating them. The exam quotes its wording almost verbatim.

| | **Interpretability** | **Explainability** |
|---|---|---|
| **AWS's definition** | Wanting **high model transparency** to understand **exactly why and how the model generates predictions**, by **observing the inner mechanics** — interpreting the model's weights and features | Taking an ML model and **explaining its behaviour in human terms**. With black boxes you **cannot** fully see how the inner mechanics drive the prediction |
| **The question it answers** | **"How does the model work?"** | **"What else can the model tell me?"** |
| **How you get it** | Use a model that is **inherently** understandable — you read the mechanism itself | **Post-hoc** techniques applied to an existing black box: SHAP, LIME, partial dependence plots, surrogate models |
| **Requires seeing inside?** | ✅ **Yes — that's the definition** | ❌ No — works from the outside |
| **Cost** | **High interpretability usually costs performance** (simpler models) | Lets you **keep a high-performing black box** and still explain it |

> 🔑 **The phrases that decide it:**
> - *"observe the **inner mechanics**"*, *"understand **exactly how** the model generates a prediction"*, *"interpret the model's weights"* → **INTERPRETABILITY**
> - *"explain **this decision** in human terms"*, *"which features drove **this** prediction"*, *"SHAP"* → **EXPLAINABILITY**
>
> **Interpretability is about the MECHANISM. Explainability is about the BEHAVIOUR.**

> **The practical tradeoff AWS draws:** if you want **high performance *and* some understanding**, you generally give up interpretability and lean on **explainability** techniques instead. That's why SHAP exists — it explains models you cannot interpret.

> **Don't confuse either with the neighbours:** **Guardrails** are safety controls (filtering harmful content) — nothing to do with understanding the model. **Model evaluation** measures *how well* it performs — not *how* it works.

### Interpretable vs. black-box models

| | Interpretable ("white box") | Black box |
|---|---|---|
| **Examples** | Linear/logistic regression, decision trees, rule-based systems | Deep neural networks, LLMs, large ensembles |
| **How you understand it** | Read the model directly — coefficients, branches | Only through post-hoc approximation (SHAP, LIME) |
| **Typical accuracy on complex data** | Lower | Higher |
| **Suits** | Regulated decisions needing justification | Complex perception and language tasks |

### 📊 The interpretability ranking — most to least

**Learn this order.** A question asking which model has the **HIGHEST interpretability** is answered straight off this list.

| Rank | Model type | Why |
|---|---|---|
| 🥇 **1. Linear regression** | **HIGHEST** | The **coefficients directly tell you the influence of each feature**. You read the mechanism off the model itself |
| 🥈 **2. Decision trees / rule-based** | Very high | You can trace the exact branches a prediction followed |
| **3. Ensemble methods** (random forest, boosting) | **LOW** | Individual models *may* be interpretable, but **aggregating many models obscures the overall decision process** |
| **4. Neural networks** | **LOW** | Multilayered structure and **non-linear transformations** — hard to trace how inputs led to a prediction |
| 🚫 **5. LLMs** | **VERY LOWEST** | Transformer architectures with **billions of parameters** — extremely difficult to know how any specific output arose |

> ⚠️ **The ensemble subtlety worth knowing:** a random forest is built from decision trees, and a single decision tree is *highly* interpretable. But **the forest as a whole is not** — the aggregation of hundreds of trees hides the decision-making. **"Made of interpretable parts" ≠ "interpretable."**

#### Why linear regression is the most interpretable

**What it is:** linear regression fits a **straight-line relationship** between input features and the output. The whole model is a single readable formula:

```
prediction = intercept + (coefficient₁ × feature₁) + (coefficient₂ × feature₂) + …
```

**A concrete example — predicting house price:**

```
price = 50,000 + (200 × square_feet) + (10,000 × bedrooms) − (3,000 × age_years)
```

You can read that sentence out loud in plain English:
- Every extra **square foot adds £200**
- Every extra **bedroom adds £10,000**
- Every **year of age subtracts £3,000That is the entire model.** Nothing is hidden. **The coefficients *are* the explanation** — you don't need SHAP or any post-hoc tool, because you can inspect the mechanism directly.

> 🔑 **Tie back to [interpretability vs. explainability](#️-interpretability-vs-explainability--a-distinction-aws-tests-directly):** linear regression is the purest case of **interpretability** — you observe the **inner mechanics** and see exactly *how* the prediction is generated. Contrast an LLM, where billions of parameters mean the mechanism is unreadable and you're forced to fall back on **explainability** techniques instead.

> 🚨 **The counterintuitive part.** **LLM** is a tempting pick here, and it is the **least** interpretable option of all. The word to anchor on: **HIGHEST interpretability → simplest model.** Sophistication and interpretability run in **opposite** directions — the more capable the model, the less you can see inside it.

### 🔧 Analogy — Two mechanics

Your car makes a noise. You take it to two garages.

**The first mechanic** hands you an itemized invoice: worn front brake pads, part number, 1.5 hours labour, and the old pad on the counter so you can see the wear. You can check the part price yourself, get a second opinion, and dispute a line if it looks wrong. If the repair fails, you can trace exactly what was done and why. **That's an interpretable model** — you can read the reasoning directly.

**The second mechanic** listens to the engine for ten seconds, says "£900, it'll be ready Thursday," and won't elaborate. Here's the uncomfortable part: **she might genuinely be the better mechanic.** Thirty years of experience produce a diagnosis her conscious mind can't fully articulate, and she's right more often than the first one. But you cannot verify it, cannot challenge it, and cannot explain it to your insurer. **That's a black-box model** — and it captures the real tradeoff: *the less explainable option is often the more accurate one.*

Now separate the two ideas the exam keeps confusing:

- Knowing the garage's credentials, what training the mechanics have, which parts brands they use, and their published complaint rate — that's **transparency**. It's about the *system*, and you can have it before any work happens.
- Getting the itemized invoice explaining *this specific £900 repair* — that's **explainability**. It's about *one decision*, after the fact.

You can have one without the other. A famous, fully documented garage that still won't tell you why your bill is £900 is **transparent but not explainable**. A back-street mechanic who talks you through every step but whose qualifications you can't verify is **explainable but not transparent**.

| At the garage | In AI |
|---|---|
| Credentials, parts brands, published complaint rate | **Transparency** — model cards, licence, training data, intended use |
| The itemized invoice for *this* repair | **Explainability** — SHAP, feature attribution, citations |
| The invoice you can read line by line | **Interpretable model** — regression, decision tree |
| "£900, trust me" — possibly more accurate | **Black box** — deep neural net, LLM |
| Needing the invoice for an insurance claim | **Regulated decision requiring justification** |

---

## 2. Tools for transparency and explainability

| Tool | What it provides |
|---|---|
| **Amazon SageMaker Model Cards** | Documented record of a model: intended use, training data, performance, limitations, risk rating, approval status. **The answer to "document our models for governance and audit"** |
| **AWS AI Service Cards** | AWS's published transparency documents for **its own** AI services and models — see the dedicated section below |
| **SageMaker Clarify** | **SHAP** feature attributions — local (per prediction) and global (overall feature importance) — plus bias metrics |
| **Amazon Bedrock Model Evaluation** | Compare models on quality and responsible-AI metrics before choosing one |
| **Open-source models** | Inspectable weights, architecture, and often training-data documentation |
| **Data and licensing documentation** | Provenance, permitted use, restrictions on outputs |
| **RAG citations** | Every claim traceable to a source document — a practical form of explainability for GenAI |
| **Chain-of-thought reasoning** | Makes the model's steps visible (though the stated reasoning isn't a guaranteed account of the actual computation) |

> **Exam cue:** "document intended use, training details, limitations, and approval for governance" → **SageMaker Model Cards**. "AWS's published documentation about its own AI services' limitations" → **AI Service Cards**. "explain which features drove a prediction" → **SageMaker Clarify / SHAP**.

### 📋 AWS AI Service Cards — the full picture

**What they are:** AWS's own **published transparency documents** for **AWS AI services and AWS-developed models**. Each card is a single place to find:

- **Intended use cases** and, crucially, **use cases the service is NOT intended for**
- **Limitations** and known weaknesses
- **Responsible AI design choices** AWS made when building it
- **Performance optimization best practices**
- Sometimes **sample performance data on common benchmark datasets**

Examples exist for Amazon Transcribe, Amazon Textract, Amazon Rekognition face matching, and the Amazon Nova models.

**You read them; you don't write them.** They're AWS's disclosure to *you*, published on AWS's website — not something you generate for your own models.

#### ⚠️ Their three hard limits — each one is a common wrong answer

| Limit | Consequence |
|---|---|
| **Only cover AWS-developed models and services** | They say **nothing about open-source or third-party models**, so they can't help you compare a mixed candidate list |
| **Don't cover YOUR custom models** | If you train your own algorithm, no Service Card describes it. **For your own models you need SageMaker Model Cards** |
| **Sample benchmark data only** | Each provider chooses what to disclose. **Test on your own use-case dataset** rather than trusting published benchmarks |

#### The comparison that keeps being tested

| | **AWS AI Service Cards** | **SageMaker Model Cards** |
|---|---|---|
| **Documents** | **AWS's** services and models | **Your** models |
| **Written by** | AWS | You (auto-populated for models trained in SageMaker AI) |
| **Purpose** | Help you choose and use an AWS service responsibly | Governance, versioned records, **compliance audits** |
| **Custom algorithms?** | ❌ Not covered | ✅ Exactly what they're for |
| **Exam cue** | "AWS's documentation of its own service's limitations" | "**transparency into model training and performance for compliance audits**" |

> 🚨 **The trap:** a healthcare company builds its **own** ML algorithm and needs transparency into **training and performance** for compliance audits. **AI Service Cards is the wrong answer** — it documents AWS's services, not your custom algorithm. The answer is **SageMaker Model Cards + SageMaker Clarify**.

---

## 3. The tradeoffs

### Accuracy vs. interpretability
The central tension: **the most accurate models are usually the least interpretable.** A decision tree you can read may be 85% accurate; a deep ensemble you can't may hit 94%. Which you choose depends on whether the decision must be *justified*.

- **Choose interpretability** for regulated decisions — credit, insurance, hiring, medical, criminal justice — where you must explain a decision to a regulator or the affected person.
- **Choose accuracy** where performance dominates and decisions don't require individual justification — image classification, recommendations, demand forecasting.

### Safety vs. transparency
The objective names this one specifically, and it's less obvious:

- **Full disclosure can be exploited.** Publishing exact filter thresholds, guardrail rules, or system prompts gives attackers a map for jailbreaking and evasion.
- **Too little disclosure destroys accountability** — users can't tell how the system works or challenge it.
- **The resolution:** disclose *purpose, limitations, data handling, and the fact that AI is in use* — while keeping *specific defensive parameters* confidential. Transparency about **what and why**, not necessarily about **exact thresholds**.

### Performance vs. explainability overhead
Generating explanations (SHAP is computationally expensive) adds latency and cost. Sometimes explanations are produced offline or on demand rather than for every inference.

---

## 4. Human-centred design for explainable AI

| Principle | What it means in practice |
|---|---|
| **AI decision transparency** | Tell users an AI is involved, what it's doing, and how confident it is |
| **User-feedback mechanisms** | Let users flag wrong or harmful output — feedback is both a safety net and training signal |
| **Appropriate trust calibration** | Help users trust the system *as much as it deserves* — neither blind faith nor blanket rejection. Show confidence and uncertainty honestly |
| **Contestability / recourse** | A clear route to challenge a decision and reach a human |
| **Explanations fit the audience** | An end user, a domain expert, and a regulator need different explanations of the same decision |
| **Human oversight for high-stakes decisions** | Human-in-the-loop review where consequences are serious |
| **Progressive disclosure** | A short answer by default, with detail available on request |
| **Graceful failure** | Say "I don't know" rather than fabricating |

> **Trap:** more explanation is not automatically better. Overwhelming a user with detail is its own failure — explanations must be **actionable for the person receiving them**.

---

## Exam traps for this task

1. **Transparency ≠ explainability.** System-level and up front vs. decision-level and after the fact.
2. **The most accurate model is often the least interpretable** — that's the tradeoff, not a flaw to be fixed.
3. **Regulated decisions favour interpretable models**, even at a cost in accuracy.
4. **SageMaker Model Cards = your model documentation. AI Service Cards = AWS's documentation of its own services.** Don't swap them.
5. **SHAP (via SageMaker Clarify) is the feature-attribution answer.**
6. **Full transparency can undermine safety** — publishing exact guardrail thresholds helps attackers.
7. **RAG citations are a practical explainability mechanism** for GenAI.
8. **Users should be told when they're interacting with AI** — a core transparency requirement.
9. **Feedback mechanisms are part of responsible design**, not just a product nicety.
