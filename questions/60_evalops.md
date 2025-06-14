# LLM Evaluation & EvalOps – Detailed Q&A

Below are 25 interview-style questions with comprehensive answers spanning beginner to advanced concepts in evaluating and operating LLM systems.

---

### Q1: Why do traditional NLP metrics like BLEU or ROUGE often fail for open-ended LLM outputs?

**Category:** EvalOps  
**Difficulty:** Beginner  
**Tags:** Metrics, BLEU, ROUGE

**Sample Answer:**
BLEU/ROUGE rely on n-gram overlap with a single reference answer. Open-ended tasks (e.g., creative writing, summarization) admit many valid outputs that share few lexical n-grams with the reference. Overlap metrics therefore undervalue semantic correctness, reward word-by-word parroting, and suffer from high variance with small references. Embedding-based measures (BERTScore), LLM-as-Judge, or task-specific criteria better capture meaning and style flexibility.

---

### Q2: Define intrinsic vs. extrinsic evaluation in the context of LLM systems. Give concrete examples.

**Category:** EvalOps  
**Difficulty:** Beginner  
**Tags:** Intrinsic, Extrinsic

**Sample Answer:**
• **Intrinsic** evaluates the model in isolation, independent of downstream application. Examples: perplexity on held-out corpora, MMLU accuracy, toxicity rates using Perspective API.
• **Extrinsic** measures impact within a product workflow. Examples: click-through rate uplift when using LLM-generated ad copy, average handle-time reduction in a support bot, revenue uplift from personalized descriptions.

---

### Q3: What is "LLM-as-Judge" evaluation and when is it appropriate? List two pitfalls.

**Category:** EvalOps  
**Difficulty:** Beginner-Medium  
**Tags:** LLM-as-Judge

**Sample Answer:**
LLM-as-Judge uses a strong reference model (often GPT-4) to grade candidate answers via a scoring rubric. Appropriate when human grading is too slow or costly and rubric can be encoded in natural language. Pitfalls: (1) **Bias/Alignment:** the judge may favor its own reasoning style, penalizing creative but valid answers; (2) **Prompt Leakage & Position Bias:** answer ordering or irrelevant tokens can skew scores – shuffled and system-level prompting mitigates this.

---

### Q4: How does the RAGAS framework score retrieval-augmented generation pipelines? Explain at least three of its sub-metrics.

**Category:** EvalOps  
**Difficulty:** Medium  
**Tags:** RAG, RAGAS

**Sample Answer:**
RAGAS decomposes quality into retrieval + generation metrics:
1. **Context Precision/Recall:** percent of retrieved chunks that are relevant vs. percent of relevant chunks retrieved.
2. **Faithfulness:** LLM judge verifies the answer is fully grounded in provided context (no hallucination).
3. **Answer Relevancy:** semantic similarity between answer and ground-truth.
4. **Context Utilization:** how much of the cited context is actually referenced in the answer.
5. **Conciseness & Fluency:** style metrics scored via LLM rubric.

---

### Q5: Describe how Arize.ai’s Phoenix uses embedding distance to surface drift in production.

**Category:** EvalOps  
**Difficulty:** Medium  
**Tags:** Monitoring, Drift, Arize

**Sample Answer:**
Requests and responses are embedded (e.g., OpenAI text-embedding-3). Phoenix stores historical vectors; it computes distance distributions (cosine) between online traffic and training baseline. Significant KL-divergence or population-shift statistics raise drift alerts. UI plots heatmaps to identify which latent dimensions changed and correlates drift with quality metrics (thumbs-up rate) for root-cause analysis.

---

### Q6: Contrast prompt-based evals (e.g., Criteria-LM) with supervised reference-based evals.

**Category:** EvalOps  
**Difficulty:** Medium  
**Tags:** Prompt Evals, Reference

**Sample Answer:**
Prompt-based evals feed the candidate answer and a natural-language rubric into an LLM grader. They require no gold references, scale cheaply, and adapt to nuanced criteria (helpfulness, coherence). Reference-based evals compare candidate to one or more human answers via distance (BLEU, BERTScore). They provide deterministic baselines and can be model-agnostic but require costly labeled data and may miss diverse valid outputs.

---

### Q7: What role do guardrail tests (toxicity, PII) play in a CI pipeline for LLM releases?

**Category:** EvalOps  
**Difficulty:** Beginner  
**Tags:** Safety, CI/CD

**Sample Answer:**
They serve as blocking tests analogous to unit tests: each new model or prompt change runs through automated safety suites. Toxicity scores (Perspective, Detoxify) and PII regex classifiers must be below thresholds; failures trigger build break or require override approval. This shifts safety left and prevents regressions from shipping.

---

### Q8: Explain Giskard’s concept of "slice" testing and why it matters for bias detection.

**Category:** EvalOps  
**Difficulty:** Medium  
**Tags:** Bias, Slice Testing, Giskard

**Sample Answer:**
Slice tests evaluate model performance on specific data sub-groups (e.g., dialect, gendered names). Giskard auto-discovers slices using clustering or user-defined predicates and computes metric deltas per slice. Large disparities flag fairness or robustness issues that aggregate scores hide.

---

### Q9: Detail how you would build a regression test suite for a RAG chatbot answering product FAQs.

**Category:** EvalOps  
**Difficulty:** Medium  
**Tags:** RAG, Regression Tests

**Sample Answer:**
1. Curate 200 representative FAQ pairs with authoritative answers.
2. Store expected citations (doc IDs).
3. CI script queries new model, records answer + cited chunks.
4. Metrics: Exact Match, Faithfulness via LLM, Citation Recall.
5. Thresholds: EM≥0.7, Faithfulness≥0.9. Failures dump diff for manual triage.

---

### Q10: How can Monte-Carlo sampling improve the robustness of LLM-as-Judge scores?

**Category:** EvalOps  
**Difficulty:** Medium  
**Tags:** Variance, MC Sampling

**Sample Answer:**
Randomly sample multiple judge prompts (different seeds, role instructions, answer ordering) and average scores. This reduces prompt sensitivity variance and yields confidence intervals; experiments show ~30% reduction in standard deviation vs. single prompt.

---

### Q11: Compare static evaluation datasets (e.g., MMLU, MT-Bench) with dynamic evaluation via live traffic canaries.

**Category:** EvalOps  
**Difficulty:** Medium  
**Tags:** Benchmarks, Canary

**Sample Answer:**
Static datasets offer repeatability, public comparability, and low noise but can overfit and may not represent product domain. Dynamic canary deploys model to 1-5% real users, measuring engagement and safety metrics in situ. Trade-off: realism vs. cost and risk; best practice uses both phases.

---

### Q12: Describe a trajectory-based evaluation for a planning agent (e.g., AutoGPT) and which metrics you would track.

**Category:** EvalOps  
**Difficulty:** Advanced  
**Tags:** Agents, Trajectory Eval

**Sample Answer:**
Record full action sequence ≈ (state, tool, arguments, result). Metrics:
• **Task Success Rate** – binary on final goal.
• **Step Efficiency** – actions taken vs. optimal.
• **Error Recovery Rate** – ratio of failing steps recovered.
• **Cost** – total API/tool spend.
• **Safety Violations** – unsafe tool invocations.
Replay logs offline for analysis; attach critic LLM to rate reasoning coherence per step.

---

### Q13: What is "step-wise" or intermediate reasoning eval and why might it catch errors missed by final-answer evals?

**Category:** EvalOps  
**Difficulty:** Advanced  
**Tags:** Chain-of-Thought, Intermediate Eval

**Sample Answer:**
Step-wise eval grades each reasoning step (thought or tool call) rather than only the final answer. Hallucinations or math errors mid-chain that cancel out later would be detected. It provides granular feedback for policy tuning and reduces reward hacking where model outputs correct answer with flawed rationale.

---

### Q14: Outline a method to estimate uncertainty in LLM outputs without access to model logits.

**Category:** EvalOps  
**Difficulty:** Advanced  
**Tags:** Uncertainty

**Sample Answer:**
Use **ensemble proxy sampling**: query the model with k prompt variants or temperature samples; compute answer diversity (e.g., pairwise BLEU or embedding variance). High variance correlates with low confidence. Another method: ask the model for self-rated confidence and calibrate against historical accuracy.

---

### Q15: How do you evaluate tool-augmented agents that execute code or API calls? Mention at least two frameworks.

**Category:** EvalOps  
**Difficulty:** Advanced  
**Tags:** Tool Use, Agents

**Sample Answer:**
Combine **exec-safety tests** (e.g., Sandbox, Pyright) with task-grounded metrics. Frameworks:
1. **OpenAI Evals (Tool mode)** – defines tool call JSON schema and validates responses.
2. **LangChain Bench** – simulates tool environment and checks final artifacts.
Also track runtime exceptions, latency, and side-effect correctness.

---

### Q16: Explain the difference between deterministic replay and synthetic traffic generation for offline evals.

**Category:** EvalOps  
**Difficulty:** Medium  
**Tags:** Replay, Synthetic Traffic

**Sample Answer:**
Deterministic replay reuses exact historical requests to compare outputs across model versions. Synthetic traffic generates new examples via data augmentation or LLMs to explore edge cases. Replay ensures real-world relevance; synthetic uncovers corner cases and improves coverage.

---

### Q17: What are Structured Output Tests (SOTs) in OpenAI Evals and how do they differ from FreeForm tests?

**Category:** EvalOps  
**Difficulty:** Medium  
**Tags:** OpenAI Evals, SOT

**Sample Answer:**
SOTs require model to produce JSON/YAML conforming to a schema; evaluation checks parseability and field accuracy. FreeForm compares natural-language answers with references or rubric grading. SOTs are crucial for tool invocation accuracy and downstream parsing safety.

---

### Q18: Discuss pros/cons of using GPT-4 as a grading rubric for smaller models versus human graders.

**Category:** EvalOps  
**Difficulty:** Medium  
**Tags:** GPT-4 Judge, Human Eval

**Sample Answer:**
Pros: scalable, consistent, faster turnaround, granular rubrics. Cons: may encode GPT-4 biases, less transparent, cost at scale, risk of reinforcing errors if GPT-4 wrong. Hybrid approach: 10% human QA of judge scores, calibrate routinely.

---

### Q19: Define “eval-paradox” in RLHF fine-tuning and how to mitigate it.

**Category:** EvalOps  
**Difficulty:** Advanced  
**Tags:** RLHF, Eval Paradox

**Sample Answer:**
As the policy optimizes reward model, distribution shifts so reward no longer correlates with human preference; policy exploits reward model blind spots. Mitigate by (1) periodically collecting fresh human data to retrain reward, (2) adversarial data mining, (3) ensemble or uncertainty-aware rewards.

---

### Q20: How can contrastive adversarial prompts be used to stress-test an LLM’s safety filters?

**Category:** EvalOps  
**Difficulty:** Advanced  
**Tags:** Adversarial, Safety

**Sample Answer:**
Generate pairs (benign, slightly perturbed) where benign is safe, perturbed seeks disallowed content (jailbreak). Evaluate filter precision/recall. Tools: **Promptbench**, **RedTeamingLLM**. Track ASR (attack success rate) and iterate defenses.

---

### Q21: Describe a BerOps-style evalops workflow that automatically rolls back a model if KPI deltas breach SLOs.

**Category:** EvalOps  
**Difficulty:** Advanced  
**Tags:** MLOps, Rollback

**Sample Answer:**
Pipeline: Blue-green deployment → live A/B metrics streamed to feature store → control charts monitor delta (e.g., CSAT drop >2%). Alert triggers rollout controller to revert traffic to previous version, opens JIRA with logs and diffed prompts. Governance doc defines SLOs and rollback thresholds.

---

### Q22: Explain how vector-search recall can be evaluated at scale without expensive manual labeling.

**Category:** EvalOps  
**Difficulty:** Medium  
**Tags:** Recall, Vector Search

**Sample Answer:**
Approximate ground truth by self-querying: treat each document as query, measure if k-NN includes itself (identity recall). Complement with synthetic Q-A generation: LLM generates question answered by doc; if retrieval brings doc, mark hit. Provides high-volume proxy recall metric.

---

### Q23: What metrics would you log for real-time evaluation of an agent system that schedules meetings via email threads?

**Category:** EvalOps  
**Difficulty:** Medium  
**Tags:** Agents, Live Metrics

**Sample Answer:**
• Task success (meeting booked)  
• Time-to-schedule  
• Email count per thread  
• Human override rate  
• Calendar conflict errors  
• Sentiment of participants  
• Cost ($ per booking)

---

### Q24: Give an example of a "chain-of-thought faithfulness" metric and how to compute it.

**Category:** EvalOps  
**Difficulty:** Advanced  
**Tags:** Faithfulness, CoT

**Sample Answer:**
Compute **Self-Consistency Faithfulness**: sample N reasoning paths at T=0.8, run final answer majority vote; for each path, judge via LLM if reasoning logically leads to answer. Faithfulness = (#consistent paths)/N. Low score indicates spurious reasoning.

---

### Q25: Predict future trends in eval infrastructure (e.g., adaptive benchmarks, self-play, simulation environments).

**Category:** EvalOps  
**Difficulty:** Opinionated  
**Tags:** Future, Trends

**Sample Answer:**
Expect (1) **Adaptive Benchmarks** that generate new adversarial items on fail cases; (2) **Multi-agent Simulations** (e.g., Arena) to test social alignment; (3) **Self-play evals** for tool use; (4) **On-device telemetry** feeding federated eval pipelines; (5) **Unified EvalOps platforms** integrating safety, drift, and business KPIs.
