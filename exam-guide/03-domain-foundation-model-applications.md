# Domain 3: Applications of Foundation Models (28%)

Official objectives, transcribed from the [AWS exam guide](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01-domain3.html).
Study notes live in [domains/03-foundation-model-applications/](../domains/03-foundation-model-applications/).

**Weighting:** 28% of scored content — roughly 14 of the 50 scored questions. **The largest domain on the exam.**

---

## Task Statement 3.1: Describe design considerations for applications that use foundation models (FMs).

Objectives:

- Identify selection criteria to choose FMs (for example, cost, modality, latency, multi-lingual, model size, model complexity, customization, input/output length, prompt caching).
- Describe the effect of inference parameters on model responses (for example, temperature, input/output length).
- Define Retrieval Augmented Generation (RAG) and describe its business applications (for example, Amazon Bedrock Knowledge Bases).
- Identify AWS services that help store embeddings within vector databases (for example, Amazon OpenSearch Service, Amazon Aurora, Amazon Neptune, Amazon RDS for PostgreSQL).
- Explain the cost tradeoffs of various approaches to FM customization (for example, pre-training, fine-tuning, in-context learning, RAG, model distillation).
- Define the role of AI agents and describe AI agents' business applications.

→ Notes: [01-design-considerations.md](../domains/03-foundation-model-applications/01-design-considerations.md)

---

## Task Statement 3.2: Choose effective prompt engineering techniques.

Objectives:

- Define the concepts and constructs of prompt engineering (for example, context, instruction, negative prompts).
- Define techniques for prompt engineering (for example, chain-of-thought, zero-shot, single-shot, few-shot, prompt templates).
- Identify and describe the benefits and best practices for prompt engineering (for example, response quality improvement, experimentation, guardrails, discovery, specificity and concision, using multiple comments).
- Define potential risks and limitations of prompt engineering (for example, exposure, poisoning, hijacking, jailbreaking).
- Describe prompt versioning and management strategies that use Amazon Bedrock Prompt Management.

→ Notes: [02-prompt-engineering.md](../domains/03-foundation-model-applications/02-prompt-engineering.md)

---

## Task Statement 3.3: Describe the training and fine-tuning process for FMs.

Objectives:

- Describe the key elements of training an FM (for example, pre-training, fine-tuning, continuous pre-training, distillation).
- Define methods for fine-tuning an FM (for example, instruction tuning, adapting models for specific domains, transfer learning, continuous pre-training).
- Describe how to prepare data to fine-tune an FM (for example, data curation, governance, size, labeling, representativeness, reinforcement learning from human feedback [RLHF]).

→ Notes: [03-training-fine-tuning.md](../domains/03-foundation-model-applications/03-training-fine-tuning.md)

---

## Task Statement 3.4: Describe methods to evaluate FM performance.

Objectives:

- Determine approaches to evaluate FM performance (for example, human-in-the-loop evaluation, benchmark datasets, Amazon Bedrock Model Evaluation).
- Identify relevant metrics to assess FM performance (for example, Recall-Oriented Understudy for Gisting Evaluation [ROUGE], Bilingual Evaluation Understudy [BLEU], BERTScore, LLM-as-a-judge).
- Determine whether an FM effectively meets business objectives (for example, productivity, user engagement, task engineering).
- Identify approaches to evaluate the performance of applications built with FM (for example, RAG, agents, workflows).
- Identify business objective alignment metrics for AI applications (for example, task completion rate, user satisfaction, cost per interaction).

→ Notes: [04-fm-evaluation.md](../domains/03-foundation-model-applications/04-fm-evaluation.md)
