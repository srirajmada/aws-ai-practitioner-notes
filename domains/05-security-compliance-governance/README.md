# Domain 5 — Security, Compliance, and Governance for AI Solutions (14%)

**~7 of the 50 scored questions.** Largely standard AWS security and governance applied to AI, plus AI-specific threats. If you've done any other AWS certification, a good portion of this is already familiar — the new parts are prompt injection, agent security, and the Scoping Matrix.

## Contents

| File | Task statement | Focus |
|---|---|---|
| [01-securing-ai-systems.md](01-securing-ai-systems.md) | 5.1 | Shared responsibility, IAM/encryption/PrivateLink/Macie, AgentCore Identity and Policy, AI-specific threats, secure data engineering, data lineage, hallucination grounding |
| [02-governance-compliance.md](02-governance-compliance.md) | 5.2 | Config/CloudTrail/Artifact/Inspector/Trusted Advisor, **the Generative AI Security Scoping Matrix**, data governance, governance processes |
| [flashcards.md](flashcards.md) | both | Rapid recall drilling |

Official objectives: [AWS exam guide — Domain 5 ↗](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01-domain5.html)

## The ten things most likely to be tested

1. **Shared responsibility model** — AWS secures *of* the cloud, you secure *in* the cloud.
2. **The five scopes of the Generative AI Security Scoping Matrix**, and that responsibility rises with the number.
3. **Config vs. CloudTrail vs. Artifact vs. Inspector** — the four most-confused governance services.
4. **AWS Artifact is where you get SOC/ISO reports** for auditors.
5. **PrivateLink** for private connectivity to Bedrock without the public internet.
6. **Macie finds PII in S3** (discovery); **Guardrails filters at runtime** (enforcement).
7. **AgentCore Identity (who the agent is) vs. Policy (what it may do).**
8. **RAG grounding is the primary hallucination defence**; contextual grounding checks automate detection.
9. **Never put secrets or PII in prompts** — and remember invocation logs capture them.
10. **Data residency can override model choice**, and cross-Region inference may move data.

## Cross-domain connections

- **Bedrock Guardrails** appears in Domain 4 (responsible AI) and here (security control). Same feature, two lenses.
- **Prompt injection and jailbreaking** are defined in Domain 3 (3.2) and defended here.
- **Hallucination** runs through the whole exam: defined in Domain 2, mitigated in Domain 3 (RAG), governed in Domain 4, detected here.
- **SageMaker Model Cards** serve transparency in Domain 4 and data-origin documentation here.
- **AgentCore** modules introduced in Domain 2 (2.3) return here as security controls.

## Study approach

1. If you know the shared responsibility model and core AWS security services, start with the **Scoping Matrix** — it's the genuinely new thing.
2. Drill the four-way service distinction (Config / CloudTrail / Artifact / Inspector) until instant.
3. The AI-specific content (prompt injection, data leakage, grounding) overlaps heavily with Domains 3 and 4 — study them together rather than separately.
