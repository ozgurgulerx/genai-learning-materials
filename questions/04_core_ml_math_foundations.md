# Core ML & Math Foundations

This section contains rubric, interview questions with sample answers, and a blind scenario assessment for evaluating candidates' mastery of core machine-learning principles and mathematical underpinnings that are critical for modern generative-AI systems.

---

## Rubric – Core ML & Math Foundations

* **Green-light:** Strong grasp of ML basics and mathematical underpinnings. Explains concepts like probability distributions, loss functions, gradient descent, etc., clearly and correctly. Connects fundamental principles (e.g. bias–variance trade-off, likelihood maximisation) to generative-model scenarios and uses correct terminology with concrete examples.
* **Yellow-light:** Basic understanding of core concepts but with minor confusion or gaps. Can recall definitions yet struggles to apply them to generative-AI contexts or relies on memorised answers without deeper insight.
* **Red-light:** Significant gaps in fundamental knowledge. Cannot clearly explain key ML concepts or shows shaky mathematical reasoning.  **Red flag:** Claiming advanced generative-AI expertise while unable to articulate basic ML principles.
* **Follow-up probes:** “Can you give a simple example or analogy for that concept?” · “How would this fundamental concept apply if we doubled the model or data size?” · “What happens when training data is very small?”

---

## Interview Questions – Core ML & Math Foundations

### Q: What are **generative models** and how do they differ from **discriminative models**?

**Category:** Core ML & Math Foundations  
**Difficulty:** Easy–Medium  
**Tags:** Generative vs Discriminative, Probability, Joint vs Conditional

**Sample Answer:**
Generative models learn the **joint probability distribution** `P(X)` (or `P(X, Y)`) of the data and can therefore **sample new data** points that resemble the training set.  Discriminative models learn the **conditional distribution** `P(Y | X)` and are optimised for making predictions or decisions, not for creating data.  
• Example generative models: Variational Auto-Encoders (VAEs), Generative Adversarial Networks (GANs), GPT--4.  
• Example discriminative models: Logistic regression, Random Forest classifiers, BERT fine-tuned for classification.  
Intuitively, generative = “What does valid data look like?” whereas discriminative = “Given data, what is the correct label?”.

**Red-flag answers:** Confusing the two (“generative models classify data”), giving no examples, or failing to mention probability distributions.

---

### Q: What **loss function** is commonly used to train large language models, and *why*?

**Category:** Core ML & Math Foundations  
**Difficulty:** Easy  
**Tags:** Cross-entropy, Negative Log-Likelihood, Language Modelling

**Sample Answer:**
Most generative language models are trained with **cross-entropy loss** (equivalently, the negative log-likelihood).  The model outputs a probability distribution over the vocabulary for the next token; cross-entropy penalises low probability assigned to the correct token and thus **maximises the likelihood** of the training data.  Mathematically: `L = -Σ log p_model(token_true)`.  It is differentiable, works naturally with softmax outputs, and aligns optimisation directly with predicting correct tokens.

**Red-flag answers:** Suggesting mean-squared error without justification, or displaying no understanding of cross-entropy / likelihood.

---

### Q: In a **Variational Auto-Encoder (VAE)**, what is the **re-parameterisation trick** and why is it required?

**Category:** Core ML & Math Foundations  
**Difficulty:** Medium–Hard  
**Tags:** VAE, Re-parameterisation, Gradient Flow

**Sample Answer:**
The encoder outputs the mean `μ` and standard deviation `σ` for a latent normal distribution.  Directly sampling `z ~ N(μ, σ²)` is **non-differentiable**, blocking back-propagation.  The **re-parameterisation trick** instead samples `ε ~ N(0, 1)` and sets `z = μ + σ * ε`.  Because `ε` is independent noise, `z` becomes a deterministic differentiable function of `μ` and `σ`, allowing gradients to flow through the sampling step and enabling end-to-end training of the VAE.

**Red-flag answers:** “Just sample normally and back-propagate”, or confusing the concept with reinforcement-learning tricks.

---

### Q: Explain the **bias–variance trade-off** in ML.  How does it manifest when training a *very large generative model on a limited dataset*?

**Category:** Core ML & Math Foundations  
**Difficulty:** Medium  
**Tags:** Bias–Variance, Overfitting, Model Capacity

**Sample Answer:**
• **Bias** = error from overly simplistic assumptions → under-fitting.  
• **Variance** = error from sensitivity to fluctuations in training data → over-fitting.  
A huge model has capacity to **memorise** a small dataset (low bias) but generalises poorly (high variance).  Symptoms: training loss plummets while validation loss stagnates or worsens.  Mitigations include stronger regularisation, early stopping, using a smaller model, or—best—adding more data.

**Red-flag answers:** Claiming “always use the biggest model” or not recognising over-fitting risk with small data.

---

### Q: What are **scaling laws** in modern AI, and how would you use them when building a generative model?

**Category:** Core ML & Math Foundations  
**Difficulty:** Hard  
**Tags:** Scaling Laws, Power-law, Data & Model Size

**Sample Answer:**
**Scaling laws** are empirical power-law relationships showing how performance (e.g. loss or perplexity) improves predictably as you scale *parameters*, *data*, or *compute*.  For example, test loss often decreases roughly as `loss ∝ N^-α` on a log-log plot until other bottlenecks dominate.  Practically, scaling-law curves let you forecast returns from doubling model size, warn when data becomes limiting (e.g. Chinchilla work), and guide budget allocation between bigger models versus more tokens.

**Red-flag answers:** Equating scaling laws to Moore’s law or only infrastructure scaling; senior candidates should be familiar with the research.

---

## Blind Scenario – Core ML & Math Foundations

**Scenario:** You must build a small generative text model for a niche domain (≈ 50 k sentences of legal documents).  Whiteboard the end-to-end plan—from data preprocessing through model selection, training, evaluation, and over-fitting mitigation.

**Scorer Checklist:**
* Mentions **data preparation** (cleaning, domain-appropriate tokenisation, train/val split).
* Chooses an appropriate **model** given limited data (e.g. fine-tune a pre-trained LM or train a small Transformer/LSTM).
* Acknowledges **over-fitting risk** and proposes transfer learning, regularisation, or smaller capacity.
* Describes **training process** (cross-entropy, validation monitoring, early stopping, LR schedule).
* Specifies **evaluation** (perplexity plus qualitative domain expert review).
* Provides **mitigation strategies** (data augmentation, smaller model, more data, or regularisation).
* **Red flags:** Proposing to train a GPT-3-scale model from scratch on 50 k sentences or skipping evaluation entirely.

---
