### Q: How do positional encodings work in Transformers, and why are they needed?

**Category:** Theory  
**Difficulty:** Easy  
**Tags:** Positional Encoding, Sinusoidal, Learnable

**Sample Answer:**
Because self-attention is permutation-invariant, Transformers need a way to inject token order. Positional encodings add or concatenate a vector that represents each position:

1. **Sinusoidal (Original Paper):**  
   • Uses fixed sin/cos waves of different frequencies:  
   `PE(pos,2i)=sin(pos/10000^{2i/d_model})`  
   `PE(pos,2i+1)=cos(pos/10000^{2i/d_model})`  
   • Enables extrapolation to sequences longer than training.
2. **Learnable Embeddings:** Train an embedding table indexed by position. Simpler but cannot naturally extrapolate.
3. **Relative / Rotary (RoPE):** Encode *distance* rather than absolute index, improving long-range generalization.

Without positional information, all tokens would be treated as a bag of words, losing syntax and semantics tied to order.

---

### Q: What is multi-head attention, and how does it improve over single-head attention?

**Category:** Theory  
**Difficulty:** Medium  
**Tags:** Multi-Head Attention, Representation Learning

**Sample Answer:**
Multi-head attention runs *h* independent self-attention operations ("heads") in parallel, each with its own projection matrices. The outputs are concatenated and linearly mixed.

Benefits:
• **Sub-space Focus:** Each head can learn to attend to different linguistic patterns (syntax, long-range dependencies).  
• **Higher Rank Mapping:** Multiple heads increase the representational capacity beyond a single dot-product similarity.  
• **Gradient Flow:** Heads act as parallel paths, aiding optimization stability.
Empirically, h≈8-16 for BERT-base; scaling laws suggest keeping head dimension ≈64.

---

### Q: Explain the purpose of the position-wise feed-forward network (FFN) block inside each Transformer layer.

**Category:** Theory  
**Difficulty:** Medium  
**Tags:** Feed-Forward Network, GLU, Transformer Layer

**Sample Answer:**
After self-attention mixes information across tokens, the FFN applies non-linear transformations to each token *independently*:
`FFN(x)=W₂· σ(W₁·x + b₁) + b₂` where `σ` is usually GELU.

Roles:
1. **Token-wise Feature Expansion:** First projection (d_model→d_ff, typically 4×) lets the model learn richer local transformations.
2. **Non-Linearity:** Adds depth and expressivity not captured by the linear attention step.
3. **Modular Separation:** Attention handles *interaction*; FFN handles *processing* of aggregated context.
Variants include SwiGLU/Gated-Linear-Units and Conv-former using 1-D convolutions.

---

### Q: Describe the role of residual connections in Transformer architectures.

**Category:** Theory  
**Difficulty:** Easy  
**Tags:** Residual, Skip Connection, Optimization

**Sample Answer:**
Each sub-layer (attention or FFN) is wrapped as `x + Sublayer(LayerNorm(x))`.

Benefits:
• **Mitigate Vanishing Gradients:** Provides short paths for gradient flow, enabling deep stacks.  
• **Feature Reuse:** Allows layers to refine, not overwrite, earlier representations.  
• **Stability:** Combined with LayerNorm, residuals help maintain signal magnitude.

Without residuals, training 24+ layer Transformers would be significantly harder.

---

### Q: What are Mixture-of-Experts (MoE) Transformers, and what advantages and challenges do they present?

**Category:** Theory  
**Difficulty:** Hard  
**Tags:** MoE, Sparse Routing, Scaling Efficiency

**Sample Answer:**
MoE layers replace a dense FFN with *E* expert FFNs. A router selects a small subset (typically 1-4) for each token, creating sparsity.

Advantages:
• **Parameter Growth with Sub-linear FLOPs:** You can scale to trillions of parameters while keeping per-token compute similar to dense models.  
• **Specialization:** Experts can learn domain-specific transformations (e.g., code vs. natural language).

Challenges:
• **Load Balancing:** Router must prevent some experts from being over/under-utilized; typically addressed with auxiliary losses.  
• **Memory & Bandwidth:** Routing tensors across devices increases communication overhead.  
• **Stability:** Expert dropout and noisy gating are needed to avoid collapse.  
• **Serving Complexity:** Dynamic routing complicates inference caching and hardware utilization.

---

### Q: Transformers sometimes exhibit positional or frequency-based attention bias. What causes this and how can it be mitigated?

**Category:** Theory  
**Difficulty:** Hard  
**Tags:** Attention Bias, Rotary Embeddings, Masking

**Sample Answer:**
**Causes:**  
1. **Dot-Product Scaling:** Near-diagonal tokens often have higher similarity, leading to short-range preference.  
2. **Training Data Distribution:** Natural language has local dependencies; models inherit this prior.  
3. **Positional Encoding Choice:** Absolute sinusoids encode proximity explicitly; frequencies attenuate with distance.

**Mitigations:**  
• **Relative/Rotary Encodings (RoPE, ALiBi):** Represent distance explicitly, enabling attention to farther positions.  
• **Sparse & Long-Range Attention:** Patterns like dilated or random attention force the model to look beyond neighbors.  
• **Attention Masking or Bias Terms:** Add learned bias favoring specific positions or penalizing overly local attention.  
• **Curriculum & Data Augmentation:** Train on tasks requiring long-range reasoning.

Addressing bias improves performance on tasks like long-document QA and code understanding.

---
