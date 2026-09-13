# Task 1.2 — Identify practical use cases for AI

> Domain 1 (20%). Scenario-driven: "a company wants to do X — which technique/service?" The two highest-value skills here are **matching a use case to the right AWS AI service** and **recognizing when AI is the wrong tool.**

---

## 1. Where AI/ML delivers value

AI/ML is a good fit when the problem has these properties:

| Value driver | What it looks like | Example |
|---|---|---|
| **Assist human decision making** | Surfaces predictions/rankings a human acts on | Radiologist triage, loan-officer risk scoring |
| **Scalability** | Handles volume no human team could | Moderating millions of posts a day |
| **Automation** | Removes repetitive manual work | Auto-routing support tickets, invoice data extraction |
| **Personalization** | Per-user tailoring at scale | Product recommendations |
| **Pattern detection beyond human capacity** | Finds subtle multi-variable signals | Fraud detection, predictive maintenance |
| **Speed** | Decisions in milliseconds | Real-time bidding, fraud blocking at checkout |
| **Handling unstructured data** | Extracts meaning from text/image/audio | Sentiment on reviews, OCR on forms |

The underlying precondition: **the problem is probabilistic, the data exists, and the pattern is learnable but too complex to hand-code.**

---

## 2. When AI/ML is NOT appropriate

Directly tested by an official objective. AWS wants you to know AI is not always the answer.

| Don't use AI/ML when… | Why | Use instead |
|---|---|---|
| **You need a specific, guaranteed, deterministic outcome** | ML produces *predictions* with uncertainty, not guarantees | Deterministic rules / traditional code |
| **The rules are simple, known, and stable** | Hand-written logic is cheaper, faster, and auditable | if/then business rules |
| **Cost exceeds benefit** | Data labeling, training, and inference cost real money | Do nothing, or a simpler heuristic |
| **You lack sufficient quality data** | No data → no learnable pattern | Collect data first |
| **You must explain every decision precisely** (some regulated contexts) | Complex models are opaque | Interpretable models, or rules |
| **The task demands 100% accuracy** | Models have irreducible error rates | Deterministic system, or human review |

> **The flagship trap:** a question describing a need for a **specific, exact, repeatable outcome** — tax calculation, payroll, regulatory threshold enforcement. The answer is *not* ML. AWS calls this out explicitly ("situations when a specific outcome is needed instead of a prediction").

**Cost-benefit framing to remember:** total cost = data acquisition + labeling + training + inference + monitoring + retraining. If the manual process is cheap, rare, or low-value, ML rarely pays back.

---

## 3. Choosing the ML technique

The core three from the objective, plus the ones that show up as distractors:

| Technique | Learning type | Output | Question cue | Example |
|---|---|---|---|---|
| **Regression** | Supervised | A **continuous number** | "How much", "how many", "predict the value/**volume**/price/demand/sales" | House price, next-month revenue, **annual sales volume**, temperature |
| **Binary classification** | Supervised | One of **two** classes | "Is it X or not", yes/no | Spam / not spam, fraud / legitimate, churn / retain |
| **Multiclass classification** | Supervised | One of **3+** classes | "Which category" | Route ticket to one of 8 departments |
| **Multilabel classification** | Supervised | **Multiple** tags at once | "Which tags apply" | Image tagged both "beach" and "sunset" |
| **Clustering** | Unsupervised | **Groups** discovered by the model | "Segment", "group similar", *no predefined categories* | Customer segmentation |
| **Anomaly detection** | Usually unsupervised | Outlier / normal | "Unusual", "rare", "deviates from normal" | Intrusion detection, equipment failure |
| **Forecasting** | Supervised (time-series) | Future values **over time** | "Predict next quarter", time-ordered data | Demand planning, inventory |
| **Dimensionality reduction** | Unsupervised | Fewer features | "Too many features", "compress while retaining info" | PCA before training |
| **Recommendation** | Mixed | Ranked items per user | "Suggest products/content" | Amazon Personalize |

> **The most-tested pair: classification vs. regression.** Discrete category → classification. Continuous number → regression. "Predict *whether* a customer churns" = classification. "Predict *how much* a customer will spend" = regression.
>
> **The second pair: classification vs. clustering.** If the categories are *already defined and labeled*, it's classification (supervised). If the model must *discover* the groups, it's clustering (unsupervised).

### 🪜 TWO LEVELS of answer — learning type vs. technique

**A question can ask at either level, and you must be able to move between them.**

```
LEVEL 1 — LEARNING TYPE          LEVEL 2 — TECHNIQUE            LEVEL 3 — ALGORITHM
─────────────────────            ──────────────────             ───────────────────
SUPERVISED  ──────────────┬────► Regression       ─────────────► XGBoost, Linear Learner
(labeled: features        ├────► Classification   ─────────────► XGBoost, k-NN
 + a target variable)     └────► Forecasting      ─────────────► DeepAR

UNSUPERVISED ─────────────┬────► Clustering       ─────────────► k-means
(no target variable)      ├────► Anomaly detection ────────────► Random Cut Forest
                          └────► Dimensionality reduction ─────► PCA

REINFORCEMENT ────────────────► Policy learning   ─────────────► Q-learning, PPO
(environment + rewards)
```

> 🚨 **The classic trap.** A scenario like *"predict electricity load usage"* is clearly a **regression** problem — but **"regression" may not be among the options.** If the choices sit at **Level 1** (supervised / unsupervised / reinforcement / NLP), you have to climb one level: **regression is a type of SUPERVISED learning**, so the answer is **supervised learning**.
>
> **Memorize the parent of each technique:**
> - **Regression → SUPERVISED**
> - **Classification → SUPERVISED**
> - **Forecasting → SUPERVISED**
> - **Clustering → UNSUPERVISED**
> - **Anomaly detection → UNSUPERVISED**
> - **Dimensionality reduction → UNSUPERVISED**

#### 🔑 The one-line test for supervised learning

> **Is there a TARGET VARIABLE being predicted FROM other known features?**
> If yes → **supervised**, regardless of whether the output is a number or a category.

Applied to a typical scenario: *predicting **electricity load**, from a dataset containing **past electricity load**, temperature, and cloud coverage.*

| Element | Role |
|---|---|
| **Electricity load** | The **target variable** — and note it's *in the training dataset*, so the data is **labeled** |
| **Temperature, cloud coverage** | The **input features** |
| **Verdict** | Labeled target + features → **SUPERVISED**. It's regression, and regression lives under supervised |

**Why the distractors fail:**

| Option | Why wrong |
|---|---|
| **Reinforcement learning** | **None of the RL signals are present** — no agent, no environment, no trial and error, no rewards. Just known variables predicting a known target |
| **Unsupervised learning** | Used when there is **no labeled target variable**. Here there clearly is one (electricity load), which "makes this a supervised learning problem" |
| **NLP** | The data is **numerical** (load, temperature, cloud coverage) — no human language anywhere |

> **The give-away that the data is labeled:** the thing you want to predict (**electricity load**) *appears in the training dataset*. If the target is in the data, you have labels, and labels mean supervised.

### 🧮 Named algorithms → problem type

The exam names **specific algorithms**, not just problem families. This is the algorithm counterpart to the [metric vs. problem type table](03-ai-ml-development-lifecycle.md) — same discipline: **match the algorithm family to the shape of the problem first.**

| Algorithm family | Named examples | Problem it solves | Question cue |
|---|---|---|---|
| **Forecasting** | **DeepAR** | Predict **future values of time-series data** | readings collected over time; predicting the next N hours or quarters |
| **Anomaly detection** | **Random Cut Forest (RCF)**, IP Insights | Find rare/unusual data points | "detect anomalies", "unusual activity", "outliers" |
| **Clustering** (unsupervised) | **k-means** | Discover groups nobody defined | "segment customers", "group similar items" |
| **Classification** (supervised) | **XGBoost**, Linear Learner, k-NN | Assign to **predefined classes** | "spam or not", "which of 5 categories" |
| **Regression** (supervised) | **XGBoost**, Linear Learner | Predict a **continuous number** | "how much", "predict the price/volume" |
| **Dimensionality reduction** | **PCA** | Compress many features | "too many features" |
| **Text / NLP** | BlazingText, Seq2Seq | Text classification, embeddings, translation | — |
| **Computer vision** | Image Classification, Object Detection, Semantic Segmentation | Analyze images | — |

#### Forecasting — the family that catches people out

**Forecasting is its own algorithm family, not a flavour of regression**, and it's the answer whenever the data is **time-ordered and you're predicting forward in time**.

- **DeepAR** is the named example: a forecasting algorithm for **one-dimensional time series data**.
- Hourly temperature readings collected over a month, used to predict the next day, are *exactly* one-dimensional time series.

> **Why the distractors fail on a forecasting question:**
> - **Clustering (k-means)** — unsupervised *grouping*. The task isn't to group anything.
> - **Anomaly detection (RCF)** — unsupervised, finds *outliers*. The task isn't to spot the weird reading.
> - **Classification** — supervised, but outputs a **category**. Temperature is a **continuous value over time**.
>
> **The tell is always "readings over a period of time" + "predict the next N hours/days."** That combination means forecasting.

---

## 4. Real-world AI application categories

| Application | What it does | AWS service |
|---|---|---|
| **Computer vision** | Identify objects, faces, scenes, text in images/video | Amazon Rekognition |
| **NLP** | Sentiment, entities, key phrases, language, PII | Amazon Comprehend |
| **Speech recognition (STT)** | Audio → text | Amazon Transcribe |
| **Speech synthesis (TTS)** | Text → lifelike speech | Amazon Polly |
| **Translation** | Language → language | Amazon Translate |
| **Document extraction** | Text/tables/forms out of scanned docs | Amazon Textract |
| **Conversational AI** | Chatbots, IVR, intent + slot filling | Amazon Lex |
| **Recommendation systems** | Personalized item ranking | Amazon Personalize |
| **Fraud detection** | Flag anomalous transactions | SageMaker AI (custom) |
| **Forecasting** | Time-series prediction | SageMaker AI (custom) |
| **Knowledge bases** | Grounded Q&A over your own documents (RAG) | Amazon Bedrock Knowledge Bases |
| **Agentic AI** | Goal-directed, multi-step, tool-using automation | Bedrock Agents / AgentCore, Strands Agents |

The last two are named in the objective and are newer additions — enough detail for Domain 1:

- **Knowledge base (RAG).** A foundation model only knows what it was trained on. A knowledge base stores *your* documents as embeddings in a vector store; at query time the relevant passages are retrieved and inserted into the prompt, so the model answers **grounded in your data with citations**. This is **Retrieval Augmented Generation**, and it's the standard answer to "the model needs current/proprietary information" without any training. Full treatment in Domain 3.
- **Agentic AI applications.** Automating multi-step work that requires acting on systems, not just producing text: resolving a support ticket end to end, processing a claim, orchestrating a workflow across several APIs.

---

## 5. AWS managed AI/ML services — capabilities

The objective names six. Know what each **does**, what it **inputs/outputs**, and its **signature feature**.

### Amazon SageMaker AI
The **build-your-own-model** platform. Full lifecycle: label → prepare → train → tune → deploy → monitor. Use it when a pre-trained AI service doesn't fit your problem and you need a custom model on your own data.

- Signature: end-to-end custom ML, you control algorithm and training data.
- Notable features: JumpStart (pre-built models/solutions), Data Wrangler, Feature Store, Model Cards, Clarify (bias + explainability), Model Monitor, Ground Truth (labeling), Canvas (no-code ML).
- **Highest effort, highest control.**

### Amazon Comprehend — NLP
Extracts meaning from text. Input text → sentiment, entities, key phrases, dominant language, syntax, topic modeling, and **PII detection/redaction**. Comprehend Medical for clinical text.
- Cue: "analyze customer feedback/reviews", "detect PII in text", "extract entities".

### Amazon Transcribe — speech-to-text
Audio/video → text transcript. Speaker diarization, custom vocabulary, automatic language ID, real-time streaming, **PII redaction**. Transcribe Medical for clinical dictation.
- Cue: "transcribe call center recordings", "generate subtitles/captions".

### Amazon Translate — machine translation
Neural translation across languages. Real-time and batch, custom terminology.
- Cue: "localize content", "translate between languages".

### Amazon Polly — text-to-speech
Text → lifelike speech audio. Neural TTS voices, SSML control, Speech Marks, Brand Voice.
- Cue: "read content aloud", "voice response", "audio version of articles".

### Amazon Lex — conversational interfaces
Builds chatbots and voice bots. Uses **intents** (what the user wants), **utterances** (how they say it), and **slots** (parameters to collect). Same tech as Alexa. Integrates with Lambda for fulfillment and with Connect for contact centers.
- Cue: "build a chatbot", "automate IVR", "book an appointment by conversation".

> **Classic pipeline question:** transcribe a call, analyze how the customer felt, and reply in another language by voice →
> **Transcribe** (audio→text) → **Comprehend** (sentiment) → **Translate** (language) → **Polly** (text→audio).

### Also worth knowing (in scope elsewhere)
| Service | Capability |
|---|---|
| **Amazon Rekognition** | Image/video analysis: objects, faces, **content moderation**, text-in-image. **Its moderation covers creative content too — drawings, paintings, cartoons, animations** — and supports **custom adapters** for use-case-specific moderation. Accessed directly by API → **least operational overhead** |
| **Amazon Textract** | OCR++: text, forms, tables from documents |
| **Amazon Personalize** | **Real-time personalized recommendations** — the same tech behind Amazon.com's. Fully managed, API-accessible, no ML expertise. Feed it **users, items, and interactions**. → [full notes](../../services/personalize.md) |
| **Amazon Bedrock** | Managed access to foundation models via API |
| **Amazon Nova** | AWS's own foundation model family, served through Bedrock |
| **AWS Transform** | Agentic service for modernizing and migrating legacy workloads |

#### Amazon Nova — AWS's own FM family
Worth knowing by name, since it's on the in-scope list:

| Model | Modality | Positioning |
|---|---|---|
| **Nova Micro** | Text only | Lowest latency, lowest cost |
| **Nova Lite** | Multimodal (text, image, video in → text out) | Very low cost, fast |
| **Nova Pro** | Multimodal | Best balance of accuracy, speed, and cost |
| **Nova Premier** | Multimodal | Most capable; also the **teacher model for distillation** |
| **Nova Canvas** | Image **generation** | Includes watermarking and content moderation |
| **Nova Reel** | Video **generation** | Includes watermarking and content moderation |

> Pattern to remember: Micro → Lite → Pro → Premier ascends in capability and cost. Canvas = images, Reel = video.

### 📄 Document services — which one actually does what

**These four services all sound document-ish, and the distinction between them is frequently tested.** Read what the task *is* — extract, find, summarize, or chart:

| Service | What it does with documents | What it **cannot** do |
|---|---|---|
| **Amazon Textract** | **EXTRACTS** text, forms, and tables from documents (OCR++) | **Cannot summarize or answer questions.** It gives you the text, nothing more |
| **Amazon Kendra** | **FINDS/SEARCHES** — intelligent search that indexes documents and returns the relevant ones via semantic search | **Cannot summarize text** |
| **Amazon Q Business** | **SUMMARIZES and ANSWERS QUESTIONS** — a fully managed generative-AI assistant. Reads PDFs directly from S3. **Natural-language interface, no coding required** | — (this is the generative one) |
| **Amazon Quick Suite** (formerly QuickSight) | **BI dashboards and reports**, primarily on **structured** data | **Cannot extract text from PDFs** to summarize or answer questions |

> **The decision cue:** *"summarize the documents and answer questions about them"* + *"no coding experience"* → **Amazon Q Business**. Textract only extracts, Kendra only finds, Quick Suite only charts structured data.

#### Amazon Q Business
A **fully managed, generative-AI assistant** for enterprise content.

- Summarizes documents and answers questions grounded in your content.
- **Directly analyzes PDFs stored in Amazon S3** — no separate extraction step needed.
- **Natural-language interface requiring no coding experience** — the standard cue for a non-technical user.
- Connects to enterprise data sources with permission-aware access.

#### 🎓 AWS learning & experimentation services

**Neither of these is on the published in-scope services list — yet both are worth knowing.** They're how AWS teaches AI/ML hands-on, and the exam asks which one suits which kind of learner.

| Service | Teaches | How | Cue |
|---|---|---|---|
| **AWS DeepRacer** | **Reinforcement learning** | Hands-on with fully autonomous **1/18th scale race cars** and a **3D racing simulator**. Train and evaluate RL models in an interactive virtual environment | "**teach reinforcement learning**", "interactive and virtual environment", "hands-on for new ML practitioners" |
| **PartyRock** (an Amazon Bedrock Playground) | **Generative AI and prompt engineering** | Build and experiment with GenAI apps **without coding**. Interactive, but **GenAI — not RL** | "experiment with generative AI", "prompt engineering without coding" |
| **Amazon Q Developer** | — (not a teaching service) | An **AI-powered coding companion** giving code suggestions and recommendations | "code suggestions", "coding assistant" |

> 🚨 **The trap.** Both DeepRacer and PartyRock are **interactive learning environments**, so "interactive and virtual environment" alone doesn't separate them. **The deciding phrase is "reinforcement learning."**
> - **Reinforcement learning** → **DeepRacer** (race cars, 3D simulator)
> - **Generative AI / prompt engineering** → **PartyRock**
>
> **PartyRock is the common wrong pick.** It *is* the right answer to *"let non-coders experiment with generative AI"* — just not to *"teach reinforcement learning."*

> **🔗 Connects to your RL work:** DeepRacer is the physical embodiment of the RL loop — the car is the **agent**, the track is the **environment**, and the lap time is the **reward**. If you can picture that, both the RL definition and the DeepRacer answer follow.

> ⚠️ **Another gap in the published service list.** DeepRacer and PartyRock appear in **neither** the in-scope **nor** out-of-scope published lists — yet both are worth knowing. The same is true of **Amazon Q Business** and the **SageMaker sub-components**. The list says "non-exhaustive," and means it.

#### Amazon Kendra
**Intelligent search**, not generation. Indexes a document set and uses **semantic search** to return the most relevant documents for a query. Often used as the retrieval layer *feeding* a generative application — but on its own it **retrieves; it does not summarize or generate**.

> ⚠️ **Note on the published service list.** **Amazon Q is absent from the published AIF-C01 in-scope services list** — but it is still worth studying. The list states outright that it is *"non-exhaustive and subject to change,"* and Amazon Q Business is clearly under-represented in it. **Treat absence from that list as weak evidence, not proof of irrelevance.**

---

## 6. Traditional ML vs. foundation models

An explicit objective — AWS wants you to *not* reach for GenAI reflexively.

| Consideration | Traditional ML | Foundation models |
|---|---|---|
| **Task scope** | One narrow, well-defined task | Many tasks, general-purpose |
| **Data needed** | Labeled, task-specific dataset | Pre-trained already; little or none to start |
| **Output** | Prediction: label, score, number | Generated content: text, image, code |
| **Determinism** | Same input → same output | Non-deterministic by default |
| **Explainability** | Higher — especially linear models, decision trees | Low — opaque, hard to justify a specific output |
| **Cost profile** | Training cost up front, cheap inference | No training cost, **per-token inference cost** |
| **Latency** | Typically milliseconds | Higher — often hundreds of ms to seconds |
| **Regulatory fit** | Better where decisions must be justified | Harder to defend for regulated decisions |

**Choose traditional ML when:**
- You need **explainability** for regulators (credit decisions, insurance underwriting, medical diagnosis support).
- You need **deterministic, reproducible, auditable** outputs.
- The task is narrow, structured/tabular, and you have labeled data.
- You need very low latency or very low per-prediction cost at high volume.

**Choose a foundation model when:**
- The task involves **generating** content, not classifying it.
- You have little or no labeled training data.
- You need broad capability, or several language tasks at once.
- You need speed to market without a training project.
- The task is open-ended/conversational.

> **Exam heuristic:** the words **regulatory**, **explainability**, **audit**, or **must justify the decision** point at traditional ML. The words **generate**, **summarize**, **draft**, **converse**, or **no training data** point at foundation models.

---

## Exam traps for this task

1. **"A specific outcome is needed instead of a prediction"** → don't use ML. Highest-yield trap in this task.
2. **Classification vs. regression** — discrete label vs. continuous number.
3. **Classification vs. clustering** — labels already defined vs. groups to be discovered.
4. **Comprehend vs. Transcribe vs. Translate vs. Polly** — text analysis / speech→text / language→language / text→speech. Trace the input and output modality of the question.
5. **Textract vs. Rekognition for text** — Rekognition detects text *in images* (signs, labels); **Textract** extracts structured text, forms, and tables from *documents*.
6. **Lex vs. Bedrock for a chatbot** — Lex for structured intent/slot task bots; Bedrock/GenAI for open-ended conversation. Question wording about "intents and slots" means Lex.
7. **Don't default to GenAI.** If explainability or regulatory justification is mentioned, traditional ML is the answer.
8. **SageMaker AI = build custom.** If a pre-trained managed service already solves it, the managed service is the better answer (less undifferentiated heavy lifting).
9. 🚨 **"LEAST operational overhead" + a purpose-built AWS service exists → pick the purpose-built service, NOT Bedrock.** Bedrock is general, so it's tempting for anything — and that generality is exactly why it *loses* overhead questions. Building a recommender on an FM needs customization and probably RAG; **Amazon Personalize** already is one. Full pattern table: [services/personalize.md](../../services/personalize.md).
10. 🚨 **Content moderation of images — including cartoons, drawings, paintings, animations — is Amazon Rekognition.** **SageMaker Canvas is a trap here:** Canvas builds ML models with no code, but **it has no content-moderation model to offer**, and building one adds overhead. Fine-tuning Titan in Bedrock needs a **labeled dataset**. Rekognition is fully managed, API-accessible, and already does it. Rekognition also supports **custom adapters** for use-case-specific moderation.
