---
### Q: What is HNSW (Hierarchical Navigable Small World) and how does it work?

**Category:** RAG  
**Difficulty:** Medium  
**Tags:** Vector Search, ANN, Indexing

**Sample Answer:**
HNSW (Hierarchical Navigable Small World) is an efficient algorithm and data structure for approximate nearest neighbor (ANN) search in high-dimensional spaces. It is widely used in vector search engines and retrieval-augmented generation (RAG) systems to quickly find the most similar vectors (such as embeddings) to a given query vector.

HNSW builds a multi-layer, graph-based index where each node represents a data point and is connected to its nearest neighbors. The structure is hierarchical: upper layers are sparser and allow for fast, long-range navigation, while lower layers are denser for precise local search. During a search, the algorithm starts at the top layer and navigates down, using greedy search to find the closest nodes at each level. This approach combines the speed of graph traversal with high recall.

**Note:** In this context, "high recall" means that the algorithm is able to retrieve most of the true nearest neighbors for a given query, even though it is an approximate search method. High recall is important for ensuring relevant results in applications like RAG.

---
