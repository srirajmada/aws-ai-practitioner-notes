# Task 3.1 — Design considerations for applications that use FMs

> Domain 3 (28%) — the largest domain, and this is its densest task. **The customization cost spectrum and RAG are the two highest-yield topics on the entire exam.**

---

## 1. FM selection criteria

| Criterion | The question it answers | Effect on the decision |
|---|---|---|
| **Modality** | Text, image, video, audio — in and out? | Eliminates models immediately |
| **Cost** | Price per input/output token × volume | Usually decisive at scale |
| **Latency** | Interactive chat or overnight batch? | Small models are dramatically faster |
| **Model size / complexity** | Bigger = more capable, slower, pricier | The central tradeoff |
| **Multi-lingual** | Which languages must it handle? | Not all models are equally strong across languages |
| **Input/output length** | How big is the context window? How long may the answer be? | Long-document work needs a large window |
| **Customization** | Does it support fine-tuning, distillation, RAG? | Constrains your adaptation path |
| **Prompt caching** | Is a long prefix re-sent on every call? | Big cost saver when supported |
| **Compliance / licensing** | Regulated industry, data residency, licence terms | Can veto an otherwise ideal model |
| **Region availability** | Is the model in the Region you need? | A hard practical constraint |

**Prompt caching** deserves its own note since the guide names it. If every request repeats the same long prefix — a big system prompt, an uploaded document, standing few-shot examples — caching lets the model **skip recomputing that prefix** on subsequent calls.

AWS's published figures: **up to 90% lower cost and up to 85% lower latency** on supported models (Claude 3.5 Haiku, Claude 3.7 Sonnet, Nova Micro/Lite/Pro). The classic use case is a chatbot where a user uploads a document and then asks several questions about it — without caching, the whole document is reprocessed and re-billed on every single question.

> **Method:** define the quality bar → try the **smallest, cheapest** model → measure on your own evaluation set → escalate only on failure. Never start at the biggest model.

---

## 2. Inference parameters

These change the *output* without changing the *model*. AWS groups them into two families.

### Randomness and diversity

At each step the model produces a **probability distribution over possible next tokens**, then samples from it. These parameters reshape or truncate that distribution.

| Parameter | What it does | Lower value | Higher value |
|---|---|---|---|
| **Temperature** | Reshapes the probability distribution — flattens or steepens it | **Steeper** → favours high-probability tokens → focused, deterministic, repetitive | **Flatter** → lower-probability tokens become viable → creative, varied, riskier |
| **Top K** | Considers only the **K most-likely** candidate tokens | Smaller pool → safer | Larger pool → more variety |
| **Top P** (nucleus) | Considers only tokens within the top **P%** of cumulative probability | Smaller pool → safer | Larger pool → more variety |

**AWS's own worked example.** For the prompt `I hear the hoof beats of "`, suppose the model's candidates are:

```
horses:   0.7
zebras:   0.2
unicorns: 0.1
```

- **High temperature** flattens these probabilities toward each other — "unicorns" becomes more likely, "horses" less.
- **Top K = 2** → the model considers only `horses` and `zebras`. "Unicorns" is off the table entirely.
- **Top P = 0.7** → only `horses` qualifies (it alone fills the top 70%). **Top P = 0.9** → `horses` and `zebras` both qualify.

> **The key distinction:** Top K is a **fixed count** (the best 5 tokens). Top P is a **cumulative percentage** (however many tokens it takes to reach 90% of the probability mass) — so the pool size *changes* with how confident the model is. Temperature doesn't truncate anything; it re-weights.

### 🎬 Analogy — Picking what to watch tonight

Your streaming app has ranked everything by how likely you are to enjoy it. Tonight's viewing is chosen by sampling from that ranked list — and the three parameters are three different ways of narrowing it.

**Top K** is house rule number one: *"Only ever show me the top 5 titles."* A fixed count. Doesn't matter whether the app is highly confident about number one or genuinely torn between fifteen options — you always see exactly 5.

**Top P** is a smarter rule: *"Show me however many titles it takes to cover 80% of what I'd probably enjoy."* Some nights the app is confident — two shows account for 80%, so you see two. Other nights it's uncertain and the same 80% takes twelve titles, so you see twelve. **The list size adapts to how certain the recommendation is.** That's the whole difference from Top K.

**Temperature** isn't about list length at all — it's your mood. On a **low-temperature** night you want comfort: the same sitcom you've watched four times, the safe number-one pick, every time. On a **high-temperature** night you're feeling adventurous, and that obscure foreign documentary sitting at rank forty suddenly looks appealing. Turn it high enough and you'll end up watching something genuinely strange.

| Friday night | Inference parameter |
|---|---|
| "Only show me the top 5" | **Top K** — fixed count |
| "Show me enough titles to cover 80% of my likely enjoyment" | **Top P** — cumulative probability, adaptive size |
| Comfort-watch mood vs. adventurous mood | **Temperature** — re-weights the odds |
| "Stop the episode at the credits" | **Stop sequences** |
| "I've only got 40 minutes" | **Max tokens / response length** |

> **Exam cue:** *factual, consistent, reproducible* (extraction, classification, SQL, structured output) → **low temperature**. *Creative, varied, brainstorming, marketing copy* → **higher temperature**. Adjust temperature *or* top-P — tuning both at once makes the effect hard to reason about.

#### 🍽️ top-p vs top-k — the party-catering analogy

*A deeper cut on the same pair. The 🎬 analogy above gets you the distinction; this one explains **why** top-p adapts and top-k can't.*

At each word the model has a probability for every possible next token — **each token is a guest, and its probability is how likely that guest is to show up**. You pick one; that's the next word.

- **`top_k` = "always cook exactly K meals"** — a **fixed headcount** regardless of the situation. It keeps the K most-likely tokens even if K−1 of them are near-zero slivers (dragging in junk when the model is confident), or if the real spread is wider than K (over-restricting when it's uncertain).
- **`top_p` (nucleus) = "cook enough to cover p% of realistic turnout, skip the flaky tail"** — an **adaptive headcount**. Starting from the most-likely token, keep adding until cumulative probability ≥ p, then drop the improbable tail — the "maybe" guests who won't come.

**Why top-p adapts (the key property).** The nucleus size **flexes with how confident the model is at that step** — i.e. with the shape of the distribution:

| The model is… | Distribution | Example prompt | Nucleus | Party version |
|---|---|---|---|---|
| **Sure** of the next word | Peaked | *"the capital of France is ___"* | ~1 token | Champions League final is on — only the die-hard shows up |
| **Torn** | Flat | *"my favorite hobby is ___"* | Large | Perfect weather — everyone comes |

**Two different ways the plate shrinks — don't conflate them:**

| Cause | What it is |
|---|---|
| **Lowering p** (0.9 → 0.3) | **You** rewriting the catering policy to be stingier — safer, more focused. **A knob you control** |
| **A peaked distribution** | **The situation** being more certain. **Context-driven, not your choice** |

Same smaller nucleus, different cause.

> **The takeaway:** `p` = *"cater for 90% of likely turnout"* — the **fixed policy**; the model's **per-word confidence** decides how many that actually is tonight.
> ⇒ **`top_p ≈ 0.9` keeps output varied-but-bounded** (prunes the nonsense tail without capping genuine variety); **`top_k` is a blunt fixed cap.**

### Length parameters

| Parameter | Effect |
|---|---|
| **Max tokens / response length** | Caps output length — a direct **cost control** *and* **latency control**. Too low and answers get cut off mid-sentence |
| **Stop sequences** | Strings that halt generation when produced — useful for structured formats, and they cut latency by ending generation sooner |
| **Penalties** | Discourage repetition, token frequency, or excessive length |

### ⚡ What actually drives LLM latency

**The rule that resolves this cleanly:**

> **Randomness parameters (temperature, Top K, Top P) change WHAT you get — they do NOT affect latency.**
> **Length factors (input tokens, output tokens, max tokens) change HOW MUCH you get — and only "how much" affects latency.** **Why sampling parameters are free.** Generating a single token requires a **full forward pass through the model's billions of parameters**, which produces a probability distribution across the entire vocabulary. Temperature, Top K, and Top P only decide **which token to pick from a distribution that has already been computed**. Temperature divides the logits by a scalar; Top K and Top P do a small selection/sort. That's microseconds against tens to hundreds of milliseconds of compute. **The expensive work is already finished before they act.**

| Factor | Impacts latency? | Why |
|---|---|---|
| **Number of tokens in the prompt** | ✅ **Yes** | The model must process the whole input before emitting the first token — this is your **time to first token** (the *prefill* phase) |
| **Number of tokens in the response** | ✅ **Yes** | LLMs generate **one token at a time**, each requiring another full forward pass (the *decode* phase). **Usually the dominant factor in total latency** |
| **Max tokens / stop sequences** | ✅ Yes | They cap or end generation sooner — fewer forward passes |
| **Model size** | ✅ Yes | A bigger model means more computation per forward pass |
| **Hardware / instance type** | ✅ Yes | Provisioned Throughput vs. on-demand, GPU type |
| **Cold starts** | ✅ Yes | Serverless endpoints spinning up |
| **Region / network distance** | ✅ Yes | Round-trip time |
| **Prompt caching** | ✅ Yes — *reduces* it | Skips re-processing a repeated prefix (**up to 85% lower latency**), which is itself proof that input tokens drive latency |
| **Temperature** | ❌ **No** | Only reshapes an already-computed distribution |
| **Top K** | ❌ **No** | Only limits how many candidates are considered from an already-computed distribution |
| **Top P** | ❌ **No** | Same — a cumulative-probability cut on an already-computed distribution |

> **The one legitimate nuance** (worth knowing, but it does *not* make temperature a right answer): a higher temperature can make a model more likely to produce rambling or longer output, which raises the **output token count** and therefore latency. That's an **indirect, non-deterministic, second-order** effect operating *through* response length. The exam asks about direct factors — and AWS states plainly that temperature, Top K and Top P do **not** impact LLM invocation latency.

> **Cross-check with what you already know:** prompt caching cuts latency by up to 85% precisely by **skipping input-token processing**, and batch inference is cheaper because it drops the latency requirement altogether. Both confirm that **token counts — not sampling settings — are the latency levers.**

> **Trap:** inference parameters change *behaviour*, never *knowledge*. Turning temperature to 0 does **not** stop hallucination — it makes the model hallucinate the *same wrong thing* consistently.

---

## 3. Retrieval Augmented Generation (RAG)

**The single most important concept in Domain 3.**

RAG grounds an FM's answers in *your* data by retrieving relevant passages at query time and inserting them into the prompt — **no training required**.

### How it works

```
INGESTION (once, and on updates)
  Documents → chunk → embed → store vectors in a vector database

QUERY (every request)
  User question → embed the question → similarity search the vector store
      → retrieve top-N relevant chunks
      → build prompt: [instructions + retrieved chunks + question]
      → FM generates an answer grounded in those chunks, with citations
```

### Why RAG is usually the right answer

| Problem | How RAG solves it |
|---|---|
| Model doesn't know your private/company data | Retrieves it at query time |
| Knowledge cutoff — no recent information | Retrieve current documents |
| Hallucination | Answers are grounded in supplied sources |
| No audit trail | Returns **citations** to source documents |
| Data changes constantly | Update the index, not the model |
| Access control | Filter retrieval by user permissions/metadata |

**Business applications:** internal knowledge assistants (HR/IT policy Q&A), customer support grounded in product docs, legal and contract search, medical literature lookup, financial research over filings, technical documentation search.

### Amazon Bedrock Knowledge Bases
The **managed RAG service** — it handles ingestion, chunking, embedding, vector storage, retrieval, and citation for you.

- **APIs:** `Retrieve` (returns passages only — you handle generation) and `RetrieveAndGenerate` (full RAG answer with citations).
- **Chunking:** fixed-size, semantic, hierarchical, or none.
- **Embedding models:** Amazon Titan Embeddings G1 (1536 dimensions), Titan Text Embeddings V2 (256/512/1024), Cohere Embed English and Multilingual (1024). Multimodal options include Titan Multimodal Embeddings and Amazon Nova Multimodal Embeddings.
- **Extras:** metadata filtering, **reranking** models to improve result ordering, hybrid search (semantic + keyword), and structured data stores (natural language → SQL).

> **Trap:** RAG does not change the model's weights at all. If a question says "the model must know current company data," the answer is **RAG**, not fine-tuning.

---

## 4. Vector stores on AWS

Where the embeddings live. **Amazon Bedrock Knowledge Bases supports:**

| Vector store | Notes |
|---|---|
| **Amazon OpenSearch Serverless** | The common default — auto-scaling, no cluster management |
| **Amazon OpenSearch Managed Cluster** | When you want cluster-level control |
| **Amazon Aurora PostgreSQL** | Uses the open-source **pgvector** extension; good if you're already on Postgres |
| **Amazon Neptune Analytics** | Graph + vectors — powers **GraphRAG** for relationship-aware retrieval |
| **Pinecone** | Third-party managed vector DB |
| **MongoDB Atlas** | Third-party |
| **Redis Enterprise Cloud** | Third-party |

Also relevant: **Amazon RDS for PostgreSQL** (pgvector), **Amazon DocumentDB**, and **Amazon MemoryDB** support vectors outside the Knowledge Bases integration. OpenSearch Serverless and Managed Cluster are the only stores supporting **binary vectors** (cheaper storage).

> **Exam cues:** "fully managed, scales automatically, no cluster to run" → **OpenSearch Serverless**. "already using PostgreSQL / relational" → **Aurora PostgreSQL with pgvector**. "relationships between entities matter" → **Neptune Analytics (GraphRAG)**.

---

## 5. FM customization — the cost spectrum

**Learn this ordering cold. It generates more questions than anything else in Domain 3** — including an **Ordering**-type question asking you to rank techniques most → least cost-effective.

> 🚨 **The exam ordering, memorize it as a sequence:**
> **1. Prompt engineering → 2. RAG → 3. Fine-tuning → 4. Continued pre-training → 5. Pre-training from scratch**
>
> **A frequent mix-up is the order of RAG and fine-tuning.** **RAG is always cheaper than fine-tuning** on this exam.

### 💰 How to actually compare RAG vs. fine-tuning cost

The question "which costs more?" is answered by looking at **what each one adds** to a baseline of just calling the model:

| | **RAG adds…** | **Fine-tuning adds…** |
|---|---|---|
| **Up-front, one-time** | Embedding the corpus (cheap — a one-off embedding-model run) | **Data labeling** (human effort — usually the single biggest cost) + **training compute** + evaluation |
| **Ongoing fixed** | **Vector database** running cost (OpenSearch Serverless, Aurora, etc.) | **Custom model hosting** — serve it **on-demand** (per-token) **or** with **Provisioned Throughput** (hourly, whether used or not). Not PT-only |
| **Ongoing per-call** | **More input tokens** — retrieved chunks are billed on every single call | Nothing extra (prompts stay short) |
| **When data changes** | **Re-index the changed documents.** Cheap and fast | **Re-label and re-train the whole model.** Expensive and slow |
| **Labeled data needed?** | ❌ No — just documents | ✅ **Yes** — labeled prompt/response pairs |

**AWS's own reasoning:**
- Prompt engineering requires **no additional infrastructure** → most cost-effective.
- RAG adds the cost of an **embedding model and a vector database** → more than prompt engineering.
- Instruction-based fine-tuning adds **data labeling and training costs** → **less cost-effective than RAG**.

> **The one-line answer to "which costs more?":**
> **RAG buys infrastructure (a vector DB and bigger prompts). Fine-tuning buys human labour and GPU time (labeling + training), then keeps charging you for dedicated hosting.** Labeling and training dominate — which is why **fine-tuning is always the more expensive of the two on this exam.**

> **The cost that decides it in practice:** *maintenance*. When your data changes, RAG needs a **re-index**; fine-tuning needs a **re-train**. Facts change constantly, so fine-tuning's cost isn't paid once — it's paid every refresh cycle.

> **Real-world nuance** (know the boundary, but don't let it change your exam answer): at very high volume with *stable* requirements, fine-tuning can win on unit economics, because RAG pays for retrieved context tokens on **every** call while a fine-tuned model uses short prompts. **The exam's ordering is fixed regardless — always rank RAG cheaper than fine-tuning.**

| Approach | What changes | Cost | Time | Needs training data? | Use when |
|---|---|---|---|---|---|
| **Prompt engineering** (in-context learning) | Nothing — just the prompt | **Lowest** | Minutes | No | Always try first |
| **RAG** | Nothing — adds retrieved context | Low | Hours–days | No (just documents) | Model needs *your* or *current* facts |
| **Fine-tuning** | Model weights, on labeled examples | Medium–high | Days | Yes — labeled | Consistent style, format, or tone; domain behaviour |
| **Continued pre-training** | Model weights, on large unlabeled domain text | High | Weeks | Yes — large unlabeled corpus | Deep domain vocabulary (medicine, law) |
| **Pre-training from scratch** | Everything — a new model | **Extreme** ($M+) | Months | Massive | Essentially never |
| **Model distillation** | Trains a small model to imitate a large one | Medium up front, **then cheap** | Days | Teacher outputs | High volume where inference cost dominates |

**In-context learning** = teaching the model within the prompt itself (instructions + examples), with **no weight changes**. Zero-shot, one-shot, and few-shot prompting are all in-context learning.

### 🍟 Analogy — Getting a new hire up to speed at a burger franchise

A new employee starts Monday. They already know how to cook — the question is how you get them making *your* food, *your* way. Five options, in increasing order of expense:

**Prompt engineering — telling them clearly what to do.**
"Two patties, no pickles, sauce on the bottom bun, into the wrapper folded seam-down." Free, instant, and repeatable for every order. If they get it wrong, you rephrase the instruction. **You'd be mad to skip this and jump straight to sending them on a course** — and yet that's exactly the mistake the exam wants you to avoid.

**RAG — handing them the binder on the counter.**
Nobody memorizes 400 menu items, allergen tables, and this week's promotions. You put a binder next to the till. When a customer asks whether the sauce contains soy, they *look it up* and read the answer back. Crucially: when head office changes the recipe on Tuesday, **you swap one page in the binder** — you don't retrain the employee. And if a manager asks where the answer came from, they can point at the page. **That's citations.** **Fine-tuning — a week of in-store training.**
You want them to *behave* a certain way without being told each time: greet in the house style, wrap to the house standard, upsell in the house tone. That takes repetition with real examples until it's habit. Costs a week of wages, and once trained they're consistent. But note what it *doesn't* do: training them in your greeting style doesn't tell them Tuesday's new price list. **Habits, not facts.** **Continued pre-training — sending them to culinary school for a different cuisine.**
You're opening a sushi counter. That's not a house-style tweak; they need a whole new vocabulary and technique base. Expensive, slow, and only worth it when the domain itself is genuinely foreign.

**Pre-training from scratch — raising a person from birth.**
Absurd for a restaurant, and it's the right instinct. Only the model providers do this.

**Distillation — the experienced hire writes the simple playbook.**
Your best veteran is brilliant but expensive per hour. So you have them work through a thousand scenarios, write down exactly how they handled each, and use that to train a junior who now handles 95% of orders at a fraction of the wage. High cost once, cheap forever after.

| At the franchise | FM customization |
|---|---|
| Telling them clearly what to do | **Prompt engineering** (in-context learning) |
| The binder on the counter, swap a page when it changes | **RAG** — facts, current, cited |
| A week of in-store training on house style | **Fine-tuning** — behaviour and format |
| Culinary school for a new cuisine | **Continued pre-training** — deep domain |
| Raising a person from birth | **Pre-training from scratch** |
| Veteran writes the playbook, junior follows it | **Model distillation** |

> **The two questions that decide it, every time:**
> **"Does the model need to KNOW something?"** → RAG (the binder).
> **"Does the model need to BEHAVE differently?"** → fine-tuning (the training week).
>
> Facts change often and must be current and citable → RAG. Style, tone, and format need to be consistent → fine-tuning. Both → do both.

---

## 6. AI agents

An **agent** uses an FM as a reasoning engine to accomplish a goal — planning steps, calling tools, observing results, and iterating, rather than returning a single response.

**What an agent adds beyond RAG:** RAG *retrieves information*. An agent **takes actions** — it can query a database, call an API, book something, file a ticket, run code.

### Amazon Bedrock Agents
Managed agents that break a request into steps and call your APIs.

| Component | Purpose |
|---|---|
| **Foundation model** | The reasoning engine |
| **Instructions** | Defines the agent's role and rules |
| **Action groups** | The tools — Lambda functions described by an **OpenAPI schema** or function definitions |
| **Knowledge bases** | RAG grounding for the agent |
| **Memory** | Retains context across sessions |
| **Prompt templates** | Customize each stage of the agent's reasoning |

**ReAct** (Reason + Act) is the underlying pattern: the model reasons about what to do, acts by calling a tool, observes the result, and repeats.

**Business applications:** automated customer service (look up an order, process the refund, send confirmation), IT helpdesk automation, insurance claims processing, travel booking, financial report generation, supply-chain checks.

> **Trap:** if a scenario only needs *information* returned, RAG is sufficient and cheaper. If it needs something *done* — a record updated, an API called — you need an agent.

---

## Exam traps for this task

1. **Know the customization cost ladder in order:** prompt engineering → RAG → fine-tuning → continued pre-training → pre-training. Cost questions almost always resolve to the cheapest option that works.
2. **RAG for facts, fine-tuning for behaviour.** The most-tested distinction in the domain.
3. **Fine-tuning does not add current knowledge.** Data changes → RAG.
4. **RAG changes no model weights.**
5. **Top K = fixed count; Top P = cumulative percentage** (adaptive pool size); **temperature re-weights** rather than truncating.
6. **Low temperature ≠ no hallucination.** It makes output consistent, not correct.
7. **Max tokens is a cost lever**, not just a formatting choice.
8. **OpenSearch Serverless is the default vector store**; Aurora/RDS PostgreSQL for pgvector; **Neptune Analytics for GraphRAG**.
9. **Agents act; RAG only retrieves.** Match to whether the scenario requires an action.
10. **Bedrock Agents use action groups backed by Lambda + OpenAPI schemas.**
11. **Prompt caching cuts cost when a long prefix repeats** across calls.
12. **Distillation trades up-front training cost for permanently cheaper inference.**
