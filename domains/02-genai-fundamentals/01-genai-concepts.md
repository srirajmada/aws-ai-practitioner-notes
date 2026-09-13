# Task 2.1 — Explain the basic concepts of generative AI

> Domain 2 (24%). The vocabulary spine of the whole exam — Domains 3 and 5 assume everything here. Also where the newest material lives: **context engineering**, **MCP**, and **multi-agent patterns** are recent additions that older courses don't cover.

---

## 1. Tokens

A **token** is the unit an LLM actually reads and writes — roughly a word-piece, not a word.

- Rough rule of thumb: **1 token ≈ 4 characters ≈ ¾ of an English word**. So ~100 tokens ≈ 75 words.
- "unbelievable" might split into `un` + `believ` + `able`. Common words are single tokens; rare words fragment.
- Both **input** (prompt) and **output** (completion) are counted — and usually **priced separately**, with output typically costing more.

**Context window** = the maximum number of tokens a model can consider at once (prompt + response together). Exceed it and the model must truncate. Modern models range from thousands to millions of tokens (Amazon Nova Premier: 1M).

> **Why you care:** tokens are the billing unit *and* the memory limit. Nearly every GenAI cost question traces back to token count.

---

## 2. Embeddings and vectors

An **embedding** is a numeric vector representation of meaning. Text (or an image, or audio) goes in; a list of numbers comes out — often hundreds or thousands of dimensions.

The critical property: **semantically similar things land close together in vector space.** "dog" sits near "puppy"; "bank loan" sits far from "river bank."

- **Vector** = the array of numbers itself, e.g. `[0.021, -0.44, 0.87, …]`.
- **Vector database / vector store** = purpose-built storage that indexes vectors so you can find nearest neighbours fast.
- **Similarity search** = given a query vector, return the closest stored vectors. Usually measured by **cosine similarity** (angle between vectors).

This is the machinery under RAG and under semantic search.

### 🛒 Analogy — The supermarket that shelves by meaning

A normal warehouse stores boxes alphabetically. Ask for "cereal" and it finds the box labelled *cereal* — but only if you use that exact word. Ask for "breakfast stuff" and it finds nothing. That's **keyword search**.

Now picture a supermarket instead. Nobody shelves alphabetically — that would put *apples* next to *ammonia*. Things are placed **by meaning**: cereal near oats near granola near breakfast bars, all in one aisle. Cleaning products are far away, in a different part of the store entirely.

You don't need the exact product name. Walk to the breakfast aisle and everything relevant is within arm's reach, including brands you'd never have thought to ask for.

- The **store layout** — the fact that position encodes meaning — is the **embedding space**.
- Each product's **shelf coordinates** (aisle 7, bay 3, shelf 2) are its **vector**.
- The machine that decides where a new product goes is the **embedding model**.
- Walking to the right aisle and grabbing what's within reach is **similarity search / nearest-neighbour retrieval**.
- The store itself is the **vector database**.

And a second store in another town can lay things out differently — which is why **you must use the same embedding model for your documents and your queries.** Query a store built by a different planner using coordinates from your own, and you'll end up in the cleaning aisle asking for cornflakes.

> **Exam-relevant consequence:** if a question mentions re-embedding, changing embedding models, or mismatched retrieval quality, the answer usually involves re-indexing the whole corpus with the *same* model.

---

## 3. Chunking

FMs have a finite context window, so you can't paste a 400-page manual into a prompt. **Chunking** splits documents into smaller passages before embedding them.

| Consideration | Effect |
|---|---|
| **Chunks too large** | Retrieval returns lots of irrelevant text; wastes tokens; dilutes the signal |
| **Chunks too small** | Meaning gets severed mid-thought; retrieved passage lacks the context to answer |
| **Overlap** | Repeating a little text between adjacent chunks prevents ideas being cut in half at a boundary |

Common strategies: **fixed-size** (every N tokens), **semantic** (split on meaning boundaries), **hierarchical** (parent/child chunks). Amazon Bedrock Knowledge Bases can chunk for you.

---

## 4. Transformers and LLMs

The **transformer** is the neural network architecture (2017, *"Attention Is All You Need"*) behind essentially every modern LLM.

Its key innovation is the **attention mechanism** (specifically self-attention): when processing a word, the model weighs how much every *other* word in the input matters to it. That's how it resolves "it" in *"the trophy didn't fit in the suitcase because **it** was too big."*

Why transformers displaced what came before (RNNs/LSTMs):

| | RNN / LSTM | Transformer |
|---|---|---|
| Processing | Sequential, word by word | **Parallel** — whole sequence at once |
| Long-range context | Degrades over distance | Attention reaches anywhere directly |
| Training speed | Slow, hard to scale | Parallelizable → trainable on GPU clusters at scale |

Parallelism is *why* massive pre-training became possible, which is *why* LLMs exist.

**LLM** = a transformer-based model with billions of parameters, pre-trained on huge text corpora, that generates text by repeatedly predicting the next token.

---

## 5. Model types you must distinguish

| Type | What it does | Examples |
|---|---|---|
| **Foundation model (FM)** | Large, broadly pre-trained, adaptable to many tasks | Amazon Nova, Claude, Llama |
| **LLM** | An FM specialized for text | Claude, Llama, Nova Micro |
| **Multi-modal model** | Accepts and/or produces **more than one modality** (text, image, audio, video) | Nova Lite/Pro/Premier (text+image+video in → text out), Claude with vision |
| **Diffusion model** | Generates **images/video** by starting from random noise and iteratively denoising toward the prompt | Stable Diffusion, Amazon Nova Canvas |
| **Embedding model** | Converts input into vectors — **does not generate content** | Amazon Titan Text Embeddings, Cohere Embed |
| **GAN** (older) | Generator vs. discriminator networks competing | Legacy image generation |

> **Trap:** *diffusion models generate images by removing noise*, not by predicting tokens. If a question describes iterative denoising from random static, it's diffusion. If it describes next-token prediction, it's an LLM.
>
> **Trap:** *multi-modal* means multiple modalities, not multiple languages. Multiple languages is **multi-lingual**.

---

## 5b. Prompt engineering (definition)

Named in this task's concept list, though the techniques are examined in Domain 3.

**Prompt engineering is the practice of designing the input to an FM to get a better output — without changing the model.** It's the cheapest, fastest form of customization, and always the first thing to try.

| Term | Meaning |
|---|---|
| **Prompt** | The full input sent to the model: instructions, context, data, and format requirements |
| **In-context learning** | Teaching the model inside the prompt itself, with **no weight changes** — zero-shot, one-shot, and few-shot are all in-context learning |
| **System prompt** | Persistent role, rules, and constraints applied across the conversation |

→ Full treatment — constructs, zero/one/few-shot, chain-of-thought, injection risks, Bedrock Prompt Management — in [Domain 3, Task 3.2](../03-foundation-model-applications/02-prompt-engineering.md).

---

## 6. The FM lifecycle

The objective lists the stages explicitly — know the order.

```
1. Data selection → 2. Model selection → 3. Pre-training → 4. Fine-tuning →
5. Evaluation → 6. Deployment → 7. Feedback ──┐
        ↑                                      │
        └──────── continuous improvement ──────┘
```

| Stage | What happens |
|---|---|
| **1. Data selection** | Choose and curate the corpus. Quality, diversity, licensing, and bias all get decided here |
| **2. Model selection** | Pick an existing FM or decide to build. For nearly everyone: pick an existing one |
| **3. Pre-training** | Self-supervised training on massive unlabeled data. Enormously expensive — done by model providers, not customers |
| **4. Fine-tuning** | Adapt the pre-trained model to a domain or task using a smaller labeled dataset |
| **5. Evaluation** | Measure quality against benchmarks, task metrics, and human judgement |
| **6. Deployment** | Serve the model — managed API or self-hosted |
| **7. Feedback** | Collect real usage signals and route them back into tuning and data selection |

> **The practical point:** as a customer you almost always join at **stage 2**, and your work is stages 4–7. Pre-training is a provider activity.

---

## 7. Token-based pricing

**You pay per token, input and output counted separately.** No servers, no idle charges — consumption pricing.

What drives your bill:

| Driver | Effect on cost |
|---|---|
| **Prompt length** | Longer prompts = more input tokens, every single call |
| **Response length** | Output tokens usually cost **more** than input tokens |
| **Model choice** | Larger/more capable models cost multiples of smaller ones |
| **Call volume** | Linear |
| **Conversation history** | Re-sent on every turn — a long chat re-bills the whole transcript each time |
| **RAG context** | Retrieved passages are input tokens too |

**Cost/performance tradeoff, and the reflexive exam answer:** use the **smallest model that meets the quality bar**. Route simple requests to a cheap fast model (Nova Micro/Lite) and escalate only hard ones to a large model (Nova Pro/Premier). Amazon Bedrock **Intelligent Prompt Routing** automates exactly this.

Other levers: **prompt caching** (reuse a repeated prefix at reduced cost), **batch inference** (~50% cheaper, offline), **model distillation** (train a small model from a large one), and concise prompting.

> **Trap:** a long system prompt or a big retrieved context looks free because it's "just input" — it isn't. It's billed on *every* invocation.

---

## 8. Context engineering

**New in the current guide.** AWS defines it as designing systems that **dynamically assemble the optimal set of information** for the model to do a task — treating the context window not as a static text box but as a **workspace to be populated deliberately**.

Prompt engineering is a subset: writing a good instruction. Context engineering is the broader discipline of deciding **everything that goes into the window, and from where**.

| | Prompt engineering | Context engineering |
|---|---|---|
| Scope | Wording of the instruction | The entire assembled payload |
| Nature | Largely static, hand-authored | **Dynamic**, assembled per request |
| Concerns | Clarity, examples, format | What to retrieve, what to include, what to leave out, ordering, token budget |

A well-engineered context payload typically combines:

1. **System prompt** — role, rules, tone
2. **Few-shot examples** — for consistent formatting
3. **User query** — the actual request
4. **User/profile data** — for relevance and personalization
5. **Retrieved knowledge (RAG)** — grounding documents
6. **Conversation history / memory** — what happened earlier
7. **Tool definitions and results** — for agents

> **Why AWS added it:** as apps became agentic and retrieval-heavy, the bottleneck stopped being prompt wording and became *context assembly under a token budget*. Garbage in the window → garbage out, no matter how good the prompt is.

---

## 9. Agentic AI concepts

Heavily expanded in the current guide. Domain 1 defined what an agent is; here you need the **building blocks**.

### The agent loop

```
Goal → Reason/Plan → Select tool → Invoke → Observe result → Re-plan → … → Answer
           ↑                                                      │
           └──────────────────── memory ──────────────────────────┘
```

### Tool use (function calling)

The FM is given descriptions of available tools. When it decides one is needed, it emits a structured request naming the tool and its arguments; your application executes it and returns the result; the model continues with that result in context.

> Critical distinction: **the model does not run the tool.** It *asks* for the tool to be run. Your application (or the agent runtime) executes it and hands back the output.

### Memory management

| Type | Scope | Purpose |
|---|---|---|
| **Short-term** | Within one session/conversation | Multi-turn coherence — remembering what was said three turns ago |
| **Long-term** | Across sessions | Durable user preferences, facts, past outcomes |

Because context windows are finite and every re-sent token costs money, memory management means **deciding what to keep, what to summarize, and what to drop**. AgentCore Memory provides both tiers as a managed service.

### Model Context Protocol (MCP)

**MCP is an open standard for connecting AI agents to external tools and data sources.** Instead of writing a bespoke integration for every model–tool pair, a tool is exposed once as an MCP **server**, and any MCP-compatible **client** (agent) can use it.

- Roles: an MCP **server** exposes tools/data; an MCP **client** (the agent) consumes them.
- Benefit: **N + M integrations instead of N × M.** Write the connector once, reuse it everywhere.
- On AWS: **AgentCore Gateway** turns your existing APIs and Lambda functions into MCP-compatible tools; **Strands Agents** can consume thousands of published MCP servers.

#### 🔌 Analogy — MCP is the standard wall socket

Think about what your kitchen would be like if every appliance needed its own custom wall connection. The kettle needs a round three-pin hole, the toaster a flat two-pin, the blender something proprietary. Buy a new appliance and you're calling an electrician to cut a new hole in the wall. Move house — every wall is different — and you re-wire everything from scratch. With 5 appliances and 4 houses, that's 20 custom wiring jobs.

Then someone standardizes the socket. Now the wall exposes one shape, every appliance ships with that plug, and *any* appliance works in *any* house. Buy a new blender, plug it in, done. 5 appliances + 4 houses = **9 things to build, not 20**.

- The **appliance** is a tool or data source (your CRM, a database, Slack, an internal API).
- The **house wiring** is the AI agent that wants to use it.
- The **standardized socket shape** is **MCP**.
- The appliance-side plug is an **MCP server**; the socket in the wall is the **MCP client**.
- **AgentCore Gateway** is the adapter that fits a plug onto appliances you already own — turning your existing Lambda functions and APIs into MCP tools without rewriting them.

That N×M → N+M collapse is the entire point of the protocol, and it's what an exam question about MCP is fishing for.

### Multi-agent systems

Rather than one agent doing everything, specialized agents collaborate.

| Pattern | Shape | Use when |
|---|---|---|
| **Supervisor / orchestrator** | A lead agent decomposes the task and delegates to specialist sub-agents, then assembles results | Most common enterprise pattern; clear accountability |
| **Sequential / pipeline** | Agent A's output feeds agent B, then C | Well-defined ordered stages |
| **Parallel** | Several agents work simultaneously, results merged | Independent subtasks; speed matters |
| **Hierarchical** | Supervisors of supervisors, multiple levels | Large, complex problem decomposition |
| **Peer-to-peer / collaborative** | Agents communicate directly as equals | Negotiation, debate, cross-checking |

**Why multi-agent:** specialization (each agent has a focused toolset and prompt), separation of concerns, independent scaling, and easier evaluation. **Cost:** more calls, more latency, more failure modes, harder debugging.

**Communication** between agents happens via structured messages — shared state, message passing, or an emerging protocol like **A2A** (agent-to-agent), which AgentCore Runtime supports alongside MCP.

### Workflow orchestration
Sequencing the steps: routing, branching on conditions, retries, error handling, and human-in-the-loop approval gates. Deterministic workflows (Step Functions) give predictability; agent-driven orchestration gives flexibility. Production systems often mix both.

---

## 10. GenAI use cases

| Use case | Description |
|---|---|
| **Text generation** | Drafting emails, articles, marketing copy, product descriptions |
| **Summarization** | Condensing documents, meetings, call transcripts |
| **AI assistants / chatbots** | Conversational help, internal Q&A over company knowledge |
| **Customer service agents** | Resolving tickets end to end, with tool access |
| **Code generation** | Writing, explaining, translating, and reviewing code |
| **Translation** | Language conversion with context awareness |
| **Image generation** | Marketing assets, product imagery, concept art (Nova Canvas) |
| **Video generation** | Short-form video from text (Nova Reel) |
| **Audio generation** | Speech, music, sound effects |
| **Search** | Semantic search that matches meaning rather than keywords |
| **Recommendation engines** | Natural-language, explainable recommendations |
| **Personalization** | Tailored content per user at scale |
| **Data augmentation** | Synthetic training data generation |

---

## Exam traps for this task

1. **Tokens ≠ words.** ~4 characters ≈ 1 token; ~100 tokens ≈ 75 words. Input and output are billed separately, output usually costs more.
2. **Context window covers prompt + response together**, and conversation history is re-sent (and re-billed) every turn.
3. **Embeddings capture semantic meaning**; the same embedding model must be used for indexing and querying.
4. **Multi-modal ≠ multi-lingual.**
5. **Diffusion = iterative denoising for images.** LLM = next-token prediction for text.
6. **Embedding models don't generate content** — they only produce vectors.
7. **Pre-training is done by providers**, not customers. You join the lifecycle at model selection.
8. **Context engineering ⊃ prompt engineering** — dynamic assembly of the whole payload vs. wording one instruction.
9. **MCP's value is N+M instead of N×M integrations** — one standard interface, not a model-specific feature.
10. **The model requests a tool call; your application executes it.** The FM never runs code itself.
11. **Chunk size is a tradeoff** — too big wastes tokens and dilutes relevance, too small severs meaning. Overlap protects boundaries.
12. **Smallest model that meets the quality bar** is the default answer to GenAI cost questions.
