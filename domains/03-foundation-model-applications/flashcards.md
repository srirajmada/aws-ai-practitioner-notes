# Domain 3 — Flashcards

Cover the right column and work down. Anything you miss twice, go back to the source note.

---

## Task 3.1 — Design considerations

| Prompt | Answer |
|---|---|
| **The customization cost ladder, cheapest first** | **Prompt engineering → RAG → fine-tuning → continued pre-training → pre-training from scratch.** ⚠️ **RAG is ALWAYS cheaper than fine-tuning** |
| **Why is RAG cheaper than fine-tuning?** | RAG adds an **embedding model + vector database**. Fine-tuning adds **data labeling + training compute** — and labeling is human labour, the biggest cost. Fine-tuning also carries **re-training costs every time the data changes** |
| Why is prompt engineering the most cost-effective? | It requires **no additional infrastructure** at all |
| RAG's ongoing per-call cost | **More input tokens** — retrieved chunks are billed on **every** call |
| Fine-tuning's ongoing cost | **Custom model hosting** — either **on-demand** (per-token) or **Provisioned Throughput** (hourly, whether used or not) |
| The maintenance cost that decides it | Data changes → RAG needs a **re-index** (cheap). Fine-tuning needs a **re-train** (expensive), every refresh cycle |
| When could fine-tuning actually be cheaper? | Very high volume with **stable** requirements — short prompts beat paying for retrieved context every call. **But the exam ordering is fixed: RAG before fine-tuning** |
| **The two questions that decide RAG vs. fine-tuning** | "Does it need to **know** something?" → RAG. "Does it need to **behave** differently?" → fine-tuning |
| What is RAG in one line | Retrieve relevant passages from your data at query time and insert them into the prompt — grounding the answer, with citations, and **no training** |
| Does RAG change model weights | **No** — nothing about the model changes |
| Five problems RAG solves | Private/company data, knowledge cutoff, hallucination, no audit trail, constantly changing data (also access control via metadata filtering) |
| The two RAG phases | **Ingestion** (chunk → embed → store) and **query** (embed question → similarity search → retrieve → generate) |
| What is Amazon Bedrock Knowledge Bases | The managed RAG service — handles ingestion, chunking, embedding, storage, retrieval, and citations |
| The two Knowledge Base APIs | `Retrieve` (passages only) and `RetrieveAndGenerate` (full answer with citations) |
| Name the vector stores Bedrock Knowledge Bases supports | OpenSearch Serverless, OpenSearch Managed Cluster, Aurora PostgreSQL, Neptune Analytics, Pinecone, MongoDB Atlas, Redis Enterprise Cloud |
| Default/most common vector store choice | **OpenSearch Serverless** — auto-scaling, no cluster management |
| Which vector store for "we already run PostgreSQL" | **Aurora PostgreSQL** (or RDS for PostgreSQL) with **pgvector** |
| Which vector store for relationship-aware retrieval | **Neptune Analytics** — graph + vectors, powers **GraphRAG** |
| Which vector stores support binary vectors | OpenSearch Serverless and OpenSearch Managed Cluster |
| Titan Text Embeddings V2 dimension options | 256, 512, 1024 (Titan G1 is 1536) |
| What does temperature do | Reshapes the probability distribution — **lower = steeper/deterministic**, higher = flatter/creative |
| What does Top K do | Limits to the **K most-likely** tokens — a fixed count |
| What does Top P do | Limits to tokens within the top **P%** of cumulative probability — pool size **adapts** to model confidence |
| Top K vs. Top P in one line | Top K = fixed number of candidates. Top P = however many it takes to reach P% of probability mass |
| AWS's hoof beats example: Top P = 0.7 with horses 0.7, zebras 0.2, unicorns 0.1 | Only **horses** qualifies. At Top P = 0.9, horses and zebras both qualify |
| Which parameter for factual, reproducible output | **Low temperature** |
| Does temperature 0 stop hallucination | **No** — it makes the model hallucinate the *same wrong thing* consistently |
| Two length parameters | Max tokens / response length, and stop sequences (also penalties) |
| **Which factors impact LLM invocation latency?** | **Number of tokens in the prompt** and **number of tokens in the response** (also model size, hardware, cold starts, region) |
| **Do temperature, Top K, or Top P affect latency?** | **No.** AWS states they do **not** impact LLM invocation latency |
| **Why don't sampling parameters affect latency?** | Generating a token needs a **full forward pass** producing the whole probability distribution. Temperature/Top K/Top P only pick from a distribution **already computed** — the expensive work is done |
| The organizing rule | **Randomness params change WHAT you get (no latency effect). Length factors change HOW MUCH you get (direct latency effect).** |
| Why do prompt tokens affect latency? | The model must process the entire input before emitting the first token — **time to first token** (prefill phase) |
| Why do response tokens affect latency? | LLMs generate **one token at a time**, each needing another full forward pass (decode phase). Usually the **dominant** factor |
| The one indirect exception | Higher temperature *can* produce longer output → more response tokens → more latency. **Indirect and second-order — not a correct exam answer** |
| What is prompt caching and why | Reuses an already-processed repeated prefix so the model skips recomputing it — useful when a long system prompt or uploaded document repeats every call |
| Prompt caching — AWS's published savings | Up to **90% lower cost** and **85% lower latency** on supported models |
| Classic prompt caching use case | A chatbot where a user uploads a document then asks several questions — without caching the whole document is reprocessed and re-billed every time |
| Agent vs. RAG | RAG **retrieves information**. An agent **takes actions** — calls APIs, updates records, books things |
| Components of a Bedrock Agent | FM, instructions, **action groups** (Lambda + OpenAPI schema), knowledge bases, memory, prompt templates |
| What is ReAct | Reason + Act — the loop of reasoning, calling a tool, observing, repeating |

---

## Task 3.2 — Prompt engineering

| Prompt | Answer |
|---|---|
| Five prompt constructs | Instruction, context, input data, output indicator, negative prompt |
| **Which construct provides "situational information" / "background" / "sets up the scenario"?** | **CONTEXT** — the word **"situation"** in any form always means context |
| 🔑 The four-way split | **Context = the SITUATION. Instruction = the TASK. Input = the CONTENT. Output indicator = the FORMAT** |
| What is a negative prompt | A statement of what the model must **not** do; also common in image generation to exclude elements |
| **"Diffusion model produces blurry images with unwanted noise — which prompting technique?"** → | **Negative prompting.** Chain-of-thought, few-shot, and single-shot are all **LLM/text** techniques and can't help with images |
| What parameter holds a negative prompt in Nova Canvas? | **`negativeText`** |
| How should you word a negative prompt? | **Plain nouns of what you don't want** — `"waves, clouds"`. **Not** "no waves" or "without clouds" |
| Why avoid "no"/"without" in an image prompt? | Image models handle negation badly — naming the thing still steers generation toward it. Put it in `negativeText` |
| **Inpainting vs. outpainting** | **Inpainting changes what's INSIDE the mask** (add/remove/replace an element). **Outpainting changes everything OUTSIDE it** — replaces the **background**, preserving the masked subject |
| Two ways to define a mask | **`maskPrompt`** (natural language, e.g. "dog") or **`maskImage`** (an image marking the region) |
| How do you **remove** an element with inpainting? | Mask the element and **omit the `text` parameter** — the omission signals removal |
| How should you word an outpainting prompt? | Describe **the whole image you want**, including the parts that won't change — not just the new background |
| 🚨 The meta-lesson from this question | **Check the MODALITY first.** Text techniques (CoT, few-shot) for LLMs; image techniques (negative prompt, masks) for image models. Same category error as using a classification metric on regression |
| System prompt vs. user prompt | System = persistent role, rules, constraints for the conversation. User = the individual request |
| Zero-shot vs. one-shot vs. few-shot | No examples / one example / several examples (typically 2–10) |
| What is chain-of-thought and when | Ask the model to reason step by step before answering — for multi-step reasoning, maths, logic. Costs more output tokens |
| Phrase that triggers chain-of-thought | "Let's think step by step" / "show your reasoning first" |
| What is in-context learning | Teaching within the prompt itself (instructions + examples), with **no weight changes** — zero/one/few-shot are all in-context learning |
| What is self-consistency | Generate several reasoning paths and take the majority answer |
| What is prompt chaining | Feed one prompt's output into the next as a pipeline |
| Name five prompt best practices | Be specific and concise, provide context, specify output format, use examples, use delimiters (also break into steps, iterate, set guardrails) |
| **Prompt injection / hijacking** | Malicious input **overrides your instructions** and redirects the model |
| **Jailbreaking** | Crafted prompts **bypass safety controls** to elicit prohibited content |
| **Prompt leaking / exposure** | Extracting the **system prompt** or confidential context |
| **Poisoning** | Corrupting **training data or the RAG corpus** so the model learns/retrieves attacker content |
| Which of the four attacks targets data rather than the runtime prompt | **Poisoning** |
| Four mitigations for prompt injection | Delimiters separating instructions from data, input validation, Bedrock Guardrails, least-privilege tool access |
| Where should secrets go in a prompt | **Nowhere** — assume the system prompt can be extracted |
| What is Bedrock Prompt Management | Central catalogue for prompts with **versioning**, variables, testing/comparison, and deployment |
| Prompt Management vs. Flows | Prompt Management = versioning and governance of prompts. **Flows** = visually chaining prompts, KBs, agents, and Lambda into a workflow |
| Does a prompt transfer between models | Not reliably — prompts are model-specific and need retesting |

---

## Task 3.3 — Training and fine-tuning

| Prompt | Answer |
|---|---|
| **Fine-tuning vs. continued pre-training — the data** | Fine-tuning = **labeled** prompt/response pairs. Continued pre-training = **large unlabeled** domain corpus |
| What does fine-tuning teach | **Behaviour** — style, tone, format, task performance. **Not** current facts |
| What does continued pre-training teach | Deep domain **vocabulary and knowledge** (medicine, law) |
| Who does pre-training from scratch | Model providers. $M+ and months — essentially never the exam answer |
| What is model distillation | A large **teacher** model generates outputs; a small **student** model is trained to imitate it — near-teacher quality at a fraction of inference cost |
| Exam cue for distillation | "Maintain quality but dramatically cut inference cost at high volume" |
| Bedrock Model Distillation — what do you supply | **Only prompts.** The service generates teacher responses and fine-tunes the student for you |
| Distillation — AWS's published figures | Up to **500% faster**, up to **75% cheaper**, **<2% accuracy loss** for use cases like RAG |
| The catch with distillation | The student matches the teacher on the **specific use case it was distilled for**, not in general — breadth traded for cheap speed |
| What does "using multiple comments" mean (3.2 objective) | Annotating/labelling prompt sections (`### INSTRUCTION ###`, `### CONTEXT ###`) — improves reliability, maintainability, and **reduces injection risk** by fencing user data |
| What does "task engineering" mean (3.4 objective) | How well the business problem is **decomposed into tasks the FM reliably performs**. A failing model is often a badly framed task |
| 🚦 **FILTER 1: Are RAG and prompt engineering training methods?** | **NO — neither changes the model's weights.** AWS: *"RAG is not a training method"* and *"prompt engineering is not a method to train an FM"* |
| **"Needs to TRAIN an FM on business data"** → what do you eliminate first? | **RAG and prompt engineering**, immediately. The word **"train"** rules them out before you compare anything else |
| Does "RAG for facts, fine-tuning for behaviour" still hold? | Yes — **but only after** you know a training method is wanted. **"Train" outranks it.** If the stem says train/fine-tune/adapt the model, RAG is off the table |
| **The four customization techniques and their purpose** | **Domain adaptation FT** = industry terminology. **Instruction-based FT** = a specific downstream task (labeled examples). **Continued pre-training** = deep domain vocabulary (large unlabeled corpus). **RLHF** = alignment with human goals |
| **"1 GB dataset, train and evaluate within 3 weeks"** → | **Domain adaptation fine-tuning** — small/limited data + a weeks-long deadline. Full pre-training takes **months** |
| Data-size signal: domain adaptation vs. continued pre-training | **Small/limited (~1 GB), "unique vocabulary"** → domain adaptation FT. **Large volume of unlabeled** → continued pre-training |
| What rules out full pre-training in a question? | **Time** (months, not weeks) and **corpus size** (massive, not 1 GB) |
| "Ensure generated content uses industry-specific terminology" → | **Domain adaptation fine-tuning** |
| "Has a **large volume of unlabeled** industry standards and research" → | **Continued pre-training** — the large *unlabeled corpus* is the tell |
| Domain adaptation vs. continued pre-training — the differentiator | **How the question describes the data.** Big unlabeled corpus → continued pre-training. Terminology goal with no dataset emphasis → domain adaptation fine-tuning |
| What is instruction-based fine-tuning for | **A specific downstream task**, using **labeled** examples. AWS explicitly says it is *not* how you teach industry terminology |
| **"Must understand and follow DIRECTIONS / give GUIDED responses"** → | **Instruction tuning.** "Directions"/"instructions" is close to a literal word-match |
| 🔑 The cleanest split of the four techniques | **Instruction tuning → how it BEHAVES. Domain adaptation & continued pre-training → what it KNOWS. RLHF → what it VALUES** |
| Can RLHF teach industry-specific terminology? | **No** — RLHF is for **alignment** with human goals and preferences, not vocabulary |
| Is "supervised learning" an FM customization technique? | **No** — it's a general training paradigm and appears as a distractor. The FM-specific equivalent is **instruction-based fine-tuning** |
| **Is a vector store needed for fine-tuning?** | **No.** Vector stores are **RAG** machinery. Fine-tuning runs a training job over labeled examples — no retrieval involved |
| **What is the OUTCOME of a Bedrock fine-tuning job?** | **A new custom model.** The base model is untouched |
| **Can every Bedrock FM be fine-tuned?** | **No** — model customization is not supported by all FMs |
| What is instruction tuning | Fine-tuning on instruction→response pairs so the model follows directions in your style |
| What is transfer learning | The umbrella principle: adapt a model trained on one task to a related one, reusing learned representations |
| What is RLHF | Humans rank outputs → a reward model learns those preferences → the model is tuned to maximize reward. Used for alignment |
| What is PEFT / LoRA | Parameter-Efficient Fine-Tuning — update a small set of added parameters instead of all weights. Cheaper and faster |
| What is catastrophic forgetting | Fine-tuning too narrowly degrades the model's general capability |
| Realistic fine-tuning dataset size | Hundreds to a few thousand high-quality examples — not millions |
| Quality or quantity for fine-tuning data | **Quality, consistency, and representativeness beat volume** |
| Six fine-tuning data requirements | Curation, labeling, size, representativeness, governance, bias review (also formatting, validation split) |
| Why does governance matter for training data | Fine-tuning data becomes part of the model — PII can surface in outputs and can't easily be un-trained |
| **How are custom models served on Bedrock?** | **On-demand custom model deployment OR Provisioned Throughput** — both are valid, and both can run for the same model. It is **not** PT-only |

---

## Task 3.4 — Evaluation

| Prompt | Answer |
|---|---|
| **ROUGE** — full name, measures, used for | Recall-Oriented Understudy for Gisting Evaluation. **Recall**-oriented overlap. **Summarization** |
| **BLEU** — full name, measures, used for | Bilingual Evaluation Understudy. **Precision**-oriented overlap. **Translation** |
| **BERTScore** — measures | **Semantic** similarity via embeddings — credits valid paraphrases |
| **"Evaluate how similar generated responses are to SME/reference responses"** → | **BERTScore** — it measures **semantic similarity** between generated and reference text |
| Why not perplexity for that? | Perplexity measures the **probability of generating a sequence** — not semantic similarity to a reference |
| Why not MSE or F1 for that? | MSE is **regression**, F1 is **classification**. Neither applies to generated text |
| **Pre-training vs. continued pre-training** | Pre-training starts from **random weights** on a **general** corpus ($M, months, done by **providers**). Continued pre-training starts from an **already-trained FM** on a **domain-specific** corpus (done by **customers**). **Both use unlabeled data** |
| Mental model for continued pre-training | Pre-training = raising a person from birth. Continued pre-training = sending an educated adult to immerse in a new field |
| Why doesn't RAG teach industry terminology? | RAG injects terminology as **context per query** — it doesn't improve the **model's own ability** to use that terminology in future responses |
| RAG vs. continued pre-training for domain language | RAG **retrieves** terminology at query time. Continued pre-training **absorbs** it into the weights. "Learn the language" → continued pre-training |
| The memory hook for ROUGE | **R** for **R**ecall → summa**R**ization |
| **"Which metric assesses FM performance for text summarization?"** | **ROUGE-N.** Accuracy, F1, and recall are all classification metrics and are pure distractors here |
| What is **ROUGE-N**? | **N-gram overlap** between generated and reference summaries. ROUGE-1 = single words, ROUGE-2 = word pairs |
| What is **ROUGE-L**? | **Longest Common Subsequence** — rewards matching word order without needing contiguity |
| **Why isn't plain "Recall" the answer for summarization?** | ROUGE is *recall-oriented*, but **bare Recall is a classification metric** (TP/(TP+FN) on class labels). ROUGE compares **text n-grams** against a reference. Different data, different problem type |
| 🚨 The most common metric error | Picking **classification metrics for non-classification tasks** (regression → Accuracy/Precision; summarization → F1). **Accuracy/precision/recall/F1 are ONLY valid when the output is a discrete class label** |
| Why do ROUGE and BLEU fail on paraphrases | They compare **words**, not meaning. "excellent film" vs. "superb movie" scores near zero |
| What is perplexity | How "surprised" a model is by text — lower is better |
| What is LLM-as-a-judge | A strong model scores another's output against a rubric — cheaper and faster than humans, scales well, but inherits the judge's biases |
| Four evaluation approaches | Automatic/programmatic, human evaluation, LLM-as-a-judge, benchmark datasets |
| Name three benchmark datasets | MMLU, HELM, BIG-bench (also GLUE, TruthfulQA) |
| Weakness of public benchmarks | Generic — may not reflect your task; risk of contamination |
| What does Bedrock Evaluations support | Automatic (BERTScore, F1, exact match), human evaluation, and LLM-as-a-judge — for both **model** and **RAG** evaluation |
| **What is FMEval?** | The **Foundation Model Evaluation library inside SageMaker Clarify** — compares FM **quality and responsibility** metrics including **bias and toxicity scores**, using built-in or your own test datasets |
| **Name the five FMEval dimensions** | **Semantic robustness, factual knowledge, prompt stereotyping, toxicity, accuracy** |
| **What is semantic robustness?** | How much the output **changes when the input is perturbed without changing its meaning** — typos, random uppercase, added/deleted whitespace |
| **"How does response quality change with SMALL ADJUSTMENTS IN THE INPUT?"** → | **Semantic robustness.** RMSE/AUC/F1 are all traditional-ML metrics and **cannot evaluate an LLM** |
| What is prompt stereotyping? | Probability the model encodes **bias** — race, gender, religion, age, nationality, disability, appearance, socioeconomic status |
| What is factual knowledge (FMEval)? | How well the model **encodes factual information** |
| Which responsible-AI dimension does semantic robustness measure? | **Veracity and robustness** — *"correct outputs even with unexpected or adversarial inputs"* |
| 🚨 **Can RMSE, MAE, R², accuracy, precision, recall, F1, or AUC evaluate an LLM?** | **NO — not one of them.** They're all traditional-ML metrics. LLM evaluation uses ROUGE, BLEU, BERTScore, perplexity, semantic robustness, toxicity, factual knowledge, faithfulness, LLM-as-a-judge |
| Clarify's **three** jobs | ① Bias detection ② Explainability (SHAP) ③ **FM evaluation via FMEval** — it's not just for traditional ML |
| "Identify which FM gives positive, unbiased responses **at scale**" → | **SageMaker Clarify to quantify toxicity on a test dataset** |
| **Why is Amazon A2I wrong for model selection?** | A2I is **human review of production responses** — it can't tell you which FM to pick, and human review **doesn't scale**. The words "at scale" rule it out |
| Model selection vs. production review | **Select** = automated metrics on a test dataset (Clarify/FMEval, Bedrock Evaluations), scales. **Production review** = humans on individual responses (A2I), doesn't scale |
| **Two limits of AWS AI Service Cards** | ① Cover only **AWS-developed** models — **not open-source or third-party**. ② Show **sample benchmark data** — test on your own use-case dataset instead |
| Bedrock built-in LLM-judge metrics | Correctness, completeness, **faithfulness** (hallucination detection), plus responsible-AI: answer refusal, harmfulness, stereotyping |
| **How do you evaluate RAG** | **Separately by stage** — retrieval metrics (context relevance, coverage) and generation metrics (faithfulness, correctness, completeness) |
| Why evaluate RAG stages separately | To know *which* stage failed — bad retrieval means fix chunking/embeddings/reranking; good retrieval with a bad answer means fix the prompt or model |
| What is faithfulness / groundedness | Is every claim in the answer supported by the retrieved context — the direct hallucination measure for RAG |
| The headline metric for an agent | **Task completion rate** |
| Name four other agent metrics | Tool selection accuracy, tool call correctness, steps/efficiency, cost and latency per task (also error recovery) |
| Three business objective alignment metrics named in the objective | **Task completion rate, user satisfaction, cost per interaction** |
| What is a golden dataset | A curated set of representative inputs with known-good outputs, used to evaluate every model or prompt change |
| Gold standard for tone, safety, and subjective quality | **Human evaluation** |

---

## Rapid-fire

| Prompt | Answer |
|---|---|
| Domain 3 weighting | 28% (~14 of 50 scored questions) — the largest domain |
| Domains 2 + 3 combined | 52% of the exam |
| "Needs current company data" → | **RAG** |
| "Needs consistent house style/format" → | **Fine-tuning** |
| "Needs deep medical/legal vocabulary" → | **Continued pre-training** |
| "Quality is fine, inference costs too much" → | **Distillation** |
| "Needs to actually do something, not just answer" → | **Agent** |
| "Cheapest thing to try first" → | **Prompt engineering** |
