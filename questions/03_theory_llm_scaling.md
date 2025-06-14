### Q: How do Transformer architectures scale into Large Language Models (LLMs), and what design and training practices enable their emergent capabilities?

**Category:** Theory  
**Difficulty:** Very Hard  
**Tags:** LLM, Scaling Laws, Pre-training, Distributed Training

**Sample Answer:**
Large Language Models (LLMs) such as GPT-4, PaLM, and Llama 3 are essentially very large Transformer decoders trained on web-scale corpora. Their success comes from scaling along three axes—model size, data size, and compute—combined with engineering innovations that make such scaling feasible.

1. **Scaling Laws**  
   Empirical studies (Kaplan et al., 2020; Hoffmann et al., 2022) reveal predictable power-law relationships between test loss and (parameters N, tokens D, compute C). These *scaling laws* guide practitioners to allocate compute roughly as `C ≈ 6·N·D` for near-optimal performance.

2. **Architectural Tweaks**  
   • *Decoder-only stack* with causal masking for autoregressive training.  
   • *Pre-LayerNorm / RMSNorm* improves stability beyond 100 B parameters.  
   • *Sparse & Mixture-of-Experts (MoE) layers* route tokens through small subsets of parameters (Switch, GLaM, Mixtral), achieving trillion-parameter models without linear cost.  
   • *Rotary / ALiBi* positional encodings extend context length to 32 k+ tokens.

3. **Tokenizer & Vocabulary**  
   Sub-word or byte-pair encodings (BPE, SentencePiece) keep the vocabulary ≈ 32-128 k, trading off OOV risk and embedding size. Newer byte-level BPE avoids language-specific heuristics.

4. **Data Pipeline**  
   • *Web-scale corpora* (CommonCrawl, books, code, academic papers) are cleaned, deduplicated, and shuffled.  
   • *Quality mixing / temperature sampling* balances high-quality sources (Wikipedia) with diverse long-tail data.  
   • *Continual pre-training* refreshes knowledge with new snapshots or synthetic data.

5. **Distributed Training**  
   • *Data Parallelism* (DDP) to split batches across GPUs.  
   • *Tensor / Megatron Parallelism* shards weight matrices.  
   • *Pipeline Parallelism* stages layers across nodes.  
   • *ZeRO / FSDP* shards optimizer states and gradients to fit > 100 B parameters.  
   • *Gradient Check-pointing & Mixed Precision* (fp16 / bf16) save memory and bandwidth.

6. **Optimization & Regularization**  
   AdamW or LAMB with cosine LR decay, warm-up, gradient clipping, and large effective batch sizes (0.5-4 M tokens/step).  
   *Learning-rate scaling* roughly follows `lr ∝ batch_size^0.5`.  
   Weight decay, dropout, and attention dropout mitigate over-fitting.

7. **Emergent Capabilities**  
   Once scale surpasses ≈ 10-50 B parameters and 100-200 B tokens, new behaviors—few-shot in-context learning, chain-of-thought reasoning, coding ability—emerge that smaller models lack.

8. **Post-training Alignment**  
   • *Instruction Tuning* on curated task sets (FLAN, Alpaca, LIMA).  
   • *RLHF / RLAIF / DPO* uses human or AI preferences to shape responses.  
   • *Tool Use & Retrieval-Augmented Generation* (RAG) extends factual accuracy without bloating the base model.

9. **Inference & Serving**  
   • *KV-cache* stores past key/value tensors, giving O(1) complexity per new token.  
   • *Speculative decoding*, *flash attention 2*, and GPU-CPU off-loading reduce latency.  
   • *Quantization* (8-bit, 4-bit), *LoRA* and *QLoRA* adapters enable edge or on-device deployment.

10. **Challenges & Open Problems**  
    Power consumption, carbon footprint, data contamination, hallucination, alignment, and intellectual-property rights remain active research areas despite impressive capabilities.

In short, LLMs are colossal yet carefully engineered Transformers whose power derives from principled scaling, vast data, sophisticated distributed systems, and iterative alignment techniques.

---

### Q: In Large Language Models, what are logits, how are they computed, and how are they manipulated during decoding (e.g., temperature scaling, logit bias, token banning)? Provide both an intuitive overview and deeper mathematical details.

**Category:** Theory  
**Difficulty:** Medium  
**Tags:** Logits, Softmax, Temperature, Logit Bias, Decoding

**Sample Answer:**
**High-Level Intuition**  
Logits are the raw, unnormalized scores a model assigns to every token before converting them to probabilities. They represent how “confident” the model is in each token at that step.

**Where Do Logits Come From?**  
After the final Transformer layer, the hidden state for position *t* (`hₜ ∈ ℝ^{d_model}`) is projected onto the vocabulary via the output matrix `W_out ∈ ℝ^{V×d_model}` (often tied to the input embeddings) and bias `b`:

```
logitsₜ = W_out · hₜ + b      # Shape: (V,)
```
where *V* is vocabulary size (e.g., 50 k).

**From Logits to Probabilities**

```
pₜ = softmax(logitsₜ) = exp(logitsₜ) / Σ exp(logitsₜ)
```

**Manipulating Logits During Decoding**
1. **Temperature Scaling (τ):**  Divide logits by τ before softmax. τ < 1 sharpens distribution; τ > 1 flattens it.
2. **Logit Bias / Additions:**  Add vector δ to boost or suppress specific tokens; δ = −∞ bans a token.
3. **Repetition & Presence Penalties:** Lower logits of tokens already generated to reduce loops.
4. **Top-k / Top-p Filtering:** Mask out all but the top-k or nucleus set before sampling.
5. **Typical & Top-a Sampling:** Further schemes modify logits based on entropy or annealing.

**Why Understanding Logits Matters**
• All decoding tricks ultimately modify logits—mastering them lets you fine-tune generation without retraining.  
• Logit “lenses” can inspect intermediate layers for interpretability.  
• Hard constraints (forcing special tokens) are implemented by direct logit manipulation.

---

### Q: What decoding hyperparameters (e.g., temperature, top-k, top-p/nucleus sampling, repetition penalty) control text generation in LLMs, and how do they affect diversity and coherence? Provide explanations that deepen progressively.

**Category:** Theory  
**Difficulty:** Hard  
**Tags:** Decoding, Temperature, Top-k, Nucleus Sampling

**Sample Answer:**
Interviewers may explore decoding settings from basic definitions to nuanced trade-offs.

**1. Entry-Level Concepts**  
• **Greedy Search:** Always picks the highest-probability token; deterministic but can be repetitive.  
• **Temperature (τ):** Scales logits before softmax.  
  – τ < 1 sharpens distribution → more deterministic.  
  – τ > 1 flattens distribution → more diverse but riskier.

**2. Intermediate Depth**  
• **Top-k Sampling:** Keep only the k highest-probability tokens, renormalize, then sample.  
  – Small k (e.g., 10) = focused, safer.  
  – Large k (e.g., 1000) ≈ pure sampling.  
• **Top-p / Nucleus Sampling:** Choose the smallest set of tokens whose cumulative probability ≥ p (e.g., 0.9) and sample.  
  – Adapts selection set per step; balances diversity and quality better than fixed k.

**3. Advanced Considerations**  
• **Typical Sampling:** Select tokens whose log-prob is within a certain KL threshold of the expected entropy; maximizes “typicality” vs. probability alone.  
• **Repetition Penalty / Presence Penalty:** Down-weights tokens already generated to reduce loops and encourage novelty.  
• **Top-a (Annealed) Sampling:** Dynamically lowers k or p as generation proceeds to converge endings.  
• **Beam Search & Beam Diversity:** Explores multiple hypotheses; high beams improve factual tasks but can reduce creativity.  
• **Candidate Filtering Order:** Usually apply repetition penalty → temperature → top-k/top-p to avoid masking all tokens.

**4. Practical Tuning Guidelines**  
• Start with τ=0.7, top-p=0.9, no top-k.  
• For deterministic QA, use greedy or τ≈0.2, top-p≈0.6.  
• For creative writing, raise τ to 1.0-1.2 and/or top-p to 0.95-0.98.  
• Combine repetition penalty (1.1-1.2) with top-p to curb loops without stifling creativity.

**Why It Matters:**  
Decoding hyperparameters are runtime controls—unlike training hyperparameters—they shape user experience, balancing correctness, diversity, and safety without retraining the model.

---


