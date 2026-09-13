# Domain 2 — Flashcards

Cover the right column and work down. Anything you miss twice, go back to the source note.

---

## Task 2.1 — GenAI concepts

| Prompt | Answer |
|---|---|
| What is a token | The unit an LLM reads/writes — roughly a word-piece. ~4 characters ≈ 1 token; ~100 tokens ≈ 75 words |
| Are input and output tokens priced the same | No — billed separately, and **output usually costs more** |
| What is the context window | Max tokens the model can consider at once — **prompt + response together** |
| Why does a long chat get more expensive each turn | Conversation history is re-sent with every request, so you re-pay for the whole transcript |
| What is an embedding | A numeric vector representing meaning; semantically similar items land close together in vector space |
| What is cosine similarity used for | Measuring how close two vectors are — the basis of similarity search |
| Why must the same embedding model be used for docs and queries | Different models produce different vector spaces; mixing them breaks retrieval |
| What is chunking and why | Splitting documents into passages so they fit the context window and retrieve precisely |
| Chunks too large vs. too small | Too large = wasted tokens, diluted relevance. Too small = meaning severed mid-thought |
| Why use chunk overlap | Stops an idea being cut in half at a chunk boundary |
| What architecture underlies modern LLMs | The **transformer** (2017), built on the **attention** mechanism |
| Why did transformers beat RNNs/LSTMs | **Parallel** processing of the whole sequence + direct long-range context → trainable at massive scale |
| What does attention do | Weighs how much every other token matters when processing a given token (resolves "it" in a sentence) |
| Multi-modal vs. multi-lingual | Multi-modal = multiple data types (text/image/audio/video). Multi-lingual = multiple languages |
| **What is a modality?** | A **type of data**: text, image, audio, video |
| **"Interpret contents FROM an image"** → which model type? | **Multimodal model** — the word **"from"** means the image is the **INPUT** |
| **Why can't an LLM interpret an image?** | It's trained purely on **text** — there is **no pathway for pixels to enter it**. Like a brilliant clerk with no eyes |
| **Why can't a diffusion model interpret an image?** | **Wrong direction.** Diffusion goes **text → image** (it *creates* images). Interpreting needs **image → text** |
| 🔑 The one question that decides it | **Is the image the INPUT or the OUTPUT?** Output → diffusion. Input → multimodal |
| Which Nova models are multimodal? | **Lite, Pro, Premier** (text+image+video → text). **Micro** is text-only; **Canvas** makes images; **Reel** makes video |
| Can Nova Canvas interpret an image? | **No** — Canvas *generates* images. To *read* one you need Lite/Pro/Premier |
| How do diffusion models work | Start from random noise and iteratively **denoise** toward the prompt — used for images/video |
| Diffusion vs. LLM in one line | Diffusion = denoising → images. LLM = next-token prediction → text |
| What does an embedding model NOT do | Generate content — it only produces vectors |
| The seven FM lifecycle stages | Data selection → model selection → pre-training → fine-tuning → evaluation → deployment → feedback |
| Which lifecycle stage do customers usually join at | **Model selection** (stage 2). Pre-training is done by providers |
| Name five drivers of token cost | Prompt length, response length, model choice, call volume, conversation history (also RAG context) |
| Default answer to a GenAI cost question | Use the **smallest model that meets the quality bar** |
| Four ways to cut token cost | Prompt caching, batch inference (~50% off), model distillation, shorter prompts/outputs (also prompt routing) |
| What is context engineering | Designing systems that **dynamically assemble** the optimal information for the model — treating the context window as a workspace |
| Context engineering vs. prompt engineering | Prompt engineering = wording one instruction (static). Context engineering = assembling the whole payload per request (dynamic). Prompt engineering is a subset |
| Name five components of a context payload | System prompt, few-shot examples, user query, user/profile data, retrieved RAG documents, conversation memory, tool definitions |
| The four capabilities that make AI "agentic" | Autonomy/planning, tool use, memory, orchestration |
| In tool use, who executes the tool | **Your application / the agent runtime.** The model only *requests* the call with structured arguments |
| Short-term vs. long-term memory | Short-term = within one session (multi-turn coherence). Long-term = persists across sessions (preferences, facts) |
| What is MCP | **Model Context Protocol** — an open standard for connecting agents to external tools and data sources |
| The core value of MCP | **N + M integrations instead of N × M** — build the connector once, any compatible agent can use it |
| MCP server vs. client | Server *exposes* tools/data; client (the agent) *consumes* them |
| Which AWS service turns your APIs and Lambdas into MCP tools | **AgentCore Gateway** |
| Name four multi-agent patterns | Supervisor/orchestrator, sequential/pipeline, parallel, hierarchical (also peer-to-peer) |
| Most common enterprise multi-agent pattern | **Supervisor** — a lead agent delegates to specialists and assembles results |
| Costs of going multi-agent | More calls, higher latency, more failure modes, harder debugging |
| What is A2A | Agent-to-agent protocol for inter-agent communication; supported by AgentCore Runtime alongside MCP |

---

## Task 2.2 — Capabilities and limitations

| Prompt | Answer |
|---|---|
| GenAI's headline advantage | **Adaptability** — one model, many tasks, no retraining |
| Name four advantages of GenAI | Adaptability, responsiveness, conversational capability, content generation (also speed to market, personalization at scale) |
| The four limitations named in the objective | Hallucination, interpretability, inaccuracy, nondeterminism |
| What is a hallucination | Confident, fluent, plausible output that is factually wrong — a consequence of predicting *likely* tokens, not *true* ones |
| Five mitigations for hallucination | RAG grounding, citations to source, lower temperature, output validation, human review (also guardrail grounding checks) |
| What is nondeterminism | The same prompt can produce different outputs on different calls |
| What does knowledge cutoff mean, and the fix | No awareness of events after training. Fix with **RAG or tool use**, not fine-tuning |
| Does fine-tuning add fresh facts | No — fine-tuning teaches style, format, and domain behaviour. Use RAG for current facts |
| Why is GenAI bad at arithmetic | It pattern-completes rather than calculates — give it a calculator tool |
| Which requirement pushes you away from GenAI | Needing explainable, auditable, reproducible, guaranteed-correct answers |
| Name six model-selection factors | Modality, capabilities, performance bar, latency, cost, model size/complexity (also context window, customization, compliance, region) |
| The standard model-selection method | Define the quality bar → try the smallest cheapest model → measure → escalate only on failure |
| What is Intelligent Prompt Routing | Bedrock feature that auto-routes each request to the cheapest model that can handle it |
| What is cross-domain performance | How well an FM performs across *different* subject areas — a key FM advantage, and exact AWS exam vocabulary |
| Name six business metrics for GenAI | ROI, efficiency/productivity, conversion rate, ARPU, CLV, cost per interaction (also CSAT, deflection rate, adoption) |
| Key metric for an agent | **Task completion rate** |
| Model metrics vs. business metrics | "Is the model good?" vs. "Was the initiative worth it?" — read which the question asks |

---

## Task 2.3 — AWS infrastructure

| Prompt | Answer |
|---|---|
| What is Amazon Bedrock | Fully managed **serverless** access to FMs from multiple providers via one API |
| Are your Bedrock prompts used to train the base models | **No** — and traffic doesn't leave AWS. The standard answer to a data-privacy question |
| Name six Bedrock built-in features | Knowledge Bases, Guardrails, Agents, Model Evaluation, Prompt Management, Flows (also Distillation, Prompt Routing, Marketplace) |
| Bedrock vs. SageMaker AI | Bedrock = managed API, serverless, per-token, low skill bar. SageMaker AI = full control, your instances, per-instance-hour, ML expertise needed |
| What is SageMaker JumpStart | Hub of pre-trained models and prebuilt solution templates, deployable in a few clicks |
| What is Amazon Quick | The Oct 2025 evolution of QuickSight — GenAI-powered BI/productivity suite (Quick Sight, Research, Flows, Automate, Index). **Not** a training service |
| What is Kiro | Agentic IDE on Code OSS using **spec-driven development**, with hooks on repo events. A developer tool |
| What is Strands Agents | Open-source (Apache 2.0) agent SDK from AWS using a **model-driven** approach — give it a prompt and tools, the model plans. Consumes MCP servers |
| What is Bedrock AgentCore | Framework- and model-agnostic platform for running agents in production securely at scale |
| Strands vs. AgentCore vs. Bedrock Agents | Strands = SDK you **build** with. AgentCore = platform you **run/operate** on. Bedrock Agents = fully managed agent offering inside Bedrock |
| Name six AgentCore modules | Runtime, Memory, Gateway, Identity, Policy, Observability (also Code Interpreter, Browser, Evaluations, Registry) |
| Which AgentCore module enforces rules on tool calls | **Policy** — natural language or Cedar, checked on every tool call |
| Which AgentCore module handles agent auth | **Identity** — works with Cognito, Okta, Entra ID, Auth0 |
| Is AgentCore AWS-only | No — works with LangGraph, CrewAI, LlamaIndex, OpenAI Agents SDK, and models outside Bedrock |
| The three Bedrock pricing modes | **On-Demand**, **Batch**, **Provisioned Throughput** |
| How much cheaper is Batch | **~50%** vs. on-demand |
| When is Provisioned Throughput right | Steady, high, predictable volume. ⚠️ It is **NOT required** for custom models |
| **Can a customized Bedrock model run On-Demand?** | **Yes.** Serve it via an **on-demand custom model deployment** OR Provisioned Throughput. Reject any option saying **"only** in Provisioned Throughput mode" |
| Does On-Demand require a term commitment? | **No.** On-Demand has **no** commitment — term commitments belong to **Provisioned Throughput** |
| The danger of Provisioned Throughput | You pay for reserved capacity **even when idle**. Wrong choice for spiky/low traffic |
| Provisioned Throughput commitment options | No commitment, 1 month, or 6 months — longer term, deeper discount |
| Under the shared responsibility model, what's yours in GenAI | Your data, prompts, access policies, and how you use the output. AWS secures the infrastructure |
| Name four benefits of AWS infrastructure for GenAI | Security, compliance, responsibility, safety (per the objective's own wording) |
| Nine cost-optimization levers, first one first | Smallest sufficient model → shorter prompts/outputs → prompt caching → batch → prompt routing → RAG over fine-tuning → distillation → provisioned throughput at scale → monitor |

---

## Rapid-fire numbers

| Prompt | Answer |
|---|---|
| Domain 2 weighting | 24% (~12 of 50 scored questions) |
| Domains 2 + 3 combined | **52% of the exam** |
| Tokens per 100 English words | ~133 tokens (100 tokens ≈ 75 words) |
| Characters per token, roughly | ~4 |
| Batch inference discount | ~50% |
| Nova Premier context window | 1 million tokens |
| Year the transformer paper was published | 2017 ("Attention Is All You Need") |
