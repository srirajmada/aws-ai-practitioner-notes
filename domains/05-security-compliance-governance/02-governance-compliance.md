# Task 5.2 — Governance and compliance regulations for AI systems

> Domain 5 (14%). Service identification plus one named framework you should know cold: **the Generative AI Security Scoping Matrix**.

---

## 1. AWS services for governance and compliance

The five named in the objective, plus the ones that naturally accompany them.

| Service | What it does | Exam cue |
|---|---|---|
| **AWS Config** | Records resource configurations over time and evaluates them against **compliance rules**; flags and can remediate drift | "Is our infrastructure configured to policy?" / "detect configuration drift" |
| **AWS CloudTrail** | Logs **who did what, when** — every API call across the account | "Audit trail", "who invoked the model", "forensic investigation" |
| **AWS Artifact** | Self-service portal for **AWS's compliance reports and certifications** — SOC, ISO, PCI DSS — and agreements like the AWS BAA | "Obtain evidence for our auditor", "download the SOC 2 report" |
| **Amazon Inspector** | Automated **vulnerability scanning** of workloads, EC2, containers, and Lambda | "Find CVEs", "vulnerability management" |
| **AWS Trusted Advisor** | Recommendations across cost, performance, security, fault tolerance, and service limits | "Best-practice checks and recommendations" |
| **AWS Audit Manager** | **CHECKS whether your AWS resources comply with regulations.** Automates compliance audits, collects evidence, maps it to frameworks — and has a **prebuilt framework specifically for auditing generative AI applications built on Amazon Bedrock** | "**check if resources comply**", "audit against regulations", "automate compliance audits" |
| **AWS Well-Architected Tool** | Reviews workloads against the AWS Well-Architected Framework pillars | "Review our architecture against best practice" |
| **Amazon CloudWatch** | Metrics, logs, alarms | "Monitor and alert" |

> **The three most confused:** **Config** = *is the configuration compliant?* **CloudTrail** = *who did what?* **Artifact** = *give me AWS's compliance certificates*. **Inspector** = *what's vulnerable?* Read what the question is actually asking for.

### 🚨 AWS Artifact vs. AWS Audit Manager

These two are easy to confuse because both sit in the compliance space. **The verb separates them.**

| | **AWS Artifact** | **AWS Audit Manager** |
|---|---|---|
| **Whose compliance?** | **AWS's own** | **Yours** — your workloads and resources |
| **What it does** | **Gives you documents** — on-demand access to AWS's security and compliance reports (SOC, ISO, PCI DSS) and agreements | **Checks whether your resources comply** with regulations; automates audits and collects evidence |
| **Active or passive?** | **Passive** — a download portal | **Active** — continuously assesses and alerts |
| **The verb that signals it** | *"obtain / download / provide **documents**"* | *"**check if** resources comply", "audit", "automate compliance"* |

> 🚨 **A common trap: over-reaching for AWS Artifact.** Because it is the best-known compliance service, it gets picked for anything compliance-flavoured. **Artifact does exactly one thing: it hands you AWS's existing compliance documents.** It never checks, assesses, audits, or scopes anything.
>
> **If the verb is anything other than "obtain / download / provide documents," Artifact is the wrong answer.** Two frequent mis-picks:
>
> | If the requirement is… | The answer is… | Not Artifact, because… |
> |---|---|---|
> | *Check whether an application complies with regulations* | **AWS Audit Manager** | Artifact provides documents; it doesn't assess your resources |
> | *Identify frameworks to assess risks, governance and controls for GenAI* | **GenAI Security Scoping Matrix** | Artifact is a document portal, not a risk framework |

> 🔑 **The one-line separator:** **Artifact hands you paperwork proving AWS is compliant. Audit Manager inspects your estate to see whether you are compliant.** Read the verb before the service name.

> ⭐ **Worth knowing specifically:** Audit Manager provides a **prebuilt framework for auditing generative AI applications built on Amazon Bedrock** — tracking model usage, flagging sensitive data, and alerting on compliance. If a question pairs *Bedrock* with *regulatory audit*, that's a strong pointer.

> **Why the Bedrock-flavoured distractors fail:** **Bedrock Model Evaluation** measures model *performance*, and **Bedrock Guardrails** filters *content*. Neither checks whether your **AWS workloads** comply with regulations — they operate on the model, not the account.

> ⚠️ **Another gap in the published service list:** **AWS Audit Manager is not on it** (Management & Governance lists only CloudTrail, CloudWatch, Config, Trusted Advisor, and the Well-Architected Tool) — yet it is the right answer for auditing compliance. Worth knowing regardless.

---

## 2. The Generative AI Security Scoping Matrix

**Named explicitly in the objective — learn the five scopes and their order.** It's AWS's framework for deciding how much security responsibility you carry, based on how you're using GenAI.

> 🔑 **Typical phrasing:** *which service or **framework** helps evaluate the **level of ownership** required for an AI solution?* → **the Generative AI Security Scoping Matrix.**
>
> **"Level of ownership" is the giveaway phrase.** The distractors are all *services* rather than *frameworks* — **Inspector** (vulnerability scanning), **Macie** (PII discovery in S3), **Trusted Advisor** (best-practice recommendations). None of them classify your use case; only the Scoping Matrix does.

### ⚠️ The Scoping Matrix is broader than "ownership"

**A common misreading**, and an easy one — the framework is most often introduced with the word *"ownership."* Determining ownership is the **scoping step**, not the framework’s whole purpose.

**What it's actually for:** identifying the **security protocols and frameworks** you need for a GenAI solution — assessing the **risks, governance, and controls** of generative AI applications.

| Aspect | Detail |
|---|---|
| **Scoping step** | Classify your use case into one of the 5 scopes → tells you your **level of ownership** |
| **Then** | Points you at the **security disciplines** that matter at that scope |
| **Focus** | The **unique risks and additional security considerations of generative AI workloads** |
| **Relationship to existing practice** | **Complements** what you already do — IAM, threat modeling — rather than replacing it |

> **Both phrasings point to the same framework:**
> - *"evaluate the **level of ownership** required"* → Scoping Matrix
> - *"identify security protocols and frameworks to assess **risks, governance, and controls** of GenAI"* → Scoping Matrix

> **The distractor to separate:** the **AWS resilience lifecycle framework for generative AI** is built on the **reliability** pillar of Well-Architected and covers **how to achieve a desired level of resiliency** — recovering from disruptions, infrastructure failures, misconfigurations. **Resilience ≠ risk/governance/controls.**

### 🌍 ISO and ISO 42001 — global AI standards

**The ISO (International Organization for Standardization) is an organization, not an AWS service** — and it's the answer when a question asks about **global standards and frameworks** for AI.

- **ISO/IEC 42001** is the international standard providing **guidance to companies for managing AI systems** (an AI management system standard).
- Watch the question wording: *"which **organization or AWS service**…"* — that phrasing deliberately opens the door to a **non-AWS body**. Don't assume the answer must be an AWS service.

> **Why the AWS services all failed here:** **Trusted Advisor** gives environment recommendations (cost, performance, security), **Amazon Inspector** scans for vulnerabilities, **CloudTrail** logs API actions. **None of them provides guidance on global standards for AI systems** — that's a standards body's job, not a monitoring tool's.

> **Other standards worth recognizing:** **NIST AI RMF** (US risk management framework), **EU AI Act** (risk-tiered regulation), **ISO/IEC 27001** (information security).

| Scope | What it is | Example | Your ownership |
|---|---|---|---|
| **Scope 1 — Consumer app** | Using a public consumer GenAI service | Employees using a public chatbot | **Lowest** — you control almost nothing. Main risk: what staff paste into it |
| **Scope 2 — Enterprise app** | Using a third-party enterprise application with GenAI built in | A SaaS tool with an AI assistant, governed by an enterprise agreement | Low — vendor contract and data-handling terms are your levers |
| **Scope 3 — Pre-trained model** | You build an application on a pre-trained FM (e.g. via Amazon Bedrock) | Your app calls Bedrock with RAG over your documents | **Medium** — you own the app, the tools, customer data, and permissions |
| **Scope 4 — Fine-tuned model** | You fine-tune a third-party FM on your own data | A marketing tool tuned on your product catalogue | **High** — plus responsibility for the training data now inside the model |
| **Scope 5 — Self-trained model** | You build and train a model from scratch on data you own | An industry-specific LLM you license to others | **Highest** — you own every aspect of the model |

**The two groupings to remember:**
- **Scopes 1–2 = prebuilt applications** — you're *buying/consuming*. Risk management is largely contractual and about what data leaves your control.
- **Scopes 3–5 = self-built applications** — you're *building*. Risk management is technical and yours.

> **The principle:as the scope number rises, so does your ownership, control, and security responsibility.** A question describing "we fine-tuned a foundation model on our customer data" is **Scope 4**, and that means you now own the data-governance implications of what's baked into those weights.

---

## 3. Data governance strategies

The objective names these directly.

| Strategy | What it means |
|---|---|
| **Data lifecycle** | Manage data from creation → use → archive → deletion, with defined stages and owners |
| **Retention** | How long data is kept and when it's destroyed. Both over-retention (risk) and under-retention (non-compliance) are failures |
| **Residency** | *Where* data physically lives. Some jurisdictions require data stay in-region — this can **override your model choice**, since not every model is in every Region |
| **Logging** | Record access and activity — CloudTrail, model invocation logs, application logs |
| **Monitoring and observation** | Ongoing watch over usage, quality, cost, drift, and anomalies |
| **Classification** | Label data by sensitivity so controls can be applied proportionately |
| **Access control** | Least privilege, enforced and reviewed |
| **Quality management** | Accuracy, completeness, and freshness standards |
| **Lineage and cataloging** | Know where data came from and where it went (AWS Glue Data Catalog) |

> **Cross-Region inference caveat:** it improves throughput and availability, but **your data can be processed in other Regions** — which may conflict with a residency requirement. AWS flags this explicitly.

---

## 4. Governance processes

| Process | What it involves |
|---|---|
| **Policies** | Written, approved rules: acceptable use, approved models, data handling, human review requirements |
| **Review cadence** | Scheduled reviews — pre-deployment approval, and periodic re-review as models, data, and regulation change |
| **Review strategies** | Risk-tiered: high-risk use cases get deeper scrutiny and human oversight; low-risk ones get lighter-touch checks |
| **Governance frameworks** | Structured approaches — the **Generative AI Security Scoping Matrix**, the AWS Well-Architected Framework, NIST AI RMF, ISO/IEC 42001 |
| **Transparency standards** | Disclose AI use to users, document models (**Model Cards**), publish limitations |
| **Team training** | Staff must understand acceptable use, risks, and escalation. Named in the objective — **people are part of governance**, not just tooling |
| **Roles and accountability** | Named owners for models, data, and risk decisions |
| **Incident response** | A defined path for handling harmful output, leaks, or model failure |
| **Inventory** | Know every AI system in use — including shadow AI adopted without approval |

### Regulations worth recognizing

| Regulation | Relevance |
|---|---|
| **GDPR** | EU personal data: consent, minimization, right to erasure, right to explanation of automated decisions |
| **EU AI Act** | Risk-tiered regulation of AI systems, with obligations rising by risk category |
| **HIPAA** | US healthcare data (AWS BAA available via Artifact) |
| **PCI DSS** | Payment card data |
| **SOC 1/2/3, ISO 27001** | Assurance reports available through **AWS Artifact** |
| **CCPA** | California consumer privacy |

---

## Exam traps for this task

1. **Know the five scopes in order:** consumer app → enterprise app → pre-trained → fine-tuned → self-trained. **Responsibility rises with scope number.**
2. **Scopes 1–2 are prebuilt (buying); 3–5 are self-built (building).**
3. **Config = configuration compliance. CloudTrail = who did what. Artifact = AWS's compliance reports. Inspector = vulnerabilities. Trusted Advisor = best-practice recommendations.**
4. **AWS Artifact is where you get the SOC/ISO reports** for an auditor — this is a recurring question.
5. **Data residency can override model choice**, since model availability differs by Region.
6. **Cross-Region inference may move data across Regions** — a residency risk.
7. **Team training is an explicit governance requirement**, not an optional extra.
8. **Retention policies cut both ways** — keeping too much is as much a failure as keeping too little.
9. **Fine-tuning changes your governance posture** (Scope 3 → 4) because your data is now inside the model.
