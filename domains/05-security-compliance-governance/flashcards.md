# Domain 5 — Flashcards

Cover the right column and work down. Anything you miss twice, go back to the source note.

---

## Task 5.1 — Securing AI systems

| Prompt | Answer |
|---|---|
| **The shared responsibility model in one line** | AWS secures **of** the cloud (infrastructure); you secure **in** the cloud (your data, IAM, configuration) |
| Under shared responsibility, what's yours in a Bedrock app | Who can call the model, what data you send, whether guardrails are on, how output is used, logging and retention |
| Which side do most real breaches come from | **The customer side** — public buckets, over-permissive IAM, credentials in prompts |
| What does AWS PrivateLink do for Bedrock | Private connectivity from your VPC **without traversing the public internet** |
| What does Amazon Macie do | Discovers and classifies **sensitive data and PII in S3** — discovery, not runtime filtering |
| Macie vs. Guardrails | Macie = **discovery** of PII at rest in S3. Guardrails = **runtime enforcement** on prompts and responses |
| What is AgentCore **Identity** | Identity, access, and authentication **for agents** — works with Cognito, Okta, Entra ID, Auth0 |
| What is AgentCore **Policy** | Deterministic rules constraining what an agent may do — natural language or **Cedar** — enforced on **every tool call** |
| Identity vs. Policy in one line | Identity = **who the agent is**. Policy = **what it's allowed to do** |
| Where should credentials live | **AWS Secrets Manager** — never in prompts or code |
| IAM best practice for access | **Roles**, not long-lived access keys; **least privilege** |
| What captures who invoked which model when | **AWS CloudTrail** |
| What captures the actual prompts and responses | **Bedrock model invocation logging** (to S3 or CloudWatch) |
| The risk created by invocation logging | Those logs now contain whatever sensitive data was in the prompts — they need encryption, access control, and retention limits |
| Name five AI-specific security concerns | Prompt injection, data leakage, toxicity, output validation failures, model/prompt theft (also poisoning) |
| Four prompt injection mitigations | Delimiters, input validation, Guardrails **prompt attack** filter, least-privilege tool access |
| Rule about model output | **Never treat it as a trusted command** — validate before acting, never execute generated code unsandboxed |
| Four secure data engineering practices | Assess data quality, data access control, data integrity, privacy-enhancing technologies |
| **Name five privacy-enhancing technologies (PETs)** | Anonymization, pseudonymization, masking/redaction, differential privacy, synthetic data (also tokenization, encryption) |
| Anonymization vs. pseudonymization | Anonymization **irreversibly** removes identifiers. Pseudonymization replaces them with tokens, **reversible with a key** |
| What is differential privacy | Adding statistical noise so individuals can't be re-identified from results |
| What is data lineage | The traceable history of data — origin, transformations, destinations |
| What service catalogs datasets | **AWS Glue Data Catalog** |
| Why does data origin matter for security | You can't prove your right to use data, honour deletion requests, investigate leaks, or defend outputs without it |
| **The primary defence against hallucination** | **RAG grounding** — retrieve authoritative passages and require the answer to come from them |
| Which Guardrails feature automatically detects hallucination | **Contextual grounding checks** — flags ungrounded or irrelevant responses |
| What is confidence scoring used for | Surfacing model/retrieval confidence so low-confidence answers get routed to review or refusal |
| Name six grounding/hallucination controls | RAG grounding, citations, contextual grounding checks, output validation, confidence scoring, human-in-the-loop (also Automated Reasoning checks, refusal instructions) |
| Does lowering temperature prevent hallucination | **No** — it makes the model wrong *consistently* |

---

## Task 5.2 — Governance and compliance

| Prompt | Answer |
|---|---|
| **AWS Config** — what and when | Records resource configurations and evaluates them against **compliance rules**. "Is our configuration compliant / has it drifted?" |
| **AWS CloudTrail** — what and when | Logs **who did what, when** across API calls. "Audit trail", "forensic investigation" |
| **AWS Artifact** — what and when | Self-service portal for **AWS's compliance reports** (SOC, ISO, PCI DSS) and agreements like the BAA. "Get evidence for our auditor" |
| **Amazon Inspector** — what and when | Automated **vulnerability scanning** of EC2, containers, Lambda. "Find CVEs" |
| **AWS Trusted Advisor** — what and when | Best-practice recommendations across cost, performance, security, fault tolerance, service limits |
| Auditor asks for your SOC 2 report — where do you go | **AWS Artifact** |
| **"Which service CHECKS IF AWS resources comply with regulations?"** → | **AWS Audit Manager** — Artifact only *provides documents*, it doesn't *check* anything |
| 🔑 **Artifact vs. Audit Manager** | **Artifact hands you paperwork proving AWS is compliant. Audit Manager inspects YOUR resources to see whether YOU are compliant** |
| What GenAI-specific capability does Audit Manager have? | A **prebuilt framework for auditing generative AI applications built on Amazon Bedrock** — tracks model usage, flags sensitive data, alerts on compliance |
| **The five scopes of the GenAI Security Scoping Matrix** | 1 Consumer app → 2 Enterprise app → 3 Pre-trained model → 4 Fine-tuned model → 5 Self-trained model |
| **"Which AWS service or FRAMEWORK evaluates the level of ownership required for an AI solution?"** → | **The Generative AI Security Scoping Matrix.** "**Level of ownership**" is the giveaway phrase |
| **"Identify frameworks to assess RISKS, GOVERNANCE and CONTROLS of GenAI"** → | **Also the Generative AI Security Scoping Matrix** — it's broader than ownership; ownership is just the scoping step |
| **Which ORGANIZATION publishes global AI standards?** | **ISO** — and **ISO/IEC 42001** is the international standard for **managing AI systems**. Note the answer is a standards body, **not an AWS service** |
| **"MOST securely prevents prompt injection"** → | **Salted sequence tags** — a random session-specific suffix on your instruction tags (`<instructions_a7f3k9>`) so attackers can't forge or close them |
| Why don't word/content filters stop prompt injection? | They catch **bad content**. Injection is about **input being mistaken for instructions** — only structurally identifiable instructions fix that |
| 🚨 A common trap | **Over-reaching for AWS Artifact.** It only **hands you AWS's compliance documents**. If the verb isn't "obtain/download documents," Artifact is wrong |
| Why aren't Inspector/Macie/Trusted Advisor the answer there? | They're **services**, not frameworks, and none classifies your use case. Inspector = vulnerabilities, Macie = PII in S3, Trusted Advisor = best-practice recommendations |
| Scope 1 vs. Scope 2 | Scope 1 = public consumer GenAI service. Scope 2 = third-party **enterprise** app with GenAI built in |
| Scope 3 | You build an app on a **pre-trained** FM (e.g. via Bedrock) — you own the app, tools, customer data, permissions |
| Scope 4 | You **fine-tune** a third-party FM on your own data |
| Scope 5 | You build and **train from scratch** — you own every aspect |
| The two groupings | **Scopes 1–2 = prebuilt** (buying/consuming). **Scopes 3–5 = self-built** (building) |
| **The principle behind the matrix** | As the scope number rises, so does your ownership, control, and **security responsibility** |
| "We fine-tuned an FM on our customer data" — which scope | **Scope 4** — and your data is now inside the model weights |
| Name six data governance strategies | Data lifecycle, retention, residency, logging, monitoring/observation, classification (also access control, quality, lineage) |
| What is data residency, and why can it override model choice | Where data physically lives. Some jurisdictions require in-Region storage — **and not every model is available in every Region** |
| Risk of cross-Region inference | Your data **can be processed in other Regions**, which may conflict with residency requirements |
| Why is over-retention a failure | Keeping data longer than needed is a risk and often a compliance breach — both over- and under-retention fail |
| Name five governance processes | Policies, review cadence, review strategies, governance frameworks, team training (also roles/accountability, incident response, inventory) |
| Which governance requirement is about people, not tools | **Team training** — named explicitly in the objective |
| What is a risk-tiered review strategy | High-risk use cases get deeper scrutiny and human oversight; low-risk ones get lighter checks |
| Name four AI governance frameworks | **Generative AI Security Scoping Matrix**, AWS Well-Architected Framework, NIST AI RMF, ISO/IEC 42001 |
| GDPR rights relevant to AI | Consent, data minimization, right to erasure, right to explanation of automated decisions |
| What is the EU AI Act | Risk-tiered regulation of AI systems — obligations rise with the risk category |
| What is shadow AI | AI tools adopted without approval — why an **inventory** of AI systems is a governance requirement |

---

## Rapid-fire

| Prompt | Answer |
|---|---|
| Domain 5 weighting | 14% (~7 of 50 scored questions) |
| "Private connection to Bedrock, no public internet" → | **AWS PrivateLink** |
| "Find PII sitting in our S3 buckets" → | **Amazon Macie** |
| "Block harmful output and mask PII at runtime" → | **Bedrock Guardrails** |
| "Who called the model last Tuesday?" → | **AWS CloudTrail** |
| "Download the ISO 27001 certificate" → | **AWS Artifact** |
| "Are our resources configured to policy?" → | **AWS Config** |
| "Scan our containers for vulnerabilities" → | **Amazon Inspector** |
| "Control what an agent is permitted to do" → | **AgentCore Policy** |
| "Make sure answers stay grounded in our documents" → | **RAG + contextual grounding checks** |
