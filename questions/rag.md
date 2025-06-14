### Q: What is HNSW (Hierarchical Navigable Small World) and how does it work?

**Category:** RAG  
**Difficulty:** Medium  
**Tags:** Vector Search, ANN, Indexing

**Sample Answer:**
HNSW (Hierarchical Navigable Small World) is an efficient algorithm and data structure for approximate nearest neighbor (ANN) search in high-dimensional spaces. It is widely used in vector search engines and retrieval-augmented generation (RAG) systems to quickly find the most similar vectors (such as embeddings) to a given query vector.

HNSW builds a multi-layer, graph-based index where each node represents a data point and is connected to its nearest neighbors. The structure is hierarchical: upper layers are sparser and allow for fast, long-range navigation, while lower layers are denser for precise local search. During a search, the algorithm starts at the top layer and navigates down, using greedy search to find the closest nodes at each level. This approach combines the speed of graph traversal with high recall.

Some important points about HNSW:
- **Performance & Scalability:** HNSW provides an excellent trade-off between search speed and accuracy, making it suitable for very large datasets (millions of vectors).
- **Dynamic Updates:** HNSW supports efficient dynamic insertions and deletions, which is useful for real-world systems where data changes over time.
- **Parameter Tuning:** Key parameters like `M` (number of neighbors per node) and `ef` (search breadth) allow you to balance speed, memory usage, and recall. Increasing `ef` improves recall at the cost of slower queries.
- **Small-World Inspiration:** The algorithm is inspired by small-world networks, where most nodes can be reached from any other node in a small number of steps, enabling efficient navigation.
- **Memory Usage:** HNSW can use more memory than some other ANN methods because it stores multiple graph layers and neighbor lists, but this is often justified by its performance.

**Note:** In this context, "high recall" means that the algorithm is able to retrieve most of the true nearest neighbors for a given query, even though it is an approximate search method. High recall is important for ensuring relevant results in applications like RAG.

### Q: How can advanced techniques like query rewriting, agentic RAG, and multiple indexes improve Retrieval-Augmented Generation (RAG) systems?

**Category:** RAG  
**Difficulty:** Hard  
**Tags:** Query Rewriting, Agentic RAG, Multi-Index, Advanced Retrieval

**Sample Answer:**
Modern RAG systems can be significantly improved by leveraging advanced retrieval and orchestration techniques:

- **Query Rewriting:** Before retrieving documents, the system can rewrite or expand the user’s query to better match the knowledge base. This may involve clarifying ambiguous queries, adding synonyms, or breaking a complex question into simpler sub-queries. Query rewriting helps improve retrieval relevance and recall, especially for vague or multi-faceted questions.
  - *Example:* A user asks, "How can I fix my laptop?" The system rewrites this as "How to troubleshoot common laptop hardware and software problems?" for more targeted retrieval.

- **Agentic RAG:** Agentic RAG refers to using an agent (often an LLM) to actively plan, decide, and orchestrate the retrieval process. The agent may choose which tools or indexes to query, decide when to reformulate queries, and iteratively refine retrieval based on intermediate results. This enables dynamic, context-aware retrieval strategies that go beyond static pipelines.
  - *Example:* An agent first queries a general knowledge base, then, based on the answer, queries a specialized medical database for more details, combining results for a comprehensive response.

- **Multiple Indexes:** Using multiple indexes allows the system to search across different data sources, modalities (text, images, code), or granularities (paragraph, document, table). The RAG pipeline can aggregate or rerank results from several indexes, or route queries to the most relevant index based on the question type. This increases coverage and accuracy, especially in complex or heterogeneous domains.
  - *Example:* A RAG system searching both a text corpus and a codebase to answer a programming question, merging results from both sources.

Other advanced and novel patterns in RAG include:

- **Multi-hop Retrieval:** The system performs retrieval in multiple steps, where the result of one retrieval is used to formulate the next query. This is useful for answering complex questions that require reasoning across multiple pieces of information.
  - *Example:* To answer "What is the latest research on vaccines for disease X?", the system first retrieves recent papers on disease X, then queries those papers for vaccine-related content.

- **Tool-Augmented RAG:** The agent can call external tools, APIs, or plugins (such as calculators, search engines, or code interpreters) as part of the retrieval and answer generation process, enabling more powerful and actionable responses.
  - *Example:* When asked "What is the square root of the population of France?", the agent retrieves France's population, then uses a calculator tool to compute the square root.

- **Feedback-Driven Retrieval:** The system incorporates user feedback or model self-evaluation to refine retrieval results over time, improving relevance and personalization.
  - *Example:* If a user marks a retrieved answer as irrelevant, the system learns to deprioritize similar results for future queries.

- **Retrieval Fusion:** Results from multiple retrieval strategies (e.g., dense, sparse, hybrid, or cross-encoder reranking) are fused or reranked to maximize answer quality.
  - *Example:* A RAG system combines results from both a BM25 keyword search and a vector similarity search, reranking them to present the most relevant answers.

- **Hybrid Search:** Combines dense (vector-based) and sparse (keyword-based) retrieval to leverage the strengths of both methods, improving recall and precision in diverse scenarios.
  - *Example:* For a legal query, the system retrieves relevant documents using both keyword and embedding search, ensuring both precise and semantically similar results are found.

- **Contextual Index Selection:** The agent dynamically selects or prioritizes indexes based on the context or type of query, optimizing for speed and relevance.
  - *Example:* For a medical question, the agent prioritizes a medical literature index over a general web index.

- **Chain-of-Thought RAG:** The system generates intermediate reasoning steps (chain-of-thought) and uses these steps to guide retrieval, enhancing performance on multi-step or reasoning-heavy queries.
  - *Example:* To answer "How did the economic crisis affect education in Europe?", the system first retrieves information about the economic crisis, then about its impact on education, and finally synthesizes the two.

Combining these methods allows RAG systems to be more flexible, accurate, and robust, providing better answers to a wider range of user queries.
