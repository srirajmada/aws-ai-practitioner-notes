# Task 1.1 — Explain basic AI concepts and terminologies

> Domain 1 (20%). This task is pure vocabulary. Expect definition-matching and "which term describes X" questions. Cheap marks — do not lose them.

---

## 1. The nesting doll: AI ⊃ ML ⊃ DL ⊃ GenAI

The single most-tested relationship in this task. Each is a **subset** of the one before it.

```
┌─ Artificial Intelligence ──────────────────────────────┐
│  Any technique that makes machines mimic human         │
│  intelligence — including plain if/then rule engines.  │
│                                                        │
│  ┌─ Machine Learning ───────────────────────────────┐  │
│  │  Systems that LEARN patterns from data instead   │  │
│  │  of being explicitly programmed.                 │  │
│  │                                                  │  │
│  │  ┌─ Deep Learning ────────────────────────────┐  │  │
│  │  │  ML using multi-layer neural networks.     │  │  │
│  │  │  Learns features automatically.            │  │  │
│  │  │                                            │  │  │
│  │  │  ┌─ Generative AI ──────────────────────┐  │  │  │
│  │  │  │  Deep learning models that CREATE    │  │  │  │
│  │  │  │  new content. Powered by FMs/LLMs.   │  │  │  │
│  │  │  └──────────────────────────────────────┘  │  │  │
│  │  └────────────────────────────────────────────┘  │  │
│  └──────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────┘

  Agentic AI = an APPLICATION PATTERN layered on top (usually on GenAI),
  not another ring in the doll. It adds goals, tools, memory, and autonomy.
```

**Key distinction to memorize:** traditional ML is mostly **discriminative** (it predicts a label or a number about existing data — "is this spam?"). Generative AI is **generative** (it produces new artifacts — "write me an email").

---

## 2. Core term glossary

### The basics

| Term | Definition | Exam cue |
|---|---|---|
| **Artificial Intelligence (AI)** | Broadest field: machines performing tasks that normally require human intelligence. Includes rule-based systems with no learning at all. | The outermost umbrella |
| **Machine Learning (ML)** | Systems that improve at a task by learning statistical patterns from data, rather than from hand-written rules. | "Learns from data" |
| **Deep Learning (DL)** | ML using neural networks with many hidden layers. Excels at unstructured data (images, audio, text). Needs lots of data + GPUs. | "Many layers", "automatic feature extraction" |
| **Neural network** | Model of interconnected layers of nodes ("neurons") with weights, loosely inspired by the brain. Input layer → hidden layers → output layer. | Building block of DL |
| **Model** | The trained *artifact* — the learned parameters/weights. The output of training. | The "what" |
| **Algorithm** | The *procedure* used to learn the model (e.g. linear regression, XGBoost, k-means). | The "how" |

> **Trap:** *algorithm ≠ model.* The algorithm is the recipe; the model is the cake. Questions test this pair directly.

### The data vocabulary

| Term | Definition | Exam cue |
|---|---|---|
| **Feature** | An input variable the model learns from — a column in tabular data. | "Independent variable", "predictor", "input" |
| **Label / target** | The correct answer being predicted — the output variable. | "Dependent variable", "ground truth" |
| **Feature engineering** | Creating, transforming, and selecting features to improve model quality. | Often the highest-leverage stage of the pipeline |
| **Parameter** | A value the model **learns by itself** during training (weights, biases). You do not set these. | "Learned during training" |
| **Weights** | The **numbers inside the model** that determine how strongly each input influences the next layer. **The weights ARE the model** — see below | "alters the model's internal weights" |
| **Hyperparameter** | A value **you set before** training that controls *how* learning happens: learning rate, number of epochs, batch size, tree depth, number of layers. | "Configured before training", "tuning" |
| **Epoch** | One complete pass through the entire training dataset. | Too few → underfit; too many → overfit |

> **Trap:** *parameter vs. hyperparameter.* Parameters are learned by the model; hyperparameters are chosen by you and tuned (SageMaker Automatic Model Tuning tunes hyperparameters, never parameters).

#### 🎛️ What "model weights" actually means

A neural network is layers of connected nodes. **Every connection carries a number — a weight — that says how strongly one node's signal influences the next.** Training adjusts those numbers until the outputs are good.

**The crucial idea: the weights ARE the model.** Everything the model "knows" is encoded in those numbers, and nothing else. Copy the weights to another machine and you've copied the model. When a model is described as **"70 billion parameters,"** that's 70 billion of these learned numbers.

> **🎛️ Analogy — a sound engineer's mixing desk.** Picture a mixing desk with billions of tiny sliders, each controlling how much one signal feeds into the next channel. **Training** is the engineer nudging every slider, over and over, until the mix sounds right. **The final slider positions are the model** — write them down and anyone can reproduce that exact sound.
>
> Now the distinction that matters: **changing the sliders** is *fine-tuning*. **Handing the band a different sheet of music, or asking them to play the same piece more loudly** is *prompt engineering, RAG, and inference parameters* — the desk hasn't moved at all.

**Which techniques change the weights, and which don't:**

| ✅ **CHANGES the weights** (training) | ❌ **Does NOT change the weights** (inference-time) |
|---|---|
| **Pre-training** (creates them from random) | **Prompt engineering** (zero/one/few-shot, chain-of-thought) |
| **Continued pre-training** | **RAG** — adds retrieved context to the prompt |
| **Fine-tuning** (instruction-based, domain adaptation) | **Inference parameters** — temperature, Top K, Top P, max tokens |
| **RLHF** | **Guardrails** — filters input/output |
| **Model distillation** (trains the student) | **Agents / tool use**, **context engineering**, **prompt caching** |

> **The consequence worth remembering:** anything on the **right** is **temporary and per-request** — change the prompt and the behaviour changes, with no lasting effect. Anything on the **left** produces a **permanent new model artifact**. That's why fine-tuning yields a *custom model* and RAG doesn't.

> **One honest nuance:** **PEFT/LoRA** freezes the original weights and trains a **small set of added adapter weights** instead. It's still a training method producing a new artifact — it just doesn't rewrite the base model. Not something a foundational exam will split hairs over, but worth knowing the claim "fine-tuning changes weights" has this asterisk.

### Training vs. inferencing

| Term | Definition |
|---|---|
| **Training** | Feeding data through the algorithm to learn parameters. Expensive, slow, compute-heavy, done periodically/offline. |
| **Inferencing** | Using the trained model to produce predictions on new, unseen data. Cheaper per call, but runs continuously in production — usually the dominant lifetime cost. |

### Feature-extraction terms

| Term | Definition | Real-world examples |
|---|---|---|
| **Computer vision (CV)** | AI that interprets images and video — classification, object detection, facial analysis, OCR. | Amazon Rekognition, Amazon Textract |
| **Natural language processing (NLP)** | AI that understands/manipulates human language — sentiment, entities, translation, summarization. | Amazon Comprehend, Amazon Translate |

### Quality and fit terms

| Term | Definition | Exam cue |
|---|---|---|
| **Bias** (fairness sense) | Systematic, unfair skew in outputs that disadvantages a group. Comes from unrepresentative training data or flawed labels. | "Disparate outcomes for a demographic group" |
| **Bias** (statistical sense) | Error from an over-simplified model — the underfitting half of the bias–variance tradeoff. | "Model too simple" |
| **Fairness** | The goal/property of producing equitable outcomes across groups. Bias is the defect; fairness is the objective. | |
| **Variance** | Sensitivity to fluctuations in the training data — the overfitting half of the tradeoff. | "Model too complex" |
| **Fit** | How well the model captures the true pattern in the data. | See table below |

> **Bias has two meanings on this exam.** Domain 4 uses the fairness sense; Domain 1 metric questions use the statistical sense. Read the surrounding words to tell which.

#### Underfitting vs. overfitting vs. good fit

| | Underfitting | Good fit | Overfitting |
|---|---|---|---|
| **Cause** | Model too simple; too few features; under-trained | Balanced | Model too complex; trained too long; too little data |
| **Training accuracy** | Low | High | Very high (near perfect) |
| **Test/new-data accuracy** | Low | High | **Low** |
| **Bias / variance** | High bias, low variance | Balanced | Low bias, **high variance** |
| **Fixes** | Bigger model, more features, train longer | — | More/diverse training data, regularization, early stopping, simplify model, data augmentation |

> **The signature of overfitting is the *gap*:** excellent on training data, poor on unseen data. That phrasing in a question is almost always pointing at overfitting.

### 🎯 The bias–variance dartboard

**The single clearest way to see this.** Each ○ is one prediction; ⊕ is the bullseye (the true value). **Bias** = how far the cluster sits *from* the bullseye. **Variance** = how *spread out* the shots are from each other.

```
                      LOW VARIANCE                 HIGH VARIANCE
                  (shots tightly grouped)        (shots scattered wide)
                ┌───────────────────────┬───────────────────────┐
                │                       │                       │
                │    ○ ○ ○              │      ○      ○         │
   HIGH BIAS    │   ○ ○ ○      ⊕        │   ○      ⊕       ○    │
  (grouped away │    ○ ○                │        ○     ○        │
   from centre) │                       │      ○           ○    │
                │  ► UNDERFITTING       │  ► WORST CASE         │
                │    model too simple   │    (rare in practice) │
                ├───────────────────────┼───────────────────────┤
                │                       │       ○    ○    ○     │
                │         ○ ○ ○         │    ○      ○           │
   LOW BIAS     │        ○ ⊕ ○ ○        │       ○  ⊕   ○   ○    │
  (centred on   │         ○ ○           │    ○        ○         │
   the centre)  │                       │        ○       ○      │
                │  ► BALANCED ✓         │  ► OVERFITTING        │
                │    the goal           │    model too complex  │
                └───────────────────────┴───────────────────────┘
```

| Quadrant | Result | Why |
|---|---|---|
| **High bias, low variance** | **UNDERFITTING** | Consistently wrong in the same way. The model makes erroneous assumptions and ignores the data's real detail |
| **Low bias, high variance** | **OVERFITTING** ⚠️ | Centred on the right answer *on average*, but wildly inconsistent — it's **paying attention to noise** in the training data |
| **Low bias, low variance** | **BALANCED** ✓ | The ideal outcome |
| **High bias, high variance** | Worst case | AWS: *"this pattern will rarely happen during model training"* and indicates **neither** overfitting nor underfitting. **A tempting-looking distractor** |

> **🔑 The line that answers the question directly:**
> **OVERFITTING = LOW bias + HIGH variance.**
> **UNDERFITTING = HIGH bias + LOW variance.**
>
> They are **opposites on both axes**. If you can recall just one, the other is its mirror image.

> **Why "high bias, high variance" is wrong even though it sounds bad:** it *is* bad — but it's not a description of overfitting. Overfitting means the model fits the training data **too well**, which by definition means **low** bias on that data. A model with high bias isn't fitting the training data closely at all, so it cannot be overfitting it.

> **Tie it to the 🚗 analogy below:** Marya (memorized the test route) has **low bias** — on her practice route she's dead-on the bullseye — but **high variance**: move her to any other road and the shots scatter everywhere. That's overfitting. Rai (two lessons) is **consistently** bad everywhere: high bias, low variance.

### 🚗 Analogy — Learning to drive vs. memorizing the test route

Three people take their driving test.

**Rai had two lessons.** He can't really drive. He stalls in the car park, he stalls on the practice runs, he stalls on the test. He's bad *everywhere* — during practice and on the day. Nobody is surprised. His preparation was simply **too thin to capture what driving involves**.

That's **underfitting**. The model is too simple. It performs badly on the training data *and* on new data. If someone tells you "the model scores poorly on everything," it never learned in the first place.

**Marya took the clever shortcut.** She found out the exact test route and practised only that: *turn left at the red postbox, third gear outside the pub, the mirror check happens just before the school.* Drive that route and she is flawless — better than anyone. Then on test day the examiner says "let's go the other way today," and she falls apart. She doesn't know when to check her mirrors; she knew to check them *at the school*. She never learned to drive. **She learned the route.**

That's **overfitting**. Near-perfect on the data it trained on, and it collapses the moment anything is slightly different. Priya memorized instead of understanding — and note that on her practice route she looked like the *best* driver of the three. **That's the trap: overfitting looks like excellence right up until you test it on something new.** **Sam actually learned to drive.** Roundabouts, junctions, wet roads, other people's stupidity. Not flawless on any single route, but competent on all of them, including roads he's never seen.

That's a **good fit** — real generalization.

| On the road | In machine learning |
|---|---|
| Two lessons — stalls everywhere, practice and test alike | **Underfitting** — too simple, bad on training *and* new data |
| Memorized the test route — flawless on it, lost anywhere else | **Overfitting** — great on training data, fails to generalize |
| Learned to drive — handles any route | **Good fit** |
| Practising the route | **Training data** |
| The examiner's surprise route | **Test data / new data** |
| Priya's perfect practice runs vs. her failed test | **The train/test gap — the signature of overfitting** |

### 🎚️ Regularization — the complexity dial

**What it is:** an optimization technique that **penalizes model complexity during training**. It adds a penalty that grows as the model's weights get large, discouraging it from fitting the training data too precisely. The result is a **simpler model that generalizes better**.

**The regularization parameter** controls *how much* penalty is applied — and the exam tests the direction:

| Change | Effect on complexity | Effect on fit |
|---|---|---|
| ⬆️ **INCREASE the regularization parameter** | **DECREASES** model complexity — constrains the model | **Reduces OVERFITTING** ✅ (too far → underfitting) |
| ⬇️ **DECREASE the regularization parameter** | **INCREASES** model complexity | **Reduces UNDERFITTING** (too far → overfitting) |

> 🔑 **Memorize the inverse relationship: MORE regularization = LESS complexity.** It reads backwards at first — a bigger number makes a *simpler* model — which is exactly why it's tested.
>
> **Overfitting → INCREASE regularization.** That's the answer.

**Common types** (worth recognizing, not memorizing for a foundational exam): **L1/Lasso** (can shrink weights to zero, effectively dropping features), **L2/Ridge** (shrinks weights toward zero), **dropout** (randomly disables nodes during neural-network training).

#### ⚠️ More DATA helps overfitting. More FEATURES makes it worse.

A genuinely easy confusion, and the exam exploits it:

| Action | Effect on overfitting |
|---|---|
| More training **DATA** (more rows/examples) | ✅ **HELPS** — more examples to generalize from |
| More **FEATURES** (more columns/variables) | ❌ **HURTS** — increases complexity, so the model memorizes rather than generalizes |
| More **EPOCHS** (train longer) | ❌ **HURTS** — an already-overfitting model just memorizes harder. The fix is **early stopping** |
| **Increase** regularization | ✅ **HELPS** — constrains complexity |
| **Decrease** regularization | ❌ **HURTS** — the opposite of what's needed |

> **The unifying principle:** overfitting means the model is **too complex for the data it has**. So **anything that adds complexity makes it worse** (more features, more epochs, less regularization) and **anything that reduces complexity or adds data makes it better** (more regularization, more data, early stopping, a simpler model).
>
> Note that *"more features"* and *"train longer"* are **underfitting** fixes — they appear as distractors on overfitting questions precisely because they're the right answer to the *opposite* problem.

### 🎣 The overfitting RED HERRING pattern

**A practice question buried the overfitting signature under an irrelevant technical detail — and it worked.**

> *A model was trained using some **custom library dependencies**. It **performs well on the training data** but **does noticeably worse on new data**. What should be done?*

| Phrase | Role |
|---|---|
| *"trained with custom libraries dependencies"* | 🎣 **RED HERRING.** Completely irrelevant to model performance — planted to steer you toward a software-packaging answer |
| *"performs well on training data… not as well on new datasets"* | ✅ **THE DIAGNOSIS.** This is the overfitting signature, and nothing else in the stem matters |

**Correct answer: perform a bias and variance tradeoff analysis** — that's the diagnostic step that tells you whether you have **high bias (underfitting)** or **high variance (overfitting)**, so you know which adjustment to make.

**Why the distractors fail:**

| Option | Why it's wrong |
|---|---|
| **Establish reliable packaging patterns for software libraries** 🎣 | Improves **consistency and reproducibility** of ML workflows — a real practice, but it **does nothing** for generalization. This is the bait the red herring was set for |
| **Train with smaller datasets** | **Backwards.** Less data **increases bias** and **reduces** the ability to generalize. More data helps overfitting, not less |
| **Automate training with MLOps and CI/CD** | Improves consistency and efficiency — but **automation doesn't replace analysing bias and variance**. Automating a badly-generalizing model just produces it faster |

> 🔑 **The transferable technique: find the DIAGNOSTIC phrase, ignore the DECORATION.**
>
> Exam stems contain scenario colour that sounds technical but carries no diagnostic weight — *custom libraries*, *global expansion*, *hourly readings*, *PDF format*. Before reading any option, ask: **"which single phrase actually tells me what's wrong?"** Here it's the train/test gap, and it points at overfitting no matter what else the sentence mentions.
>
> **Whenever you see "performs well on training data but poorly on new data," the answer is about bias/variance and generalization — full stop.** No infrastructure, packaging, or tooling answer can compete with it.

> **🔑 The mnemonic that fixes the matching question:**
> **OVER**fitting = the model **OVER**-learned the training data (memorized it).
> **UNDER**fitting = the model **UNDER**-learned (never got it at all).
>
> **Two phrases decide it every time:**
> - *"performs well on training data but fails to generalize to new data"* → **OVERfitting** (there's a gap)
> - *"too simple to capture the underlying patterns"* → **UNDERfitting** (the word **simple** is the giveaway)
>
> If a question mentions **good training performance at all**, it cannot be underfitting — underfitting is bad at *everything*.

### Generative AI terms

| Term | Definition |
|---|---|
| **Foundation model (FM)** | A very large model pre-trained on massive, broad, unlabeled data, adaptable to many downstream tasks. General-purpose by design. |
| **Large language model (LLM)** | An FM specialized for text, typically transformer-based, with billions of parameters. All LLMs are FMs; not all FMs are LLMs (image/multimodal FMs exist). |
| **Generative AI (GenAI)** | The application of FMs to generate new content — text, images, audio, video, code. |
| **Agentic AI** | AI that pursues a goal with autonomy: it plans multi-step work, calls tools/APIs, observes results, keeps memory, and iterates — rather than returning a single response. |

---

## 3. Agentic AI — new emphasis, expect questions

The current guide added agentic AI to Domain 1 (and much more in Domain 2). Minimum you need here:

A plain GenAI call is **one turn**: prompt in, content out. An **agent** adds a loop:

```
Goal → Plan → Pick a tool → Call it → Observe result → Re-plan → … → Done
                    ↑                                        │
                    └──────────────  memory  ────────────────┘
```

The four capabilities that make something "agentic":

1. **Autonomy / planning** — decomposes a goal into steps without being told each one.
2. **Tool use** — invokes external APIs, databases, functions to act on the world.
3. **Memory** — carries state across steps and sessions.
4. **Orchestration** — sequences the work, and may coordinate multiple specialized agents.

| | GenAI (plain) | Agentic AI |
|---|---|---|
| Interaction | Single request → response | Iterative loop until goal met |
| Can act externally? | No — text only | Yes — calls tools/APIs |
| State | Stateless (beyond the context window) | Persistent memory |
| Best for | Draft, summarize, answer | Book the trip, resolve the ticket, run the workflow |

AWS services to associate: **Amazon Bedrock Agents / AgentCore** (managed agents), **Strands Agents** (open-source SDK for building agents). Deeper coverage in Domain 2.

---

## 4. Types of machine learning

| Type | Training data | What it learns | Common algorithms | Use cases |
|---|---|---|---|---|
| **Supervised** | **Labeled** (inputs + known correct answers) | Map input → known output | Linear/logistic regression, decision trees, random forest, XGBoost, SVM | Spam detection, fraud detection, price prediction, image classification |
| **Unsupervised** | **Unlabeled** | Hidden structure/groupings on its own | k-means, hierarchical clustering, PCA, anomaly detection | Customer segmentation, anomaly detection, topic discovery, dimensionality reduction |
| **Reinforcement (RL)** | No dataset — an **environment** | Best action policy, via trial and error using **rewards** and penalties | Q-learning, PPO | Robotics, game playing, autonomous navigation, RLHF for tuning LLMs |

Two more you should recognize:

- **Semi-supervised** — a small labeled set plus a large unlabeled set. Used when labeling is expensive.
- **Self-supervised** — the model generates its own labels from the data's structure (e.g. predict the next token). **This is how FMs/LLMs are pre-trained**, and it's why they can use raw internet-scale text with no human labeling.

### ⚠️ "FMs use UNLABELED data for SELF-SUPERVISED learning"

**Memorize that sentence as a unit.** It's a single testable fact, and the wrong options are built by breaking it in one of two places.

| Option | Verdict |
|---|---|
| **Unlabeled + self-supervised** | ✅ **CORRECT** — how FMs are actually pre-trained |
| **Labeled + self-supervised** | ❌ **A contradiction in terms** — if humans labeled it, it isn't *self*-supervised |
| **Labeled + supervised** | ❌ True of ML generally, but **not how FMs are pre-trained** |
| **Unlabeled + supervised** | ❌ **A contradiction in terms** — supervised learning *requires* labels |

> 🔑 **The word "self" is the whole clue.** *Self*-supervised means **the model supervises itself** — it manufactures its own labels out of the raw data. Hide the next token, predict it, check against the actual text. **Nothing human-labeled is involved, by definition.** So "labeled" and "self-supervised" can never appear in the same correct answer.
>
> Equally, "unlabeled" and "supervised" can never co-occur — **supervised learning is defined by having labels.**

> **Why this matters beyond the definition:** self-supervision is *what makes foundation models possible at all*. Nobody could hand-label trillions of tokens. Because the data labels itself, you can pour in raw internet-scale text and get a model that learns language from it. **Remove self-supervision and there are no FMs.**

> **Two spots to keep separate:pre-training = unlabeled + self-supervised.** **Fine-tuning = labeled + supervised.** Same model, different stage, opposite data requirement — see [Domain 3, Task 3.3](../03-foundation-model-applications/03-training-fine-tuning.md).

> **Exam heuristic:** "we have labeled/historical examples with known answers" → supervised. "Find groups/patterns we haven't defined" → unsupervised. "Learns by trial, error, and reward" → reinforcement.

### 🔑 Signal words → learning type

The exam rarely names the technique; it describes it. **Learn to read the giveaway phrasing.**

| If the question says… | The answer is |
|---|---|
| labeled data, known outcomes, historical examples with answers, "predict X from past Y" | **Supervised** |
| unlabeled, discover, group, segment, "find patterns we haven't defined", no predefined categories | **Unsupervised** |
| **trial and error**, **environment**, **rewards and penalties/punishments**, **agent**, navigate, "learn effective strategies", policy, maximize a score, game playing, robotics | **Reinforcement** |
| a few labels plus lots of unlabeled data, "labeling is expensive" | **Semi-supervised** |
| the model creates its own labels from the data, predict the next token, how FMs are pre-trained | **Self-supervised** |
| take a model trained on one task and **reuse it on a related task**, fine-tune a pre-trained model | **Transfer learning** |

#### Reinforcement learning vocabulary

Any of these words in a question points at RL. AWS describes it as an agent learning to make decisions by performing actions in an environment, receiving rewards and punishments for each one.

| Term | Meaning |
|---|---|
| **Agent** | The learner/decision-maker |
| **Environment** | The world it acts in |
| **State** | The current situation |
| **Action** | What the agent does |
| **Reward / penalty** | The feedback signal that shapes behaviour |
| **Policy** | The learned strategy — which action to take in which state |

> **Why "trial and error" is decisive:** it means there's **no dataset of correct answers to learn from.** Supervised learning needs labeled right answers; unsupervised learning needs data to find structure in. Only RL learns from *consequences of its own actions* in an environment. Add "unpredictable environment" and "learning effective strategies" and there is no other candidate.
>
> The 📦 delivery-driver analogy below covers this: **Sam gets a bonus for a fast route and complaints for a late one.** Nobody hands Sam the optimal route — the world responds and Sam adjusts.

#### Transfer learning — the common distractor

A common wrong option, and easy to confuse because it *sounds* general-purpose.

**Transfer learning** = take a model already trained on one task and **adapt it to a different but related task**, reusing what it learned instead of starting from scratch. It's the principle behind fine-tuning a pre-trained FM.

| | Transfer learning | Reinforcement learning |
|---|---|---|
| **Starts from** | An existing trained model | Nothing — the agent explores |
| **Learns from** | A labeled dataset for the new task | Rewards and penalties from an environment |
| **Answers** | "How do I avoid training from scratch?" | "How do I learn a strategy with no correct answers to copy?" |

> **Trap:** transfer learning is a **reuse strategy**, not a way of learning from experience. It never answers a "trial and error in an environment" question. See also [Domain 3, Task 3.3](../03-foundation-model-applications/03-training-fine-tuning.md) where it appears as a fine-tuning method.

### 📦 Analogy — Training a new Amazon delivery driver

It's Monday morning. A new driver, Sam, starts at the depot. How Sam learns the job maps cleanly onto all five learning types.

**Supervised learning — riding along with a veteran**

Week one, Sam rides with Maya, who has driven the route for six years. At every stop Maya says: *"This one is 42 Oak Street — the blue door, not the gate. That's the right house."* Sam sees the input (a building) and is told the correct answer (the address it belongs to), hundreds of times. Eventually Sam predicts correctly before Maya says anything.

Someone provided the right answers. That's a **labeled dataset**, and this is **supervised learning**.

**Unsupervised learning — the unsorted pile**

Week two, Sam is handed 200 packages in a heap with no route sheet and no instructions. Nobody says how to organize them. Sam notices some share a postcode, some are all headed downtown, some are bulky and awkward — and sorts them into piles that make sense.

Nobody defined those piles in advance; Sam found the structure in the data. That's **clustering**, and this is **unsupervised learning**.

**Reinforcement learning — six months of routes**

Now Sam drives solo. One day Sam takes the highway and finishes early — bonus, happy customers. Another day Sam tries a shortcut, hits a school zone at 3pm, and delivers late — complaints. No one hands Sam the optimal route; Sam *tries things* and the world responds with rewards and penalties. After six months, Sam has an excellent internal policy for routing.

Learning by trial, error, and consequence. That's **reinforcement learning**.

**Semi-supervised learning — a few confirmed addresses**

New neighborhood. The supervisor confirms 10 of the 300 addresses in it. Sam uses those 10 known-correct examples to work out the numbering pattern, then infers the other 290.

A few labels plus a lot of unlabeled data. That's **semi-supervised learning** — used when labeling is expensive.

**Self-supervised learning — quizzing yourself**

On a slow afternoon Sam covers half of each address label with a thumb and guesses the hidden half before checking. *"12 Ma— Street"* → "Maple." Nobody made a quiz; Sam manufactured the questions and the answers out of the labels themselves.

That's **self-supervised learning** — and it's exactly how an LLM is pre-trained. Hide the next token, predict it, check against the real text. That's why LLMs can consume raw internet-scale text with **no human labeling at all**.

| In the depot | In machine learning |
|---|---|
| Maya naming the correct house at each stop | **Supervised** — labeled data |
| Sorting an unlabeled heap into sensible piles | **Unsupervised** — clustering |
| Getting bonuses for fast routes, complaints for late ones | **Reinforcement** — reward signal |
| 10 confirmed addresses, infer the other 290 | **Semi-supervised** |
| Covering the label and quizzing yourself | **Self-supervised** — FM/LLM pre-training |

> **The one-line test:** Was Sam **told** the answer (supervised), left to **find structure** (unsupervised), or shaped by **consequences** (reinforcement)?

---

## 5. Types of data

### Structured vs. unstructured

| | Structured | Semi-structured | Unstructured |
|---|---|---|---|
| **Form** | Fixed schema, rows/columns | Tags/hierarchy, flexible schema | No predefined model |
| **Examples** | Databases, **CSV**, spreadsheets | **JSON, XML**, log files | Text documents, images, audio, video, PDFs |
| **Typical approach** | Traditional ML (XGBoost, regression) | Either | Deep learning / FMs |
| **AWS storage** | RDS, Aurora, Redshift | DynamoDB, DocumentDB, S3 | S3 |

> ~80–90% of enterprise data is unstructured — which is exactly the argument for deep learning and FMs.

#### 📄 File format → data type cheat sheet

**The trap:** JSON and XML *look* organized — they have tags, nesting, and hierarchy — so they feel structured. **They are not.** "Structured" is a specific technical category meaning a **rigid, predefined, typically tabular schema where every record has the same attributes**. Having *some* structure is not the same as being *structured data*.

| Format / example | Category | Why |
|---|---|---|
| **`.csv`** — employee records with employee ID, name, department, salary | **Structured** | Tabular. Every row has the **same columns**. Standardized format, clearly defined attributes |
| Relational database table, Excel/spreadsheet, Parquet | **Structured** | Fixed schema, rows and columns |
| **`.json`** — user activity log with nested attributes | **Semi-structured** | Doesn't conform to a strict data model, but **has some elements of structure** (self-describing keys, nesting). Records can differ from one another |
| **`.xml`** — healthcare config settings, tagged and hierarchical | **Semi-structured** | Same reasoning: tagged and hierarchical, but no rigid tabular schema |
| Log files, YAML, HTML, NoSQL documents, emails | **Semi-structured** | Some organizing markup, flexible shape |
| **Plain text file of blog posts** (even with metadata) | **Unstructured** | No data model at all; attributes aren't clearly defined. **Adding metadata does not make it structured** |
| Images, audio, video, PDFs, free-form documents | **Unstructured** | No predefined model |

> **The one-question test:** *"Does every record have the same, predefined set of fields, laid out in rows and columns?"*
> **Yes** → structured (CSV, database table).
> **It's organized but records can vary / it's nested** → semi-structured (JSON, XML).
> **No inherent organization at all** → unstructured (plain text, images, audio).

> **Exam framing to expect:** a question offering a CSV, a JSON, an XML, and a plain text file and asking which is **structured**. The CSV is the answer *specifically because it's tabular with consistent columns* — and JSON/XML are the designed distractors, because "hierarchically organized" and "tagged" sound like structure. AWS's own rationale: semi-structured data "does not conform to a strict data model (like structured data) but has some elements of structure."

### Data types you must name

| Type | Description | Example task |
|---|---|---|
| **Labeled** | Each record carries the correct answer/tag | Supervised training |
| **Unlabeled** | Raw records, no answers attached | Unsupervised training, FM pre-training |
| **Tabular** | Rows and columns, fixed schema | Churn prediction, credit scoring |
| **Time-series** | Values ordered by timestamp, sequence matters | Demand forecasting, sensor anomaly detection |
| **Image** | Pixel data | Object detection, defect inspection |
| **Text** | Natural language | Sentiment analysis, summarization |

---

## 6. Types of inferencing

**Heavily tested.** AWS maps these to SageMaker AI inference options; know the decision drivers.

| Type | How it works | Latency | Scales to zero? | Sizing facts | Choose when |
|---|---|---|---|---|---|
| **Real-time** | Persistent, always-on endpoint | Milliseconds | ❌ No — you pay while idle | Small payloads, short timeout | Interactive apps needing consistent low latency |
| **Serverless** | Endpoint auto-provisions per request; scales down to 0 when idle | Low, but **cold starts** | ✅ Yes | Memory **1–6 GB**; max concurrency **200** per endpoint. No GPU support | Spiky/intermittent traffic; idle gaps; no infra management |
| **Asynchronous** | Requests queued, payload read from S3, result written to S3 | Near-real-time to minutes | ✅ Yes (to zero when queue empties) | Payloads up to **1 GB**, processing up to **1 hour** | Large payloads or long processing, but you still want per-request handling |
| **Batch (batch transform)** | Whole dataset processed offline, no endpoint at all | Minutes to hours | N/A — resources exist only for the job | Whole datasets | Scoring millions of records on a schedule; no live consumer waiting |

> 🚨 **Learn these numbers — don't settle for the relative ordering.** It's tempting to assume a foundational exam won't test exact limits, but the **60-second** threshold alone is enough to decide which inference type a scenario needs.

### ⏱️ The processing-time thresholds — these decide the question

| Inference type | **Max processing time** | Payload / data size | Queues requests? | Scales to zero? |
|---|---|---|---|---|
| **Real-time** | **60 seconds** | Small | ❌ No | ❌ No |
| **Serverless** | **60 seconds** | Small | ❌ No | ✅ Yes |
| **Asynchronous** | **1 hour** | **Up to 1 GB** | ✅ **Yes** | ✅ Yes |
| **Batch transform** | **Hours to days** | GB-scale whole datasets | N/A — offline job | N/A |

> 🔑 **The decision ladder — go by how long one request takes:**
>
> | Processing time | Answer |
> |---|---|
> | **Under 60s**, steady traffic, synchronous | **Real-time** |
> | **Under 60s**, spiky/intermittent traffic | **Serverless** |
> | **60s – 1 hour**, "near real-time", can be queued, ≤1 GB | **Asynchronous** ⭐ |
> | **Hours to days**, whole dataset, no live consumer | **Batch transform** |

> 🚨 **Worked example.** *"Near real-time inference for high-definition images… **up to 3 minutes** to process."*
> **3 minutes > 60 seconds**, so **real-time and serverless are both eliminated on the timeout alone.** 3 minutes is far short of "hours to days," so batch is out too. **Asynchronous** is the only option left — and the phrase **"near real-time"** is its signature.

> **Two phrases that always mean asynchronous:"near real-time"** and **"requests can be queued."** Async is the only type that queues requests to a live endpoint.

> **Two serverless details worth knowing:** it has **no GPU support** and **cannot use Model Monitor**, so a question about a serverless endpoint needing GPU inference or drift monitoring is pointing you somewhere else. **Provisioned Concurrency** is the fix for cold starts — it keeps capacity warm for predictable bursts.

**Decision cues:**

- "Immediate response to a user" → **real-time**
- "Unpredictable, intermittent traffic; don't pay for idle" → **serverless**
- "Large payloads / long inference times / can wait" → **asynchronous**
- "Score the entire dataset nightly, results to S3" → **batch**

> **Trap:** batch vs. asynchronous. *Batch* = an offline job over a whole dataset, no endpoint. *Asynchronous* = a live endpoint with a queue, per-request. If the question mentions a *queue* and *individual requests*, it's asynchronous.

---

## Exam traps for this task

1. **Algorithm vs. model** — procedure vs. trained artifact.
2. **Overfitting is identified by the train/test gap**, not by low accuracy alone.
3. **Bias is overloaded** — fairness bias (Domain 4) vs. statistical bias (bias–variance).
4. **All LLMs are FMs; not all FMs are LLMs.**
5. **Deep learning ⊂ ML ⊂ AI**, and GenAI sits inside deep learning. Agentic AI is a pattern on top, not a subset ring.
6. **Serverless ≠ asynchronous.** Both scale to zero; only asynchronous handles huge payloads and long runtimes.
7. **FMs are pre-trained with self-supervised learning on unlabeled data** — that's why no human labeling is needed at pre-training.
8. **Rule-based systems are still AI**, even though they involve no learning. A question describing hard-coded if/then logic is AI but *not* ML.
