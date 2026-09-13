# Amazon Personalize

> **The recommendation service.** If a question describes recommending items, content, or products to users — Personalize is almost certainly the answer.

---

## What it is

A **fully managed AI service that generates personalized item recommendations for your users** — built on the same technology behind Amazon.com's recommendations.

- **No ML expertise required.** You provide data; AWS trains and hosts the model.
- **Accessed directly through an API** — this is why it wins "least operational overhead" questions.
- **Real-time.** Recommendations adapt to a user's behaviour as it happens, not just to historical batches.

## What you feed it

| Dataset | Contains |
|---|---|
| **Users** | User attributes — age, location, subscription tier |
| **Items** | Item attributes — genre, category, price, publication date |
| **Interactions** | The events that matter most — clicks, views, purchases, watch time |

## What it's for

| Use case | Example |
|---|---|
| **Product recommendations** | "Customers who bought this also bought…" |
| **Content / news personalization** | **Show each subscriber the most relevant articles** |
| **Personalized ranking** | Re-order a search result or catalogue page per user |
| **Similar items** | "More like this" |
| **Targeted marketing** | Personalized email and campaign content |
| **Trending now** | Surface what's gaining traction |

## Why it beats the alternatives

**A representative scenario:** a news publisher wants to raise click-through rate by showing each subscriber the most relevant articles, with the **least operational overhead**.

| Option | Verdict |
|---|---|
| **Amazon Personalize** ✓ | **Purpose-built for exactly this.** Fully managed, API-accessible, no customization needed |
| **Amazon Bedrock** | You *could* build a recommender on an FM — but it needs **model customization and probably RAG components**. That's **more** operational overhead, so it loses a "least overhead" question |
| **Amazon Comprehend** | NLP for entities, key phrases, sentiment, language. **Cannot make recommendations** |
| **Amazon Kendra** | Intelligent **search** — finds relevant documents for a query. Not per-user recommendations |

---

## 🎯 The pattern this question teaches

> **When a question says "LEAST operational overhead" and a purpose-built AWS AI service exists for that exact task — pick the purpose-built service, not a foundation model.**

Bedrock is powerful and general, which makes it a *tempting* answer to almost anything. That generality is precisely why it **loses** overhead comparisons: building a recommender on an FM means customization, prompt design, possibly RAG, and evaluation work that Personalize has already done for you.

The same logic applies across the whole AI service catalogue:

| Task | Purpose-built service (low overhead) | The tempting-but-wrong general answer |
|---|---|---|
| Recommendations | **Personalize** | Bedrock + customization |
| Content moderation in images | **Rekognition** | SageMaker custom model / Canvas |
| Sentiment / entities / PII in text | **Comprehend** | Bedrock prompt |
| Speech → text | **Transcribe** | Bedrock |
| Text → speech | **Polly** | Bedrock |
| Translation | **Translate** | Bedrock |
| Text/forms/tables out of documents | **Textract** | Bedrock |
| Chatbot with intents and slots | **Lex** | Bedrock / SageMaker |
| Summarize documents & answer questions, no code | **Amazon Q Business** | Bedrock + RAG build |

> **The reflex to build:** read the task, ask *"is there an AWS service whose entire purpose is this?"* If yes, and the question emphasizes **least overhead / fully managed / no ML expertise**, that service is the answer. Reach for Bedrock or SageMaker only when the task is **genuinely custom** or **generative**.

---

## Related notes

- AWS managed AI services overview: [Domain 1, Task 1.2](../domains/01-ai-ml-fundamentals/02-practical-use-cases.md)
- Traditional ML vs. FMs: [Domain 1, Task 1.2](../domains/01-ai-ml-fundamentals/02-practical-use-cases.md)
- Bedrock capabilities: [Domain 2, Task 2.3](../domains/02-genai-fundamentals/03-aws-genai-infrastructure.md)
