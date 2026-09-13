# Amazon SageMaker AI — the complete component map

> **Why SageMaker Canvas, Studio, Clarify, Model Registry etc. aren't in the exam guide's service list.**
> The in-scope list is written at **service** level, not **feature** level. It names only **Amazon SageMaker AI** and **Amazon SageMaker JumpStart** — everything below is a *component of* SageMaker AI and is covered by that single entry. The list also says outright that it is **"non-exhaustive and subject to change."**
>
> **They are absolutely tested.** Practice material regularly asks about **Canvas, Studio, Model Registry, Clarify, Ground Truth, Model Monitor, Model Cards, and Amazon A2I** — none of which appear in the published list by name. Don't take absence from that list as absence from the exam.

---

## 1. Every component, by lifecycle stage

### Data preparation
| Component | What it does | Exam cue |
|---|---|---|
| **Ground Truth** | **Data labeling** with human labelers (your team, vendors, or Mechanical Turk) | "label a large training dataset", "human annotation" |
| **Data Wrangler** | Visual **data prep and transformation**, EDA, feature engineering | "prepare and transform data with minimal code" |
| **Feature Store** | Central store to **save, share, and reuse features** across teams and models | "reuse features", "consistent features for training and inference" |
| **Processing** | Run preprocessing / postprocessing / evaluation jobs at scale | "run a processing job" |

### Build and develop
| Component | What it does | Exam cue |
|---|---|---|
| **Studio** | The unified **web IDE** for ML — JupyterLab, RStudio, Code Editor (Code-OSS). Where **data scientists** work | "integrated development environment for ML" |
| **Notebook instances** | Managed Jupyter compute for **development, experimentation, and testing**. **Not for production deployment** — not scalable, manual management | "experiment with a model", "develop and test" |
| **Canvas** | **NO-CODE** visual ML — build and predict without writing code. For **business analysts** | "**no coding required**", "business analyst builds a model" |
| **JumpStart** | Hub of **pre-trained models and prebuilt solution templates**, deployable in a few clicks | "start from a pre-trained model quickly" |
| **Autopilot** | **AutoML** — automatically does feature engineering, algorithm selection, and tuning, then ranks candidates on a leaderboard | "automatically build the best model" |

### Train and tune
| Component | What it does |
|---|---|
| **Training jobs** | Managed training on your chosen instances |
| **Automatic Model Tuning** (HPO) | Searches **hyperparameter** combinations for the best model |
| **Experiments** | Tracks runs, parameters, and metrics so results are comparable and reproducible |
| **Debugger** | Inspects training in progress; catches issues like vanishing gradients |

### Evaluate and govern
| Component | What it does | Exam cue |
|---|---|---|
| **Clarify** | Three jobs: **① bias detection** (pre-training in data, post-training in predictions), **② explainability** via **SHAP**, and **③ FM evaluation via the FMEval library** — quality *and* responsibility metrics including **toxicity** and bias scores, over built-in or your own test datasets | "detect bias", "**explain model predictions**", "feature importance", "**quantify model toxicity at scale**" |
| **Model Cards** | **Documents** a model: intended use, training details, evaluation metrics, risk rating, approval status. Exportable for audits | "document models for compliance audits", "transparency into training and performance" |
| **Model Registry** | **Versioned catalog** of models with metadata and an **approval workflow** | "central catalog", "manage model versions and approval", "share models between teams" |
| **Model Dashboard** | Single view of all models, endpoints, and monitoring status | "overview of all models" |

### Deploy

> 🚀 ** The default answer for "deploy a model to production with the LEAST operational overhead" is a SageMaker AI endpoint.** They're **fully managed, scalable, and require minimal setup** — SageMaker handles the infrastructure and scaling for you.
>
> **The development ↔ production boundary the exam tests:**
>
> | Tool | Purpose | Production-ready? |
> |---|---|---|
> | **SageMaker AI endpoints** | Serving models to production traffic | ✅ **Yes** — managed, auto-scaling |
> | **SageMaker notebook instances** | Model **development, experimentation, testing** | ❌ **No.** You *can* run a model on one, but it isn't scalable, needs manual management, and lacks automated scaling and serving |
> | **S3 + calling the model locally** | Storage | ❌ **No** — you'd build serving, scaling, and management yourself |
> | **Amazon Personalize endpoints** | **Personalization use cases only** | ❌ You **cannot deploy an arbitrary model** to them |
>
> **The trap:** *"notebook instance"* sounds like a legitimate way to run a model — and it is, for **experimenting**. It is not a deployment mechanism. **Notebooks are where you build; endpoints are where you serve.**

| Option | Use when |
|---|---|
| **Real-time endpoint** | Persistent, low millisecond latency, always on |
| **Serverless inference** | Spiky/intermittent traffic, scales to zero, cold starts |
| **Asynchronous inference** | Large payloads (up to 1 GB), long processing (up to 1 hr), queued |
| **Batch transform** | Offline scoring of a whole dataset, no endpoint |
| **Multi-model endpoints** | Many models behind one endpoint to save cost |
| **Neo** | Compile/optimize models for target hardware |

### Monitor and improve
| Component | What it does | Exam cue |
|---|---|---|
| **Model Monitor** | Detects **drift** in production — data quality, model quality, bias drift, feature attribution drift. Alarms via CloudWatch | "detect model degradation over time" |
| **Amazon A2I** (Augmented AI) | **Human review workflows for individual predictions** — routes low-confidence predictions to people | "human review of predictions" |
| **Pipelines** | **CI/CD orchestration** for ML — repeatable, automated workflows | "automate and repeat the ML workflow" |

> ⚠️ **A2I vs. Model Cards — a classic trap.** **A2I** reviews *individual predictions* with humans. **Model Cards** produce *system-level documentation* for compliance audits. A question about **audit reporting** wants Model Cards (and Clarify), **not** A2I.

---

## 1b. The compliance & explainability cluster

**"Transparency into model training and performance for compliance audits"** → **Model Cards + Clarify** (both, on a multi-select).

| Service | Why it qualifies | Why the others don't |
|---|---|---|
| **SageMaker Model Cards** ✅ | Documents intended uses, **risk ratings, training details, evaluation metrics, performance**. **Auto-populates training info** for models trained in SageMaker AI. Edits tracked as new versions. **Exportable to share with stakeholders and support compliance audits** | — |
| **SageMaker Clarify** ✅ | Provides insights into **bias and feature importance**, and **produces baselines and reports to support audits** | — |
| **AWS AI Service Cards** ❌ | Documents **AWS's own** services — gives **no insight into the training or performance of your custom algorithm** |
| **SageMaker Ground Truth** ❌ | Improves **labeled dataset quality**. Gives **no visibility into model training or performance** |
| **Amazon A2I** ❌ | Human audit of **individual predictions**. **Not a reporting tool for system-level compliance audits** |

**"Evaluate models AND explain model predictions"** → **SageMaker Clarify**, alone.

| Service | Verdict |
|---|---|
| **Clarify** ✅ | The only one that **evaluates and explains predictions** — bias insights plus **feature importance on individual decisions** |
| **Model Monitor** ❌ | Detects **deviations in behaviour** over time. Doesn't explain predictions |
| **Ground Truth** ❌ | **Labels data.** Nothing to do with explaining predictions |
| **Studio** ❌ | An **IDE**; supports versioning and collaboration, but doesn't itself evaluate or explain predictions |

> 🔑 **The single fact that answers both questions:** **Clarify is AWS's explainability service.** Any question containing *"explain model predictions"*, *"feature importance"*, *"why did the model decide this"*, or *"regulatory compliance requires we justify decisions"* → **SageMaker Clarify**. Model Monitor watches, Ground Truth labels, Studio hosts the notebooks — **only Clarify explains.**

---

## 2. How they interact — the flows

There is no single path. These are the four that matter, and the exam tests which one fits the scenario.

### Flow 1 — The no-code path: Canvas → Model Registry → Studio

A business analyst builds in Canvas; data scientists review and approve in Studio. **Model Registry is the hand-off point between them.**

```
  BUSINESS ANALYST                 SHARED                    DATA SCIENTIST
  ────────────────                 ──────                    ──────────────

  ┌──────────────┐          ┌──────────────────┐          ┌──────────────┐
  │   SageMaker  │  push    │    SageMaker     │  access  │   SageMaker  │
  │    CANVAS    │ ───────► │  MODEL REGISTRY  │ ◄─────── │    STUDIO    │
  │  (no code)   │  model   │                  │  same    │ (data scien- │
  └──────────────┘          │ • versions       │ registry │  tists)      │
                            │ • metadata       │          └──────┬───────┘
                            │ • approval status│                 │
                            └────────┬─────────┘         review &│approve
                                     │                           │
                                     │  ◄────────────────────────┘
                                     ▼
                              deploy to endpoint
```

**Why Model Registry and not the alternatives:**

| Option | Verdict |
|---|---|
| **SageMaker Model Registry** ✓ | Fully managed catalog. Canvas **pushes** models to it; Studio users **access the same registry**. **Least operational overhead** — you only register the models |
| **Amazon DynamoDB** | You *could* build a custom registry on it, but you'd design, configure, and scale the tables yourself → **more** overhead |
| **Amazon ECR** | A **container image** registry, not a model registry |
| **SageMaker JumpStart** | ❌ **A hub of pre-existing open-source/pre-trained models. You CANNOT insert your own models into it.** This is the trap |

### Flow 2 — The code-first path (data scientists in Studio)

The full custom-model lifecycle:

```
 Ground Truth ──► Data Wrangler ──► Feature Store
   (label)          (prepare)         (store/reuse)
                                           │
                                           ▼
                                  ┌─────────────────┐
                                  │ Studio: Training│◄──┐
                                  │      jobs       │   │ Experiments
                                  └────────┬────────┘   │ (track runs)
                                           │            │
                                  Automatic Model ──────┘
                                  Tuning (HPO)
                                           │
                                           ▼
                        ┌──────────── Clarify ────────────┐
                        │  bias detection + SHAP explain  │
                        └────────────────┬────────────────┘
                                         ▼
                              Model Cards (document)
                                         │
                                         ▼
                              Model Registry (version + approve)
                                         │
                                         ▼
                         Deploy: real-time / serverless / async / batch
                                         │
                                         ▼
                   Model Monitor (drift) ──► CloudWatch alarm
                                         │
                                         └──► retrain (loop back to training)
```

### Flow 3 — The fast path: JumpStart

```
  JumpStart hub ──► pick a pre-trained model / solution template
        │
        ├──► deploy directly to an endpoint          (fastest)
        │
        └──► fine-tune on your data ──► deploy       (customized)
```

> Use when you want an existing model quickly. **One-directional: you take models OUT of JumpStart. You never put yours in.**

### Flow 4 — The AutoML path: Autopilot

```
  Tabular data in S3
        │
        ▼
  Autopilot ──► auto feature engineering
        │       auto algorithm selection
        │       auto hyperparameter tuning
        │       k-fold cross-validation (k=5 for ≤50,000 rows in HPO mode)
        ▼
  Candidate leaderboard (ranked by objective metric)
        │
        ▼
  Deploy the best candidate
```

### Flow 5 — The governance overlay

These wrap around *any* of the flows above:

```
       ┌──────────────────── GOVERNANCE ────────────────────┐
       │                                                    │
  Clarify ──────► bias + explainability (SHAP)              │
  Model Cards ──► documentation for compliance audits       │
  Model Registry► version control + approval workflow       │
  Model Monitor► production drift detection                 │
  A2I ──────────► human review of individual predictions    │
       │                                                    │
       └────────────────────────────────────────────────────┘
```

---

## 3. The confusions the exam exploits

| Pair | The distinction |
|---|---|
| **Studio vs. Canvas** | Studio = **code**, for data scientists (IDE). Canvas = **no code**, for business analysts |
| **Model Registry vs. JumpStart** | Registry = **your** models, versioned and approved. JumpStart = **pre-existing** models to consume. **You can't push your model into JumpStart** |
| **Model Registry vs. ECR** | Models vs. **container images** |
| **Model Cards vs. Model Registry** | Cards = **documentation** of a model. Registry = **catalog + versions + approval** |
| **Model Cards vs. AI Service Cards** | Model Cards document **your** models. AI Service Cards are **AWS's** docs for **its own** services |
| **Clarify vs. Model Monitor** | Clarify = bias **and explainability** analysis. Model Monitor = **production drift** detection (and calls Clarify for bias drift) |
| **Clarify vs. Ground Truth** | Clarify = explain and detect bias. Ground Truth = **label data** |
| **A2I vs. Model Cards** | A2I = human review of **individual predictions**. Model Cards = **system-level audit documentation** |
| **A2I vs. Clarify/FMEval** | A2I = **humans reviewing production responses** (doesn't scale, can't pick a model). Clarify/FMEval = **automated evaluation over a test dataset to SELECT an FM at scale** |
| **Ground Truth vs. Data Wrangler** | Ground Truth = **labeling**. Data Wrangler = **transformation/prep** |
| **Autopilot vs. JumpStart** | Autopilot = **AutoML builds a model from your data**. JumpStart = **take an existing pre-trained model** |

---

## 4. Cue-to-component quick lookup

| The question says… | Answer |
|---|---|
| "no coding experience / business analyst builds a model" | **Canvas** |
| "IDE for ML / integrated development environment" | **Studio** |
| "label a large training dataset" | **Ground Truth** |
| "prepare and transform data visually" | **Data Wrangler** |
| "reuse features across models" | **Feature Store** |
| "automatically build and rank models" | **Autopilot** |
| "start from a pre-trained model" | **JumpStart** |
| "**explain model predictions**" / "detect bias" | **Clarify** |
| "document intended use, training details, metrics for **audits**" | **Model Cards** |
| "central catalog, versions, **approval**, share between teams" | **Model Registry** |
| "detect drift / model degrading in production" | **Model Monitor** |
| "human review of individual predictions" | **Amazon A2I** |
| "automate a repeatable ML workflow" | **Pipelines** |
| "tune hyperparameters automatically" | **Automatic Model Tuning** |

---

## Related notes

- ML lifecycle and metrics: [Domain 1, Task 1.3](../domains/01-ai-ml-fundamentals/03-ai-ml-development-lifecycle.md)
- Inference types (real-time / serverless / async / batch): [Domain 1, Task 1.1](../domains/01-ai-ml-fundamentals/01-ai-concepts-terminology.md)
- Bedrock vs. SageMaker AI: [Domain 2, Task 2.3](../domains/02-genai-fundamentals/03-aws-genai-infrastructure.md)
- Model Cards for transparency: [Domain 4, Task 4.2](../domains/04-responsible-ai/02-transparency-explainability.md)
