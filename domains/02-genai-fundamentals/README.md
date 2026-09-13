# Domain 2 — Fundamentals of GenAI (24%)

**~12 of the 50 scored questions.** The second-largest domain, and the conceptual foundation for Domain 3 (28%) — together they're **52% of the exam**. Whatever you don't nail here costs you twice.

## Contents

| File | Task statement | Focus |
|---|---|---|
| [01-genai-concepts.md](01-genai-concepts.md) | 2.1 | Tokens, embeddings, chunking, transformers, model types, FM lifecycle, token pricing, context engineering, agentic AI + MCP |
| [02-capabilities-limitations.md](02-capabilities-limitations.md) | 2.2 | Advantages, hallucination and the other limitations, model selection factors, business metrics |
| [03-aws-genai-infrastructure.md](03-aws-genai-infrastructure.md) | 2.3 | Bedrock, SageMaker AI, JumpStart, Quick, Kiro, Strands Agents, AgentCore, and the cost tradeoffs |
| [flashcards.md](flashcards.md) | all | Rapid recall drilling |

Official objectives: [AWS exam guide — Domain 2 ↗](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01-domain2.html)

## The ten things most likely to be tested

1. **Tokens are the billing unit and the memory limit** — ~4 chars ≈ 1 token, input and output priced separately, history re-sent every turn.
2. **Embeddings put similar meanings close together in vector space** — the basis of semantic search and RAG.
3. **Chunking is a tradeoff** — too big dilutes relevance, too small severs meaning; overlap protects boundaries.
4. **Transformers use attention and process in parallel** — that's why large-scale pre-training became possible.
5. **Diffusion (denoising → images) vs. LLM (next-token → text) vs. embedding models (vectors, no generation).**
6. **Hallucination is inherent** — mitigate with RAG grounding, citations, validation, human review.
7. **Context engineering ⊃ prompt engineering** — dynamically assembling the whole payload, not wording one instruction.
8. **MCP turns N×M integrations into N+M** — one open standard between agents and tools.
9. **Bedrock (serverless managed FM API) vs. SageMaker AI (build and host it yourself).**
10. **Bedrock pricing: On-Demand / Batch (~50% cheaper) / Provisioned Throughput (reserved capacity).** ⚠️ Custom models can run **on-demand OR** provisioned — Provisioned Throughput is *not* mandatory for them.

## Newer content to watch

This domain absorbed most of the guide's recent changes. Older courses and practice sets will not cover:

- **Context engineering** — named as its own objective, distinct from prompt engineering.
- **MCP (Model Context Protocol)** — how agents connect to external systems.
- **Multi-agent patterns** — supervisor, sequential, parallel, hierarchical; plus memory management and orchestration.
- **Strands Agents** (open-source agent SDK) and **Amazon Bedrock AgentCore** (production agent platform, 10+ modules).
- **Amazon Quick** and **Kiro** added to the service list. ⚠️ **Amazon Q Business is NOT on that published list but is still worth knowing** — study it anyway.

## Analogies in this domain

- 🛒 **The supermarket that shelves by meaning** — embeddings, vectors, and similarity search ([01](01-genai-concepts.md))
- 🔌 **MCP is the standard wall socket** — why one protocol beats N×M custom integrations ([01](01-genai-concepts.md))
- 🚚 **Renting a van vs. leasing one** — On-Demand vs. Batch vs. Provisioned Throughput ([03](03-aws-genai-infrastructure.md))

## Study approach

1. Start with 2.1 and get the vocabulary solid — Domain 3 is unreadable without tokens, embeddings, and chunking.
2. For 2.2, practice spotting the *limitation* in a scenario. Many questions are really asking "what will go wrong here?"
3. For 2.3, focus on service boundaries (which service does what) and the three pricing modes.
4. Drill [flashcards.md](flashcards.md), then move to Domain 3 — it builds directly on this.
