# Domain 1 — Flashcards

Cover the right column and work down. Anything you miss twice, go back to the source note.

---

## Task 1.1 — Concepts and terminology

| Prompt | Answer |
|---|---|
| Order the nesting: AI, deep learning, GenAI, ML | AI ⊃ ML ⊃ Deep Learning ⊃ GenAI |
| Difference between an algorithm and a model | Algorithm = the learning procedure; model = the trained artifact (weights) it produces |
| Parameter vs. hyperparameter | Parameter = learned by the model during training (weights). Hyperparameter = set by you *before* training (learning rate, epochs, batch size, tree depth) |
| What is a feature | An input variable the model learns from — a column in tabular data |
| What is a label / target | The correct answer being predicted — the output variable |
| What is an epoch | One complete pass through the entire training dataset. Too few → underfit; too many → overfit |
| Training vs. inferencing | Training = learning parameters from data (expensive, periodic). Inferencing = making predictions on new data (continuous, usually the bigger lifetime cost) |
| What is deep learning | ML using multi-layer neural networks; learns features automatically; excels on unstructured data |
| Is a rule-based if/then system AI? | Yes — it's AI, but not ML (nothing is learned from data) |
| All LLMs are FMs — true or false? | True. All LLMs are FMs; not all FMs are LLMs (image/multimodal FMs exist) |
| Definition of a foundation model | Large model pre-trained on massive broad unlabeled data, adaptable to many downstream tasks |
| What makes AI "agentic" | Autonomy/planning, tool use, memory, and orchestration — it loops toward a goal instead of returning one response |
| Two meanings of "bias" on this exam | Fairness bias (unfair skew against a group) and statistical bias (error from an over-simple model) |
| Signature symptom of overfitting | High training accuracy, **low** accuracy on new/test data |
| **"Performs well on training data but fails to generalize to new data"** | **OVERfitting** — it OVER-learned the training data (memorized the test route) |
| **"Too simple to capture the underlying patterns in the data"** | **UNDERfitting** — it UNDER-learned. The word **"simple"** is the giveaway |
| The mnemonic | **OVER**fitting = **OVER**-learned (memorized). **UNDER**fitting = **UNDER**-learned (never got it) |
| If a question mentions **good training performance**, can it be underfitting? | **No** — underfitting is bad at *everything*, training data included |
| Which failure mode *looks* like excellence at first? | **Overfitting** — near-perfect on training data, right up until you test on something new |
| Definition: "the ability to understand how a model arrives at a prediction" | **Explainability** |
| Definition: "unfair prejudice or preference that favors or disfavors a person or group" | **Bias** |
| Definition: "impartial and just treatment without discrimination" | **Fairness** |
| **Overfitting = which bias/variance combination?** | **LOW bias + HIGH variance** — centred on average but scattered; it's fitting the **noise** |
| **Underfitting = which bias/variance combination?** | **HIGH bias + LOW variance** — consistently wrong in the same way |
| Which bias/variance quadrant is ideal? | **Low bias + low variance** = balanced |
| **Is "high bias, high variance" overfitting?** | **No.** AWS: it *"rarely happens during model training"* and indicates **neither** overfitting nor underfitting. It's a designed distractor |
| Why can't a high-bias model be overfitting? | Overfitting means fitting the training data **too well** — which requires **low** bias on that data. High bias means it isn't fitting closely at all |
| The dartboard: what do bias and variance mean? | **Bias** = how far the shot cluster sits **from the bullseye**. **Variance** = how **spread out** the shots are from each other |
| Overfitting vs. underfitting on both axes | **Exact opposites**: overfitting = low bias/high variance; underfitting = high bias/low variance. Recall one, mirror it for the other |
| Three fixes for overfitting | More/diverse data, regularization, early stopping (also: simplify the model, data augmentation) |
| **What is regularization?** | An optimization technique that **penalizes model complexity during training**, so the model generalizes instead of memorizing |
| 🔑 **Which way does the regularization parameter go for overfitting?** | **INCREASE it.** More regularization = **LESS** complexity = less overfitting. The inverse relationship is the tested bit |
| **Does adding more FEATURES fix overfitting?** | **No — it makes it WORSE.** More features = more complexity. More **DATA** helps; more **features** hurts |
| Does training for more epochs fix overfitting? | **No — worse.** An overfitting model just memorizes harder. The fix is **early stopping** |
| **What's the DIAGNOSTIC step when a model generalizes badly?** | **Perform a bias and variance tradeoff analysis** — it tells you whether it's high bias (underfitting) or high variance (overfitting) so you know what to adjust |
| **"Trained with custom libraries dependencies… performs well on training, poorly on new data"** → | **Bias and variance tradeoff analysis.** "Custom libraries dependencies" is a **RED HERRING** — the train/test gap is the only phrase that matters |
| 🎣 The red-herring technique | Find the **diagnostic** phrase, ignore the **decoration**. Ask: "which single phrase tells me what's actually wrong?" |
| Does better software packaging fix poor generalization? | **No.** It improves consistency and reproducibility — nothing to do with the model's ability to generalize |
| Does training on a **smaller** dataset fix overfitting? | **No — backwards.** Less data **increases bias** and **reduces** generalization. More data helps |
| Does MLOps/CI/CD automation fix overfitting? | **No.** Automation improves consistency and speed — it doesn't replace analysing bias and variance |
| 🚨 Why drill overfitting so hard? | It is tested from **four angles** — the definitions, the bias/variance pairing, the dartboard quadrant, and buried under a red herring |
| Supervised learning uses what data | **Labeled** data — inputs with known correct answers |
| **Is regression supervised or unsupervised?** | **SUPERVISED.** Regression is a *type of* supervised learning |
| **Name the parent learning type of each technique** | **Regression, classification, forecasting → SUPERVISED.** **Clustering, anomaly detection, dimensionality reduction → UNSUPERVISED** |
| 🔑 The one-line test for supervised learning | **Is there a TARGET VARIABLE being predicted FROM known features?** If yes → supervised, whether the output is a number or a category |
| Predicting electricity load from past load, temperature and cloud coverage → | **Supervised learning.** Target = electricity load; features = temperature, cloud coverage. It is regression, and **regression is supervised** |
| How do you spot that data is labeled? | **The thing you want to predict appears in the training dataset.** Target in the data = labels = supervised |
| 🪜 Why did "regression" not appear as an option? | The question asked at the **learning-type level** (supervised/unsupervised/reinforcement). You must **climb from technique → learning type** |
| Unsupervised learning uses what data | **Unlabeled** data — the model discovers structure itself |
| Reinforcement learning learns from what | Trial and error in an environment, guided by **rewards** and penalties |
| **"Trial and error" in a question means…** | **Reinforcement learning** — always. It signals there is *no dataset of correct answers*, so only learning from consequences is possible |
| Name six RL signal words | Trial and error, environment, rewards/penalties, agent, navigate, "learn effective strategies" (also policy, maximize a score) |
| Six RL vocabulary terms | Agent, environment, state, action, reward/penalty, policy |
| **Which AWS service TEACHES reinforcement learning?** | **AWS DeepRacer** — 1/18th scale autonomous race cars + a **3D racing simulator** |
| **What is PartyRock?** | **An Amazon Bedrock Playground** — experiment with **generative AI and prompt engineering without coding**. Interactive, but **GenAI, not RL** |
| DeepRacer vs. PartyRock | Both are interactive learning environments. **"Reinforcement learning" → DeepRacer. "Generative AI / prompt engineering" → PartyRock** |
| What is Amazon Q Developer? | An **AI-powered coding companion** — code suggestions and recommendations. Not a teaching service |
| 🔗 How does DeepRacer map to RL vocabulary? | The **car = agent**, the **track = environment**, the **lap time = reward** |
| ⚠️ Are DeepRacer and PartyRock on the in-scope services list? | **No — neither list mentions them, and both were tested.** Third confirmation that the published list is genuinely "non-exhaustive" |
| What is a policy in RL | The learned strategy — which action to take in which state |
| **What is transfer learning** | Take a model trained on one task and **adapt it to a related task**, reusing what it learned. The principle behind fine-tuning a pre-trained FM |
| Transfer learning vs. reinforcement learning | Transfer = start from an existing model, learn from a labeled dataset. RL = start from nothing, learn from rewards in an environment. **Transfer learning never answers a "trial and error" question** |
| Why isn't supervised learning the answer to "trial and error"? | Supervised needs labeled correct answers. Trial and error means no correct answers exist to copy |
| **How are FMs/LLMs pre-trained** | **UNLABELED training data + SELF-SUPERVISED learning.** Memorize as one unit |
| Why can't "labeled" and "self-supervised" go together? | **Contradiction.** *Self*-supervised means the model **makes its own labels** from the raw data. Human labels would make it plain supervised |
| Why can't "unlabeled" and "supervised" go together? | **Contradiction.** Supervised learning is *defined* by having labels |
| Pre-training vs. fine-tuning — data requirement | **Pre-training = unlabeled + self-supervised. Fine-tuning = labeled + supervised.** Opposite requirements, same model |
| Structured vs. unstructured data | Structured = fixed schema, rows/columns (databases, CSV). Unstructured = no predefined model (text, images, audio, video) |
| **Is a `.csv` structured or semi-structured?** | **Structured** — tabular, every row has the same columns |
| **Is `.json` structured or semi-structured?** | **Semi-structured** — self-describing keys and nesting, but no strict tabular schema. Records can differ |
| **Is `.xml` structured or semi-structured?** | **Semi-structured** — tagged and hierarchical, but not a rigid data model |
| Why do JSON and XML *feel* structured? | They have tags, nesting, and hierarchy — but "having some structure" ≠ "structured data". **Structured means a rigid predefined tabular schema** |
| Is a plain text file with metadata structured? | **No — unstructured.** Adding metadata doesn't create a data model |
| The one-question test for data type | "Does **every record have the same predefined fields in rows and columns**?" Yes → structured. Organized but varying/nested → semi-structured. No organization → unstructured |
| Name four semi-structured formats | JSON, XML, YAML, log files (also HTML, NoSQL documents, emails) |
| Name the four inference types | Real-time, serverless, asynchronous, batch |
| Which inference type for spiky, intermittent traffic | **Serverless** — scales to zero, no idle cost, but cold starts |
| Which inference type for 1 GB payloads / long processing | **Asynchronous** — queued, up to ~1 GB and ~1 hour |
| Which inference type for scoring a whole dataset nightly | **Batch** (batch transform) — offline job, no endpoint, results to S3 |
| Which inference type for consistent millisecond latency | **Real-time** — persistent endpoint, always on (you pay for idle) |
| Batch vs. asynchronous — the distinction | Batch = offline job over a whole dataset, no endpoint. Async = live endpoint with a queue, per-request |
| Two things serverless inference does NOT support | GPUs and Model Monitor (also VPC config, multi-model endpoints) |
| How do you eliminate serverless cold starts | **Provisioned Concurrency** — keeps capacity warm for predictable bursts |

---

## Task 1.2 — Practical use cases

| Prompt | Answer |
|---|---|
| When is ML the *wrong* choice | When a specific deterministic outcome is required instead of a prediction; simple stable rules; insufficient data; cost > benefit; 100% accuracy required |
| Predict a house price — which technique | **Regression** (continuous number) |
| Predict whether a customer churns — which technique | **Binary classification** (discrete label) |
| Segment customers into groups nobody has defined | **Clustering** (unsupervised) |
| Classification vs. clustering | Classification uses predefined labeled categories (supervised); clustering discovers groups (unsupervised) |
| Detect unusual/rare events | **Anomaly detection** |
| Predict next quarter's demand from time-ordered data | **Forecasting** (time-series) |
| Hourly readings over a month, predicting the next day → | **Forecasting algorithm** — the tell is *readings over time* + *predict the next N periods* |
| **Name the forecasting algorithm** | **DeepAR** — for **one-dimensional time series** data |
| **Name the anomaly detection algorithm** | **Random Cut Forest (RCF)** (also IP Insights for anomalous IP usage) |
| Name the clustering algorithm | **k-means** |
| Two algorithms for classification and regression | **XGBoost** and **Linear Learner** (also k-NN) |
| Algorithm for dimensionality reduction | **PCA** |
| Is forecasting just regression? | Treat it as **its own family** — time-ordered data predicted forward in time. DeepAR, not XGBoost |
| Why isn't classification right for a temperature forecast? | Classification outputs a **category**; temperature is a **continuous value over time** |
| Why isn't anomaly detection right for a forecast? | RCF finds **outliers**; the task is to **predict future values**, not spot weird ones |
| Amazon Comprehend does what | NLP on text: sentiment, entities, key phrases, language detection, **PII detection** |
| Amazon Transcribe does what | Speech-to-text (audio → transcript) |
| Amazon Translate does what | Machine translation between languages |
| Amazon Polly does what | Text-to-speech (text → lifelike audio) |
| Amazon Lex does what | Builds chatbots/voice bots using intents, utterances, and slots |
| Amazon Rekognition does what | Image and video analysis: objects, faces, moderation, text in images |
| **"Moderate cartoons, drawings, paintings, animations — LEAST overhead"** → | **Amazon Rekognition content moderation** — it covers creative content types and is API-accessible |
| **Why is SageMaker Canvas wrong for content moderation?** | Canvas builds models with no code, but **has no content-moderation model to offer** — building one adds overhead |
| Why is fine-tuning Titan wrong for content moderation? | It requires a **labeled dataset** → additional operational overhead |
| What are Rekognition **custom adapters**? | Let you tune Rekognition moderation for **use-case-specific** moderation needs, still managed |
| Amazon Textract does what | Extracts text, forms, and tables from **documents** (OCR++) |
| Amazon Personalize does what | **Real-time personalized recommendations** — fully managed, API-accessible, no ML expertise. Same tech as Amazon.com's recommendations |
| What three datasets does Personalize need? | **Users, Items, and Interactions** (clicks, views, purchases) |
| "Increase click-through by showing each subscriber the most relevant news, LEAST overhead" → | **Amazon Personalize** — purpose-built for it |
| **Why is Bedrock wrong for a "least overhead" recommendation question?** | You *could* build a recommender on an FM, but it needs **model customization and probably RAG** — that's *more* overhead. Personalize already is a recommender |
| 🚨 **The purpose-built rule** | **"LEAST operational overhead" + a purpose-built AWS service exists → pick the purpose-built service, not Bedrock.** Bedrock's generality is exactly why it loses overhead questions |
| Can Amazon Comprehend make recommendations? | **No** — it does NLP (entities, key phrases, sentiment, language) |
| Kendra vs. Personalize | Kendra = **search** (find relevant docs for a *query*). Personalize = **recommendations** (relevant items for a *user*) |
| Rekognition vs. Textract for reading text | Rekognition = text *in images* (signs, labels). Textract = structured text/forms/tables from *documents* |
| Pipeline: analyze a recorded call and reply by voice in Spanish | Transcribe → Comprehend → Translate → Polly |
| Amazon SageMaker AI is for what | Building, training, tuning, deploying, and monitoring **custom** ML models — full lifecycle |
| **How do Canvas models reach Studio users?** | **Canvas pushes models to SageMaker Model Registry; Studio users access the same registry.** Least operational overhead |
| **Can you put your own model into SageMaker JumpStart?** | **No.** JumpStart is a consume-only hub of pre-existing open-source/pre-trained models |
| Model Registry vs. ECR | Registry = **models**. ECR = **container images** |
| Why not build a model registry on DynamoDB? | You'd design, configure, and scale the tables yourself — **more** operational overhead than the managed Model Registry |
| Studio vs. Canvas | Studio = **code**, data scientists (IDE). Canvas = **no code**, business analysts |
| Model Cards vs. Amazon A2I | Model Cards = **system-level documentation for audits**. A2I = **human review of individual predictions** |
| Clarify vs. Model Monitor | Clarify = bias **and explainability** (SHAP). Model Monitor = **production drift** detection |
| Which SageMaker component explains predictions? | **Clarify** (SHAP) — not Model Monitor, not Ground Truth, not Studio |
| Where is the full SageMaker component map? | [services/sagemaker-ai.md](../../services/sagemaker-ai.md) — all components, 5 flows, 10 confused pairs |
| Pick traditional ML over an FM when… | Explainability/regulatory justification needed, deterministic auditable output, narrow tabular task with labeled data, very low latency/cost at volume |
| Pick an FM over traditional ML when… | Generating content, no labeled training data, broad/open-ended tasks, speed to market, conversational |
| Which words in a question point to traditional ML | "Regulatory", "explainability", "audit", "must justify the decision" |
| What is a knowledge base (RAG) in one line | Your documents stored as embeddings; relevant passages retrieved at query time and inserted into the prompt, so the FM answers grounded in your data with citations |
| The six Amazon Nova models | Micro (text only), Lite / Pro / Premier (multimodal, ascending capability), Canvas (image generation), Reel (video generation) |
| Which Nova model is the distillation teacher | **Nova Premier** — the most capable |
| Is Amazon Q on the AIF-C01 in-scope list? | **Not on the published list — but it IS tested.** Practice material treats Amazon Q Business as a correct answer. The list is explicitly "non-exhaustive." **Study it** |
| **"Summarize PDFs in S3 and answer questions, user has no coding experience"** → | **Amazon Q Business** — managed generative-AI assistant, reads S3 PDFs directly, natural-language interface |
| What does **Amazon Kendra** do? | **Intelligent search** — indexes documents, returns relevant ones via **semantic search**. It **finds; it does not summarize or generate** |
| Textract vs. Kendra vs. Q Business | Textract **EXTRACTS** text. Kendra **FINDS** documents. Q Business **SUMMARIZES and ANSWERS** |
| Can Amazon Quick Suite summarize PDFs? | **No** — it's BI on **structured** data. It can't extract text from PDFs |
| Which cue points to a no-code managed assistant? | "does not have coding experience" → **Amazon Q Business** |

---

## Task 1.3 — Development lifecycle

| Prompt | Answer |
|---|---|
| The three data splits and their jobs | Training (learn parameters), validation (tune hyperparameters during development), test (final unbiased evaluation, used once) |
| Why not evaluate on the training set | High training accuracy proves nothing — only unseen data reveals overfitting |
| Three sources of foundation models | Pre-trained via managed API (e.g. Bedrock), open-source pre-trained weights, custom pre-training from scratch |
| Cheapest → most expensive FM customization | Prompt engineering → RAG → fine-tuning → continued pre-training → pre-training from scratch |
| Managed API vs. self-hosted — the cue | "Minimal operational overhead" → managed API. "Full control of weights / must run in our VPC" → self-hosted |
| Amazon Bedrock is what | Fully managed serverless access to foundation models from multiple providers via one API |
| Amazon Quick is what | The Oct 2025 evolution of QuickSight — GenAI-powered BI and productivity suite (Quick Sight, Quick Research, Quick Flows, Quick Automate, Quick Index). **Not** a training service |
| Kiro is what | An agentic IDE built on Code OSS using **spec-driven development** — a developer tool, not a data/hosting service |
| Strands Agents is what | Open-source SDK for building AI agents |
| SageMaker JumpStart is what | Hub of pre-trained models and prebuilt solution templates for fast deployment |
| MLOps in one line | DevOps principles applied to ML, extended to version and monitor **data and models**, not just code |
| Why ML needs more than DevOps | Code + data + model all change independently; a model degrades with no code change at all |
| Data drift vs. concept drift | Data drift = the input distribution changes. Concept drift = the input→output relationship changes |
| Response to detected drift | Retrain on fresh data (SageMaker **Model Monitor** detects it) |
| Confusion matrix: what is a false negative | The model predicted negative but the truth was positive — a **miss** |
| Precision formula and meaning | TP / (TP + FP) — of what I flagged, how much was real |
| Recall formula and meaning | TP / (TP + FN) — of what was real, how much I caught |
| F1 score is what | Harmonic mean of precision and recall — use when you need balance or classes are imbalanced |
| Optimize recall when… | False negatives are costly: cancer screening, fraud detection, threat detection |
| Optimize precision when… | False positives are costly: spam filtering, blocking legitimate transactions |
| The accuracy trap | On imbalanced data (99.9% legitimate), always predicting the majority class scores 99.9% accuracy and catches nothing. Use precision/recall/F1/AUC |
| AUC-ROC of 0.5 means | Random guessing (1.0 = perfect) |
| 1,000 transactions, 100 fraudulent; model flags 80, of which 60 are real fraud — precision and recall? | Precision = 60/80 = **75%**. Recall = 60/100 = **60%**. Accuracy = 940/1000 = 94% and hides the miss rate |
| What is specificity | True negative rate = TN / (TN + FP) — recall's mirror image on the negative class |
| **Five regression metrics** | **MAE, MAPE, MSE, RMSE, R²** |
| "Predict the annual sales volume" — which metrics? | **Regression** → **MAE and MAPE** (also MSE/RMSE/R²). Accuracy, precision, and recall are **classification-only** and are pure distractors here |
| What is MAPE and why use it | Mean Absolute **Percentage** Error — error as a %, so it's **scale-independent** and comparable across items of very different size. The natural forecasting metric |
| MAE vs. MAPE | Same idea, different unit: MAE in the target's own units ("off by 400 units"), MAPE as a percentage ("off by 8%") |
| When does MAPE break down | When actual values are zero or near zero (division by zero) |
| Can you compute accuracy on a regression model? | **No.** Accuracy/precision/recall/F1/AUC require discrete class labels — a continuous prediction has none |
| The metric decision rule | Ask what **shape the output is**: a number → regression metrics; a label → classification metrics; generated text → ROUGE/BLEU/BERTScore |
| Metrics for clustering | Silhouette score, Davies-Bouldin index, inertia (WCSS) — no ground-truth labels needed |
| Metrics for ranking/recommendation | Precision@k, Recall@k, NDCG, MRR, hit rate |
| RMSE advantage over MSE | Same units as the target, so it's interpretable |
| MAE vs. RMSE — the difference in one line | MAE treats every error equally; RMSE punishes large errors disproportionately |
| Ten deliveries 6 min late vs. nine on time + one 60 min late — MAE and RMSE? | **MAE is 6 for both.** RMSE is 6 vs. **19** — RMSE exposes the disaster, MAE hides it |
| "Large errors are especially costly" → which metric | **MSE / RMSE** |
| "Treat all errors equally / robust to outliers" → which metric | **MAE** |
| What does R² = 0 mean | The model is no better than always predicting the average |
| What does a **negative** R² mean | Worse than a constant horizontal line — worse than guessing the average every time |
| AWS's stated R² range | **1 to −1** (higher is better) |
| Is RMSE comparable across different datasets | No — it's scale-dependent |
| What is balanced accuracy | `0.5 × ((TP/P) + (TN/N))` — accuracy normalized for class size; the fix for the accuracy trap on imbalanced data |
| What is log loss (cross-entropy) | Scores the quality of predicted **probabilities**; heavily punishes being confidently wrong. 0 → ∞, lower better |
| What are F1macro / PrecisionMacro / RecallMacro | The per-class metric averaged across classes — used for **multiclass** problems |
| What is k-fold cross-validation | Split data into k folds; train on k−1 and validate on the held-out one; repeat k times and average the scores |
| Why use cross-validation | Reduces overfitting and selection bias, makes full use of limited data, gives a more reliable estimate than one lucky/unlucky split |
| Cost of cross-validation | Trains the model k times — AWS notes **~20%** average increase in training time |
| Autopilot HPO mode: when does it cross-validate, and with what k | Automatically for datasets of **≤50,000** training instances, with **k = 5** |
| Autopilot ensembling mode default split | **80% train / 20% validation** if you don't supply your own |
| Name five business metrics | ROI, cost per user/interaction, development cost, customer satisfaction (CSAT/NPS), conversion rate, ARPU, CLV, task completion rate |
| Model metrics vs. business metrics | "Is the model good?" vs. "Was the project worth it?" — the exam asks for both |

---

## Rapid-fire numbers

| Prompt | Answer |
|---|---|
| Domain 1 weighting | 20% (~10 of 50 scored questions) |
| Total scored questions on the exam | 50 (plus 15 unscored) |
| Passing scaled score | 700 out of 100–1,000 |
| Typical train/validation/test split | ~70–80% / ~10–15% / ~10–15% |
| **Max processing time — all four inference types** | **Real-time: 60 s. Serverless: 60 s. Asynchronous: 1 hour (≤1 GB). Batch: hours to days** |
| **"Near real-time, images take up to 3 minutes to process"** → | **Asynchronous inference.** 3 min > 60 s kills real-time *and* serverless; 3 min ≪ days kills batch |
| Two phrases that always mean asynchronous | **"near real-time"** and **"requests can be queued"** — async is the only type that queues to a live endpoint |
| Real-time endpoint payload limit | ~6 MB |
