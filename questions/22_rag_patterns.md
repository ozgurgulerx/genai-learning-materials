# Retrieval-Augmented Generation (RAG) Patterns – Detailed Q&A

Below are 10 key RAG patterns with sample answers, ordered from beginner to advanced.

---

### Q1: What is hybrid (dense + sparse) retrieval and why can it outperform pure dense search in RAG systems?

**Category:** RAG  
**Difficulty:** Easy  
**Tags:** Hybrid Search, BM25, Embeddings

**Sample Answer:**
Hybrid search linearly or heuristically combines scores from sparse keyword-based methods (e.g., BM25, SPLADE) with dense vector similarity.
• **Complementarity:** Sparse excels at exact term matches; dense captures semantic similarity.
• **Score Fusion:** Common schemes include α·BM25 + (1−α)·cosine or Reciprocal Rank Fusion.
• **Empirical Gains:** In domains with vocabulary mismatch (e.g., code), hybrid improves recall 5–15 % over either method alone, leading to more grounded RAG answers.

---

### Q2: Explain how retrieval fusion (e.g., Reciprocal Rank Fusion) combines multiple retrievers and its impact on answer recall.

**Category:** RAG  
**Difficulty:** Easy-Medium  
**Tags:** RRF, Ensemble Retrieval

**Sample Answer:**
Retrieval fusion aggregates ranked lists from *n* independent retrievers.
• **Reciprocal Rank Fusion (RRF):** Score(doc) = Σ 1/(k + rank_i), with k≈60 smoothing.
• **Benefit:** Robust to noisy individual rankings; top-k recall increases because each retriever covers different parts of the corpus.
• **Use in RAG:** Higher recall helps the generator synthesize complete answers without extra API calls.

---

### Q3: Describe multi-hop RAG and give an example use-case.

**Category:** RAG  
**Difficulty:** Medium  
**Tags:** Multi-Hop, Iterative Retrieval

**Sample Answer:**
Multi-hop RAG performs successive retrieval steps where later queries are conditioned on earlier retrieved passages.
Example workflow for chain-of-thought QA:
1. **First hop:** Retrieve docs about entity A.
2. **Query expansion:** Extract clue “B” from passage; formulate new query.
3. **Second hop:** Retrieve docs about entity B.
4. Generator combines info from both hops to answer “How is A related to B?”.
This approach solves compositional questions that single-hop retrieval misses.

---

### Q4: How does query rewriting improve retrieval quality and what techniques are commonly used?

**Category:** RAG  
**Difficulty:** Medium  
**Tags:** Query Rewriting, Clarification

**Sample Answer:**
Models rewrite the original user query into a form better aligned with retrieval indexes.
• **Conversational Rewrites:** T5-based QRewriter adds context (“In Star Wars lore, who…”) to resolve pronouns.
• **Keyword Extraction:** Use sequence labeling to drop stop-words and keep entities.
• **Paraphrase Diversification:** Generate multiple rewrites and run parallel retrieval to increase coverage.
Overall, rewriting raises recall and precision, especially in multi-turn chats.

---

### Q5: When using multiple indexes (documents, code, tables), how does an agent choose which index to query?

**Category:** RAG  
**Difficulty:** Medium-Hard  
**Tags:** Index Selection, Agents

**Sample Answer:**
• **Classifier:** A small LM or logistic model predicts domain (text vs. code) from the question.
• **Routing via Tool-Calling:** The generator emits a tool name (`search_code` vs `search_docs`).
• **Meta-Retrieval:** Issue trial queries to all indexes, then pick the one with highest max similarity.
Proper routing avoids wasted tokens and returns domain-specific context to the generator.

---

### Q6: Discuss feedback-driven or active RAG where model confidence or user feedback triggers follow-up retrieval.

**Category:** RAG  
**Difficulty:** Hard  
**Tags:** Active Retrieval, Confidence

**Sample Answer:**
During generation, the model estimates uncertainty (e.g., softmax entropy). If above a threshold, it calls the retriever again with refined keywords. Alternatively, explicit user thumbs-down triggers re-querying.
Active loops cut hallucination rates by ~30 % and let the system converge to satisfactory answers with minimal user friction.

---

### Q7: How can external tools or calculators be integrated into a RAG pipeline (tool-augmented RAG)?

**Category:** RAG  
**Difficulty:** Hard  
**Tags:** Tools, Calculation

**Sample Answer:**
Tool-augmented RAG extends retrieval with deterministic APIs:
• **Numeric QA:** After retrieving stats, the LLM calls `calc()` to perform divisions.
• **Code Examples:** Fetch code snippet then run `python_executor` to show output.
Implementation requires schemas, secure sandboxes, and caching to balance latency.

---

### Q8: What are the benefits and challenges of jointly training the retriever and generator components?

**Category:** RAG  
**Difficulty:** Hard  
**Tags:** Joint Training, End-to-End

**Sample Answer:**
Benefits:
1. **Task Alignment:** Retriever learns to surface passages that maximize downstream log-likelihood.
2. **Reduced Cascaded Errors:** Generator gradients back-prop to retriever via REINFORCE or contrastive losses.
Challenges:
• **Memory Footprint:** Need to encode millions of docs per step; often mitigated by negative caching.
• **Stability:** RL-style credit assignment noisy; temperature annealing or Gumbel-Softmax helps.

---

### Q9: Compare short-term memory buffers with long-term vector stores for personalized assistants.

**Category:** RAG  
**Difficulty:** Hard  
**Tags:** Memory, Personalization

**Sample Answer:**
• **Short-Term (Conversation) Memory:** Recent chat history stored in context window or ephemeral cache; fast but volatile.
• **Long-Term Vector Store:** Persisted embeddings of user docs/preferences; slower lookup but durable.
Hybrid strategy: write salient chat snippets to vector DB with metadata; retrieve when similar intent appears later.

---

### Q10: Outline a chain-of-thought RAG pipeline where the generator’s intermediate reasoning triggers targeted retrieval.

**Category:** RAG  
**Difficulty:** Very Hard  
**Tags:** CoT RAG, Iterative

**Sample Answer:**
1. Generator starts CoT: “First, I need GDP of France.”
2. Detect `[RETRIEVE] GDP France` tag → call retriever.
3. Insert passage; continue reasoning.
4. Subsequent steps spawn further retrieval (“population Paris”).
Evaluate answer after full chain; optional verifier checks each evidence citation. This tightly couples reasoning and retrieval, boosting factual completeness on multi-fact tasks.

---
