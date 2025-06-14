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
The softmax converts logits to a valid probability distribution:

```
pₜ = softmax(logitsₜ) = exp(logitsₜ) / Σ exp(logitsₜ)
```

**Manipulating Logits During Decoding**  
1. **Temperature Scaling (τ):**  
   Divide logits by τ before softmax.  
   • τ < 1 → sharper distribution (more deterministic).  
   • τ > 1 → flatter distribution (more diverse).
2. **Logit Bias / Logit Additions:**  
   Add a vector δ to boost or suppress specific tokens. Setting δ = −∞ for a token bans it entirely.
3. **Repetition & Presence Penalties:**  
   Lower logits of tokens already generated to reduce loops:  
   `logits ← logits − penalty × f(freq(token))`.
4. **Top-k / Top-p Filtering:**  
   After any scaling, mask out all but the top-k tokens, or those in the nucleus set with cumulative probability ≥ p, before sampling.
5. **Typical & Top-a Sampling:**  
   Additional schemes modify logits based on entropy or annealing schedules to balance diversity and coherence.

**Why Understanding Logits Matters**  
• All decoding tricks ultimately modify logits—mastering them lets you fine-tune generation without retraining.  
• Logit “lenses” can inspect intermediate layers by mapping hidden states directly to vocab logits for interpretability.  
• Hard constraints (e.g., forcing special tokens) are implemented by directly setting or masking logits.

---
