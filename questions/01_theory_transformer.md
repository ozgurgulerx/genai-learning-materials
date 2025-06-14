### Q: What is a Transformer in machine learning? Explain how self-attention works (including key, query, value calculations), the difference between encoder-only, decoder-only, and encoder-decoder models, and how Transformers are trained and used for inference.

**Category:** Theory  
**Difficulty:** Hard  
**Tags:** Transformers, Self-Attention, Deep Learning, Architecture

**Sample Answer:**
A Transformer is a deep learning architecture introduced in the paper "Attention Is All You Need" (Vaswani et al., 2017) that has become the foundation for modern natural language processing (NLP) and generative AI models. Transformers are designed to handle sequential data (like text) but do so without relying on recurrence or convolution. Instead, they use a mechanism called self-attention to model relationships between all elements in a sequence, regardless of their distance.

**Self-Attention and KQV Calculations:**
Self-attention allows each token in a sequence to attend to all other tokens, enabling the model to capture context and dependencies. The process involves three key vectors for each token:
- **Query (Q):** Represents the current token's "question" about other tokens.
- **Key (K):** Represents how much information each token has that might be relevant to other tokens' queries.
- **Value (V):** Contains the actual information/content of the token.

For each token, the attention score is calculated by taking the dot product of its Query with the Keys of all tokens, scaling, and applying a softmax to get attention weights. These weights are then used to compute a weighted sum of the Value vectors, producing the output for each token:

    Attention(Q, K, V) = softmax(QKᵀ / √dₖ) V

where dₖ is the dimension of the Key vectors. This operation is performed in parallel for all tokens and across multiple attention heads (multi-head attention) for richer representations.

**Model Variants:**
- **Encoder-only (e.g., BERT):** Processes input sequences to produce contextual embeddings for each token. Used for tasks like classification, named entity recognition, etc.
- **Decoder-only (e.g., GPT):** Generates output sequences one token at a time, attending only to previous tokens (causal masking). Used for language modeling and text generation.
- **Encoder-Decoder (e.g., T5, original Transformer):** The encoder processes the input, and the decoder generates output conditioned on the encoder's output. Used for tasks like translation and summarization.

**Training:**
Transformers are typically trained using supervised learning with large labeled datasets. For language models, training often involves predicting the next token (causal language modeling) or masked tokens (masked language modeling). Training uses backpropagation and gradient descent to optimize model weights.

**Inference:**
During inference, the Transformer processes input sequences through its layers of self-attention and feed-forward networks. For generation (decoder or encoder-decoder), tokens are produced one at a time, with each new token appended to the sequence and fed back into the model until a stopping condition is met (e.g., end-of-sequence token or max length).

Transformers are highly parallelizable, scale well with data and compute, and have achieved state-of-the-art results in many NLP, vision, and multi-modal tasks.

---

### Q: What is the Transformer architecture and why is it significant in deep learning?

**Category:** Theory  
**Difficulty:** Easy  
**Tags:** Transformers, Architecture, NLP

**Sample Answer:**
A Transformer is a neural network architecture that relies on self-attention to process sequences in parallel. Introduced by Vaswani et al. (2017), it replaces recurrence and convolution with attention, making training highly parallelizable and enabling state-of-the-art results in language modeling, translation, and many other domains.

---

### Q: How does the self-attention mechanism work in a Transformer? Provide the full key-query-value (KQV) computation and explain multi-head attention.

**Category:** Theory  
**Difficulty:** Medium  
**Tags:** Self-Attention, KQV, Multi-Head

**Sample Answer:**
For each token, three projections are computed: Query (Q), Key (K), and Value (V).

1. Compute dot-product similarity between the query of one token and the keys of all tokens:  
   `scores = Q · Kᵀ / √dₖ`.
2. Apply softmax to obtain attention weights:  
   `α = softmax(scores)`.
3. Produce the output as the weighted sum of value vectors:  
   `output = α · V`.

Multi-head attention repeats this process in parallel with `h` different learned projection matrices, allowing the model to attend to information from multiple representation subspaces.

---

### Q: Compare encoder-only, decoder-only, and encoder-decoder Transformer models. Give typical use-cases for each.

**Category:** Theory  
**Difficulty:** Medium-Hard  
**Tags:** Encoder, Decoder, Architecture

**Sample Answer:**
- **Encoder-only (e.g., BERT):** All tokens attend bidirectionally within a single stack. Best for understanding tasks (classification, NER) where the whole input is available at once.
- **Decoder-only (e.g., GPT):** Uses causal masking so each position attends only to previous tokens. Ideal for autoregressive generation tasks such as open-ended text generation.
- **Encoder-Decoder (e.g., T5, original Transformer):** The encoder builds contextual representations; the decoder attends to both its past outputs and the encoder outputs. Suited for sequence-to-sequence tasks like translation, summarization, or question answering.

---

### Q: Describe common training objectives for Transformers and how inference is performed (including decoding strategies).

**Category:** Theory  
**Difficulty:** Hard  
**Tags:** Training, Inference, Decoding

**Sample Answer:**
**Training Objectives:**  
• *Masked Language Modeling (MLM):* Randomly mask ~15 % of tokens and predict them (BERT, RoBERTa).  
• *Causal Language Modeling (CLM):* Predict the next token given all previous tokens (GPT).  
• *Sequence-to-Sequence:* Predict target sequence given source sequence using cross-entropy loss (T5, MT models).

**Inference & Decoding:**  
During inference, the trained weights are frozen. For generation, tokens are produced autoregressively. Decoding strategies include greedy search, beam search, top-k sampling, nucleus (top-p) sampling, and temperature scaling, each trading off diversity vs. determinism.

---
