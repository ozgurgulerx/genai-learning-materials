# ML Basics Interview Questions

A starter set of fundamental machine-learning questions for assessing entry-level to mid-level candidates.

---
### Q: What is the difference between **supervised**, **unsupervised**, and **reinforcement** learning?

**Category:** ML Basics  
**Difficulty:** Easy  
**Tags:** Paradigms, Supervised, Unsupervised, RL

**Sample Answer:**
• **Supervised** learning trains a model on labelled data `(X, y)` to predict `y` for new `X`.  
• **Unsupervised** learning finds patterns or structure in un-labelled data `X` (e.g. clustering, dimensionality reduction).  
• **Reinforcement** learning learns via *trial-and-error* interactions with an environment, receiving feedback as rewards to learn a policy that maximises cumulative reward.

---
### Q: Define **overfitting** and describe two common techniques to mitigate it.

**Category:** ML Basics  
**Difficulty:** Easy  
**Tags:** Overfitting, Regularisation

**Sample Answer:**
Overfitting occurs when a model captures noise in training data, performing well on the training set but poorly on unseen data.  Mitigation techniques:  
1. **Regularisation** (e.g. L1/L2 weight penalties, dropout) discourages overly complex models.  
2. **Cross-validation / Early stopping** monitors validation performance and halts training before overfitting intensifies.

---
### Q: Explain the bias-variance trade-off and its implication for model complexity.

**Category:** ML Basics  
**Difficulty:** Medium  
**Tags:** Bias–Variance, Generalisation

**Sample Answer:**
• **Bias** refers to error from simplifying assumptions (under-fitting).  
• **Variance** refers to error from sensitivity to data fluctuations (over-fitting).  
Increasing model complexity decreases bias but increases variance; the optimal model balances the two to minimise total expected error.

---
### Q: What does the **ROC curve** illustrate and what does **AUC** represent?

**Category:** ML Basics  
**Difficulty:** Medium  
**Tags:** Evaluation, ROC, AUC

**Sample Answer:**
An ROC curve plots **True Positive Rate (TPR)** against **False Positive Rate (FPR)** at various classification thresholds.  The **Area Under the Curve (AUC)** summarises the model's ability to rank positives above negatives; 1.0 = perfect, 0.5 = random.

---
### Q: When would you choose **precision** over **recall** as your primary metric? Give an example.

**Category:** ML Basics  
**Difficulty:** Medium  
**Tags:** Precision, Recall, Metrics

**Sample Answer:**
Prioritise **precision** when false positives are costlier than false negatives.  Example: an email spam filter—mis-classifying legitimate mail as spam (false positive) annoys users more than letting occasional spam through.

---
### Q: Describe the difference between **bagging** and **boosting**.

**Category:** ML Basics  
**Difficulty:** Medium-Hard  
**Tags:** Ensemble Methods, Bagging, Boosting

**Sample Answer:**
• **Bagging** (Bootstrap Aggregating) trains multiple models independently on bootstrapped samples and averages their predictions, reducing variance (e.g. Random Forest).  
• **Boosting** trains models sequentially, where each model focuses on correcting errors of the previous ones, reducing bias (e.g. AdaBoost, Gradient Boosting).

---
### Q: Explain **gradient descent** and the role of the **learning rate**.

**Category:** ML Basics  
**Difficulty:** Easy  
**Tags:** Optimisation, Gradient Descent, Learning Rate

**Sample Answer:**
Gradient descent iteratively updates model parameters in the **negative direction of the gradient** of the loss function to minimise it.  The **learning rate** determines step size; too high can overshoot minima, too low slows convergence or stalls in local minima.

---
### Q: What assumptions underlie **linear regression**? Name two diagnostic checks.

**Category:** ML Basics  
**Difficulty:** Medium  
**Tags:** Linear Regression, Assumptions

**Sample Answer:**
Key assumptions: linear relationship, homoscedastic (constant) error variance, errors are independent and normally distributed, no multicollinearity.  Diagnostics:  
• **Residual plot** for non-linearity or heteroscedasticity.  
• **Variance Inflation Factor (VIF)** to detect multicollinearity.

---
### Q: How does **k-means clustering** work and how do you choose *k*?

**Category:** ML Basics  
**Difficulty:** Medium  
**Tags:** Unsupervised, Clustering, k-means

**Sample Answer:**
K-means alternates between assigning each point to the nearest centroid and recalculating centroids as the mean of assigned points until convergence.  Choosing *k*: techniques include the **elbow method** (plot within-cluster SSE vs k) or **silhouette score** to balance compactness and separation.

---
### Q: Compare **L1** and **L2** regularisation.

**Category:** ML Basics  
**Difficulty:** Medium  
**Tags:** Regularisation, L1, L2

**Sample Answer:**
• **L1 (Lasso)** adds `λ|w|` to the loss, promoting sparsity and feature selection by driving some weights to zero.  
• **L2 (Ridge)** adds `λw²`, shrinking weights smoothly toward zero but rarely exactly zero, reducing variance but retaining all features.

---

### Q: Explain the **kernel trick** in Support Vector Machines (SVMs). How do the hyper-parameters **C** and **γ** (gamma) influence the decision boundary?

**Category:** ML Basics  
**Difficulty:** Hard  
**Tags:** SVM, Kernel Trick, Hyper-parameters

**Sample Answer:**
The **kernel trick** implicitly maps data into a high-dimensional space via a kernel function `k(x, x′) = φ(x) · φ(x′)`, enabling SVMs to learn non-linear separators without computing φ explicitly.  
• **C (regularisation)** controls the trade-off between maximising margin and minimising classification error. Small *C* → wider margin, more tolerance to mis-classification (higher bias); large *C* → narrower margin, fits training data closely (higher variance).  
• **γ** in the RBF kernel sets the radius of influence of support vectors. Small *γ* yields smooth boundaries; large *γ* produces complex, wiggly boundaries that risk over-fitting.

---

### Q: Derive why the first **principal component** in PCA maximises projected variance and relate this to the largest eigenvalue of the covariance matrix.

**Category:** ML Basics  
**Difficulty:** Very Hard  
**Tags:** PCA, Eigenvalues, Linear Algebra

**Sample Answer:**
For zero-mean data matrix `X ∈ ℝ^{n×d}`, projecting onto unit vector *w* gives variance `Var(Xw) = wᵀΣw`, where `Σ = (1/n) XᵀX`.  Maximising this Rayleigh quotient under `‖w‖₂ = 1` yields eigenvector *v₁* with largest eigenvalue `λ₁` of Σ. Thus the first principal component is the direction of maximum variance, and that maximum variance equals `λ₁`.

---

### Q: Outline the **Expectation–Maximisation (EM)** algorithm for a Gaussian Mixture Model (GMM). Why does log-likelihood increase at each iteration?

**Category:** ML Basics  
**Difficulty:** Hard  
**Tags:** EM, GMM, Maximum Likelihood

**Sample Answer:**
1. **Initialise** parameters θ⁰ = {πₖ, μₖ, Σₖ}.  
2. Repeat until convergence:  
   • **E-step:** Compute responsibilities `γᵢₖ = P(k | xᵢ, θᵗ)`.  
   • **M-step:** Update parameters with closed-form maxima:  
     `πₖ = (1/n) Σᵢ γᵢₖ`,  `μₖ = (Σᵢ γᵢₖ xᵢ)/(Σᵢ γᵢₖ)`,  `Σₖ = (Σᵢ γᵢₖ (xᵢ−μₖ)(xᵢ−μₖ)ᵀ)/(Σᵢ γᵢₖ)`.  
EM alternates between tightening a lower bound (E-step) and maximising it (M-step); this monotonic property ensures the observed-data log-likelihood **never decreases**.

---

### Q: Provide the **bias–variance decomposition** of Mean Squared Error (MSE) and interpret each term.

**Category:** ML Basics  
**Difficulty:** Hard  
**Tags:** Bias–Variance Decomposition, MSE

**Sample Answer:**
For target `y = f(x) + ε` with `E[ε] = 0`, expected test MSE is:  
`E[(ŷ(x) − y)²] = (E[ŷ(x)] − f(x))²  +  Var[ŷ(x)]  +  σ²_ε`  
= **Bias²** + **Variance** + **Irreducible noise**.  Increasing model complexity lowers bias but can raise variance; the sum determines generalisation error.

---

### Q: What is the **curse of dimensionality** and how does it affect distance-based methods like **k-Nearest Neighbours (k-NN)**?

**Category:** ML Basics  
**Difficulty:** Hard  
**Tags:** Curse of Dimensionality, k-NN, Distance Metrics

**Sample Answer:**
As dimensionality *d* grows, volume expands exponentially and data become sparse; distances between points concentrate, so the nearest neighbour is nearly as far as the farthest. For **k-NN**, this degrades discrimination power, inflates variance, and demands exponentially more data. Remedies include feature selection, dimensionality reduction (PCA), or algorithms less reliant on raw Euclidean distance.

---
