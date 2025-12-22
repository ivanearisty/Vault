# Cheat Sheet: Log-Likelihood, NLL, and Cross-Entropy

---

## 🎯 Core Concepts: LL ↔ NLL ↔ Cross-Entropy

### Definitions (Unconditional Models)

- **Likelihood** for i.i.d. samples $X=\{x^{(i)}\}_{i=1}^m$ under model $p_{\text{model}}(x;w)$:
$$
\mathcal{L}(w) = \prod_{i=1}^m p_{\text{model}}(x^{(i)};w)
$$

- **Log-Likelihood** (avoids numerical underflow):
$$
\ell(w) = \sum_{i=1}^m \log p_{\text{model}}(x^{(i)};w) = m \cdot \mathbb{E}_{x\sim \hat p_{\text{data}}}\big[\log p_{\text{model}}(x;w)\big]
$$

- **Maximum Likelihood Estimation (MLE) Objective**: Maximize Log-Likelihood (LL), which is equivalent to minimizing Negative Log-Likelihood (NLL).
$$
w^{*} = \arg\max_{w} \ell(w) = \arg\min_{w} \underbrace{-\mathbb{E}_{x\sim\hat p_{\text{data}}}\log p_{\text{model}}(x;w)}_{\text{NLL}}
$$

> [!info] Connection to KL Divergence & Cross-Entropy
> Minimizing NLL is equivalent to minimizing the Cross-Entropy (CE) between the empirical data distribution $\hat p_{\text{data}}$ and the model distribution $p_{\text{model}}$.
> $$
> \mathrm{CE}(\hat p_{\text{data}}, p_{\text{model}}) = \underbrace{\mathrm{KL}(\hat p_{\text{data}} || p_{\text{model}})}_{\text{Minimize this}} + \underbrace{H(\hat p_{\text{data}})}_{\text{Constant w.r.t. } w}
> $$
> Since $H(\hat p_{\text{data}})$ is constant, minimizing CE is equivalent to minimizing KL divergence and maximizing Log-Likelihood. **NLL equals CE** (up to a constant).

---

##  supervised Models (Conditional)

For supervised learning with inputs $x$ and labels $y$, the objective is to minimize the **Negative Log-Likelihood**, which is exactly the **Cross-Entropy Loss** (or **Log Loss**).

> [!success] The Practical Loss Function
> $$
> \mathcal{L}(w) = - \mathbb{E}_{(x,y)\sim\hat p_{\text{data}}} \log p_{\text{model}}(y|x;w)
> $$

### Special Case: MSE as Cross-Entropy

Minimizing **Mean Squared Error (MSE)** in regression is a special case of MLE under a Gaussian conditional model. If you assume $p_{\text{model}}(y|x;w) = \mathcal{N}(y; \mu_w(x), \sigma^2)$, then minimizing NLL is equivalent to minimizing MSE.

---

## 🧠 Information Theory Quick Refs

- **Entropy**: Measures the uncertainty or average number of bits needed to encode a sample from a distribution $P$.
  $$
  H(P) = -\mathbb{E}_{x\sim P}\log P(x)
  $$
- **Binary Entropy**: For a Bernoulli trial with probability $p$.
  $$
  H(p) = -[p\log p + (1-p)\log(1-p)]
  $$

---

## ✍️ Exam Templates

### A) Generic MLE Template

For questions like "find the MLE for parameter $w^*$":

1.  **Write the Likelihood**: Assume i.i.d. samples.
    $$
    \mathcal{L}(w) = \prod_{i=1}^m p_{\text{model}}(x^{(i)};w)
    $$
2.  **Take the Log-Likelihood**: Simplifies math and prevents underflow.
    $$
    \ell(w) = \sum_i \log p_{\text{model}}(x^{(i)};w)
    $$
3.  **Differentiate**: Set the gradient to zero and solve for $w$.
    $$
    \nabla_w \ell(w) = 0 \quad (\text{or equivalently } \nabla_w \text{NLL} = 0)
    $$
4.  **Optimize**: If no closed-form solution exists, use an optimization algorithm like (stochastic) gradient descent on the NLL.

> [!tip] Handy Gradient Form
> The gradient of the NLL is:
> $$
> \nabla_w \text{NLL} = -\frac{1}{m}\sum_{i=1}^m \nabla_w \log p_{\text{model}}(\cdot ;w)
> $$
> (Just plug in the marginal or conditional density).

### B) Conditional Cross-Entropy Formulas

- **Binary Classification** (labels $y \in \{0,1\}$, model output $\hat p = \Pr(y=1|x;w)$):
  $$
  \text{CE} = -\frac{1}{m}\sum_{i=1}^m \big[y^{(i)}\log \hat p^{(i)} + (1-y^{(i)})\log (1-\hat p^{(i)})\big]
  $$
  This is the NLL of a conditional Bernoulli model.

- **Multiclass Classification** (one-hot labels $y$, softmax outputs $\hat p_k = \Pr(y=k|x;w)$):
  $$
  \text{CE} = -\frac{1}{m}\sum_{i=1}^m \sum_{k} y_k^{(i)}\log \hat p_k^{(i)}
  $$

### Gaussian Distribution Parameters

Given i.i.d. samples $x^{(1)}, \dots, x^{(N)}$ from $\mathcal{N}(\mu, \sigma^2)$:
- **Mean**: $\displaystyle \hat\mu = \frac{1}{N}\sum_{n=1}^N x^{(n)}$
- **Variance**: $\displaystyle \hat\sigma^{2} = \frac{1}{N}\sum_{n=1}^N\big(x^{(n)}-\hat\mu\big)^2$

> [!abstract] Derivation Sketch
> The log-likelihood simplifies to:
> $$
> \ell(\mu,\sigma^2) = -\frac{N}{2}\log(2\pi\sigma^2) - \frac{1}{2\sigma^2}\sum_{n=1}^N(x^{(n)}-\mu)^2
> $$
> Setting derivatives $\frac{\partial\ell}{\partial\mu}=0$ and $\frac{\partial\ell}{\partial\sigma^2}=0$ yields the estimators above.

### General Result to Cite

For any supervised model, the loss function you minimize is the **cross-entropy**, which is the **negative conditional log-likelihood** of the model $p_{\text{model}}(y|x;w)$. You don't redesign the loss for a new architecture; you change the model $p_{\text{model}}$.

---

## 📝 Worked Pattern for Exams

**Problem**: "Given a distribution, derive the MLE for its parameters" (e.g., Gaussian $\mu, \sigma^2$)

1.  **Write the Log-Likelihood**:
    $\ell(\mu,\sigma^2) = \sum_{n}\log \mathcal{N}(x^{(n)} \mid \mu,\sigma^2)$
2.  **Differentiate w.r.t. first parameter**:
    $\partial \ell/\partial \mu=0 \implies \hat\mu=\frac{1}{N}\sum_n x^{(n)}$
3.  **Differentiate w.r.t. second parameter**:
    $\partial \ell/\partial \sigma^2=0 \implies \hat\sigma^{2}=\frac{1}{N}\sum_n(x^{(n)}-\hat\mu)^2$
4.  **Conclude**: State that these estimators maximize the Log-Likelihood (and minimize NLL/CE).

