# Amazon Bedrock

Fully managed, serverless access to foundation models from multiple providers through a single API.

→ Exam-oriented coverage lives in [Domain 2, Task 2.3](../domains/02-genai-fundamentals/03-aws-genai-infrastructure.md) (what Bedrock is, its features, pricing modes) and [Domain 3, Task 3.1](../domains/03-foundation-model-applications/01-design-considerations.md) (Knowledge Bases, Agents, inference parameters).

---

## The four API endpoints

Bedrock doesn't have one API — it has **four separate clients**, and the names look almost identical:

```
bedrock  ·  bedrock-runtime  ·  bedrock-agent  ·  bedrock-agent-runtime
```

Pick the wrong one and the operation you want simply isn't there. The split is deliberate: AWS separates **managing** resources from **using** them, and separates the **raw model** from the **packaged agent**.

### 🍽️ The restaurant analogy — decode any of the four

Ask two questions about every endpoint: **(1) Are you *managing* or *actually running a request*?** (control plane vs data plane) and **(2) Is it a *chef* (raw model) or a *robot waiter* (agent/flow)?**

| | **Manage / set up** (control plane) | **Actually run a request** (data plane) |
|---|---|---|
| **Raw model (chef)** | `bedrock` | `bedrock-runtime` |
| **Agent / flow (robot waiter)** | `bedrock-agent` | `bedrock-agent-runtime` |

- 🏢 **`bedrock` — the manager's office / menu board** *(control, base):* see which chefs are available, read specialties, check access. You manage, you don't eat. → `ListFoundationModels`, `GetFoundationModel`, model-access & custom-model management.
- 🍽️ **`bedrock-runtime` — the order window to the kitchen** *(data, base):* hand in an order (prompt), get a plate back (completion). The real invoke. → `InvokeModel`, `InvokeModelWithResponseStream`, `Converse`.
- 🛠️ **`bedrock-agent` — the kitchen design studio** *(control, agent):* build & configure the robot waiter — recipe card (prompt), recipe book (KB), tools (actions), workflow (flows). Build-time. → `CreateAgent`, `PrepareAgent`, `CreateFlow`, `CreatePrompt`, `CreateKnowledgeBase`.
- 🔔 **`bedrock-agent-runtime` — the "serve table 5" button** *(data, agent):* press go and the agent actually runs — reasons, calls tools, fetches from the KB, returns the answer. → `InvokeAgent`, `InvokeFlow`, `Retrieve`, `RetrieveAndGenerate`.

**The naming rule (decode any endpoint):`-runtime` = the *doing*** (data plane, invoke) · **no `-runtime` = the *managing*** (control plane, list/create). **`-agent` = the packaged assistant** · **no `-agent` = the raw model.** *Office = manage, kitchen window = invoke; chef = model, robot waiter = agent — and `-runtime` is always the kitchen.*

> **Where you'll feel this in practice:** `Retrieve` and `RetrieveAndGenerate` — the two Knowledge Base query APIs — live on **`bedrock-agent-runtime`**, not on `bedrock-runtime`. Querying a knowledge base is *running an agent-side request*, even though no agent is involved.

---

## TODO

- Model catalogue and provider list
- Built-in features (Knowledge Bases, Guardrails, Agents, Flows, Prompt Management, Model Evaluation, Distillation, Intelligent Prompt Routing)
- Pricing modes (On-Demand / Batch / Provisioned Throughput)
- Security and data-handling posture
