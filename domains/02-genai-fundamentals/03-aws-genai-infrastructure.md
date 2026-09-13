# Task 2.3 — AWS infrastructure and technologies for building GenAI applications

> Domain 2 (24%). Service identification plus the cost/benefit story. The service list in the current guide changed significantly — **Amazon Quick, Kiro, Strands Agents, and AgentCore** are all named, and Amazon Q is gone.

---

## 1. The core services

### Amazon Bedrock — the centrepiece
**Fully managed, serverless access to foundation models from multiple providers through a single API.** If a question describes building a GenAI application on AWS without managing infrastructure, the answer is almost always Bedrock.

- **Multiple providers, one API:** Amazon (Nova, Titan), Anthropic (Claude), Meta (Llama), Mistral, Cohere, AI21, Stability AI. Switching models is a parameter change, not a rewrite.
 - *Builder's note:* "one API" is one **surface**, but four **clients** — `bedrock`, `bedrock-runtime`, `bedrock-agent`, `bedrock-agent-runtime`. The 🍽️ [restaurant analogy in services/bedrock.md](../../services/bedrock.md) decodes which is which.
- **Serverless** — no infrastructure to provision, patch, or scale.
- **Your data stays yours:** prompts and outputs are **not used to train the base models**, and traffic doesn't leave AWS.

**Built-in capabilities** (each covered in depth in Domain 3):

| Feature | Purpose |
|---|---|
| **Knowledge Bases** | Managed RAG — ingest, chunk, embed, store, retrieve, cite |
| **Guardrails** | Content filters, denied topics, PII redaction, contextual grounding checks |
| **Agents** | Managed agents that plan and call tools/APIs |
| **Model Evaluation** | Automatic and human evaluation jobs |
| **Prompt Management** | Version, test, and deploy prompts |
| **Flows** | Visual chaining of prompts, KBs, agents, and Lambda |
| **Custom Model Import / Fine-tuning / Distillation** | Adapt models to your data |
| **Intelligent Prompt Routing** | Auto-route each request to the cheapest model that can handle it |
| **Marketplace** | Access to a wider catalogue of specialized models |

### Amazon SageMaker AI
The **build-your-own** platform. Full control of training, hyperparameters, and hosting. For GenAI specifically: deploy open-weight FMs on your own endpoints, fine-tune with full control, and use SageMaker's evaluation and monitoring tooling.

> **Bedrock vs. SageMaker AI** — the most-tested comparison in this task:
>
> | | Amazon Bedrock | Amazon SageMaker AI |
> |---|---|---|
> | Model access | Managed API, provider catalogue | Any model, incl. your own weights |
> | Infrastructure | None — serverless | You choose and manage instances |
> | Control | Prompt, params, and Bedrock features | Full — training loop, container, hosting |
> | Skill required | Low — API calls | High — ML engineering |
> | Pricing | Per token (or provisioned throughput) | Per instance-hour |
> | Choose when | Speed, minimal ops, standard GenAI app | Custom models, deep control, non-Bedrock models |

### Amazon SageMaker JumpStart
A **hub of pre-trained models and prebuilt solution templates**, deployable in a few clicks. The bridge between "I want a specific open-weight model" and "I don't want to build hosting from scratch." Includes many open-source FMs.

### Amazon Quick (Quick Suite)
The evolution of **Amazon QuickSight** into a GenAI-powered BI and productivity suite (rebranded October 2025). Components: **Quick Sight** (dashboards/BI), **Quick Research**, **Quick Flows**, **Quick Automate**, **Quick Index** — reached through a chat interface.

> **Not** a model-hosting or training service. If a question is about *analyzing data, building dashboards, or asking questions of a dataset in natural language*, that's Quick.

### Kiro
An **agentic IDE** built on Code OSS using **spec-driven development**: you write a spec, the agent generates and maintains code against it, and **hooks** fire on repo events (file save, PR open) to run tests or update docs. Works with any stack or cloud.

> A **developer tool**. If the question is about *building software with an AI agent*, it's Kiro.

### Strands Agents
An **open-source SDK** (Apache 2.0, Python/TypeScript) from AWS for building agents with a **model-driven approach**: rather than hard-coding a task flow, you give the agent a prompt and a list of tools, and let the model's reasoning handle planning and tool selection. Supports Bedrock models plus Anthropic, Llama, Ollama, OpenAI via LiteLLM, and consumes published **MCP servers** as tools. Used in production by AWS teams themselves.

### Amazon Bedrock AgentCore
A **platform for running agents in production securely at scale** — framework-agnostic and model-agnostic. It works with Strands Agents, LangGraph, CrewAI, LlamaIndex, OpenAI Agents SDK, and with models inside or outside Bedrock.

Modular services you can use together or independently:

| Module | What it does |
|---|---|
| **Harness** | A managed agent loop — define and invoke an agent with a single API call (model + system prompt + tools inline). Each session runs in an isolated microVM |
| **Runtime** | Secure serverless runtime for agents — session isolation, fast cold starts, long-running async agents. Supports MCP and A2A |
| **Memory** | Managed short-term (in-session) and long-term (cross-session) memory |
| **Gateway** | Turns your APIs and Lambda functions into **MCP-compatible tools**; connects to existing MCP servers |
| **Identity** | Agent identity and access management; works with Cognito, Okta, Entra ID, Auth0 |
| **Policy** | Deterministic guardrails on agent behaviour — rules in natural language or **Cedar**, enforced on every tool call |
| **Code Interpreter** | Isolated sandbox for agents to execute code |
| **Browser** | Managed cloud browser so agents can navigate sites and fill forms |
| **Observability** | OpenTelemetry-based tracing and debugging of each agent step |
| **Evaluations** | Automated assessment of agent task performance |
| **Optimization** | Continuous improvement — AI-generated config recommendations plus A/B testing of prompts and tool descriptions |
| **Payments** | Lets agents pay for paid APIs and MCP servers, with spending limits |
| **Registry** | Catalogue for discovering agents, MCP servers, and tools across the org |

> That's 13 modules. For a foundational exam you're unlikely to be asked to enumerate them — know that AgentCore is **modular** (use any piece independently), and recognize the headline ones: **Runtime** (run it), **Memory** (remember), **Gateway** (tools via MCP), **Identity** and **Policy** (secure it — both named in Domain 5), **Observability** (watch it).

> **The division of labour to remember:** **Strands Agents** = the SDK you *build* the agent with. **AgentCore** = the managed platform you *run and operate* it on. **Bedrock Agents** = the fully managed agent offering inside Bedrock itself. Domain 5 references **AgentCore Identity** and **Policy** specifically for securing agents.

---

## 2. Advantages of using AWS GenAI services

| Advantage | How AWS delivers it |
|---|---|
| **Accessibility** | Console, CLI, SDK, and no-code paths; playgrounds for experimenting |
| **Lower barrier to entry** | No ML expertise, no GPUs, no training run needed to ship something |
| **Efficiency** | Managed RAG, guardrails, evaluation, and agents you'd otherwise build yourself |
| **Cost-effectiveness** | Pay-per-token, no idle infrastructure, cheaper options (batch, distillation, caching) |
| **Speed to market** | Prototype in hours; the undifferentiated heavy lifting is done for you |
| **Choice of model** | Multiple providers behind one API — avoids lock-in to a single model |
| **Meets business objectives** | Scale, reliability, and integration with the rest of your AWS estate |

## 3. Benefits of AWS infrastructure

| Benefit | Detail |
|---|---|
| **Security** | Data encrypted in transit and at rest; IAM access control; PrivateLink for private connectivity; your prompts/outputs are **not used to train base models** |
| **Compliance** | SOC, ISO, HIPAA eligibility, GDPR support; evidence via AWS Artifact; regional data residency |
| **Responsibility** | Guardrails, AI Service Cards, watermarking on Nova Canvas/Reel, bias tooling in SageMaker Clarify |
| **IP indemnification** | **Uncapped IP indemnity** for copyright claims on the **output** of Amazon-developed models (**Titan, Nova**) via Bedrock — AWS defends and covers you. Conditional on responsible use: don't input infringing data, **don't disable content filters**. The exam calls this **"infringement protection capabilities"** → [Domain 4](../04-responsible-ai/01-responsible-ai-development.md) |
| **Safety** | Content filtering, denied topics, contextual grounding checks, toxicity detection |
| **Reliability** | Multi-AZ, global regions, managed availability |
| **The shared responsibility model** | AWS secures the infrastructure; **you** secure your data, prompts, access policies, and how the output is used |

---

## 4. Cost tradeoffs

The objective lists these by name — each is a lever with a cost consequence.

| Lever | Tradeoff |
|---|---|
| **Token-based pricing** | Pay only for use, no idle cost — but cost scales linearly with volume and prompt length |
| **Provisioned Throughput** | Reserve guaranteed capacity for a time commitment; discounted hourly rate, but **you pay whether you use it or not** |
| **Model size** | Larger models cost multiples more per token and are slower |
| **Custom models** | Fine-tuning/distillation costs training money up front. To serve the result you choose **either an on-demand custom model deployment (per-token, no provisioned compute) or Provisioned Throughput** — it is **not** Provisioned-Throughput-only |
| **Responsiveness / latency** | Faster responses mean smaller models or reserved capacity — both are cost decisions |
| **Availability & redundancy** | Multi-region failover improves resilience and raises cost |
| **Regional coverage** | Not every model is in every Region; cross-Region inference improves availability and throughput, and data residency rules may restrict you |
| **Performance** | Higher throughput needs provisioned capacity or higher quotas |

### Bedrock pricing modes

| Mode | How it works | Best for |
|---|---|---|
| **On-Demand** | Pay per input/output token, no commitment | Development, POCs, variable or unpredictable traffic |
| **Batch** | Submit a file of prompts, get a file back — **~50% cheaper than on-demand** | Large offline jobs with no live user waiting |
| **Provisioned Throughput** | Reserve model units for a term (no commitment / 1 month / 6 months — longer term, deeper discount) | Steady high volume, guaranteed throughput. ⚠️ **NOT required for custom models** — they can also run on-demand |

### 🚚 Analogy — Renting a van vs. leasing one

You run a small shop that occasionally ships furniture.

**On-Demand** is calling a van hire service each time an order comes in. You pay per trip. Some weeks you make two deliveries, some weeks forty — you only ever pay for the trips you take. If you make zero deliveries in January, January costs nothing. The catch: on a chaotic Saturday when every shop in town wants a van, you're in the queue with everyone else, and there's no guarantee one is free the moment you need it.

**Batch** is waiting until Thursday and sending everything in one big lorry at the off-peak rate. Roughly half the price per item. The trade: nothing ships until Thursday. Fine for restocking a warehouse, useless for a customer waiting at home.

**Provisioned Throughput** is leasing your own van for a year. There's a van in your yard every morning, guaranteed, no queue, and the longer the lease you sign, the cheaper the monthly rate. The trade is unforgiving: **you pay for that van in the weeks you don't drive it at all.** It only makes sense once your delivery volume is steady and high enough that per-trip hire would cost more.

And if you have a **custom-built van** — modified shelving for your specific product, i.e. a **custom fine-tuned model** — you have **both options too**: lease it for the year (**Provisioned Throughput**) *or* pay per trip (**on-demand custom model deployment**). ⚠️ *This used to be lease-only, and older material — including an earlier version of these notes — still says so. It's no longer true.*

| In the yard | In Bedrock |
|---|---|
| Per-trip van hire | **On-Demand** — pay per token, no commitment |
| Thursday's off-peak lorry run | **Batch** — ~50% cheaper, offline only |
| Leasing a van for the year | **Provisioned Throughput** — reserved capacity, term commitment |
| Your custom-fitted van | **Custom model** — needs reserved capacity to serve |

### Cost optimization levers, cheapest first

1. **Choose the smallest sufficient model** — the single biggest lever
2. **Shorten prompts and cap output length** — you pay for both
3. **Prompt caching** — reuse a repeated prefix at reduced cost
4. **Batch inference** for anything that doesn't need to be live (~50% off)
5. **Intelligent Prompt Routing** — cheap model for easy requests, big model only when needed
6. **RAG instead of fine-tuning** — no training cost, and easier to keep current
7. **Model distillation** — train a small cheap model to imitate a large one
8. **Provisioned Throughput** only once volume is steady and high
9. **Monitor** with CloudWatch, Cost Explorer, and Budgets

---

## Exam traps for this task

1. **Bedrock = serverless managed FM API. SageMaker AI = build/host it yourself.** "Minimal operational overhead" → Bedrock.
2. **Amazon Quick is BI and automation**, not model training — and it's the renamed QuickSight.
3. **Kiro is an IDE.** Not a hosting, data, or model service.
4. **Strands Agents builds agents; AgentCore runs them.** SDK vs. platform.
5. **AgentCore is framework- and model-agnostic** — it works with LangGraph and CrewAI and non-Bedrock models, not only AWS ones.
6. **Batch inference is ~50% cheaper** — the standard answer for large offline workloads.
7. **Provisioned Throughput costs money while idle.** Wrong answer for spiky or low volume; right answer for steady high volume and custom models.
8. **Your prompts and outputs are not used to train the base models** — the standard reassurance for a data-privacy question about Bedrock.
9. **Not every model is available in every Region.** Regional coverage is a real constraint, and data residency can override model preference.
10. **Amazon Q Business IS tested**, despite being absent from the published in-scope services list. It's the answer for *"summarize documents and answer questions, no coding experience"* — see [Domain 1, Task 1.2](../01-ai-ml-fundamentals/02-practical-use-cases.md).
