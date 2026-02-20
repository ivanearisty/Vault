# Problem 2: Linear Regression with Huber Loss (15 pts)

Residuals: $r_i(w) = y_i - w^\top x_i$, where $w, x_i \in \mathbb{R}^d$, $y_i \in \mathbb{R}$.

Huber loss:
$$
\ell_\delta(r) = \begin{cases}
\frac{1}{2}r^2 & |r| \leq \delta \\
\delta\left(|r| - \frac{1}{2}\delta\right) & |r| > \delta
\end{cases}
$$

Objective: $L_\delta(w) = \sum_{i=1}^{n} \ell_\delta(r_i(w))$

---

## 1. Gradient derivation (6 pts)

$$
\frac{\partial \ell_\delta}{\partial r} = \begin{cases}
r & |r| \leq \delta \\
\delta \cdot \text{sign}(r) & |r| > \delta
\end{cases} = \text{clip}(r, -\delta, \delta)
$$

Since $\frac{\partial r_i}{\partial w} = -x_i$, by chain rule:

$$
\boxed{\nabla_w L_\delta(w) = -\sum_{i=1}^{n} \text{clip}(r_i(w), -\delta, \delta) \cdot x_i}
$$

---

## 2. Optimal w: closed form or not? (4 pts)

**No closed-form solution.**

For OLS, $\nabla_w L = 0$ gives $X^\top Xw = X^\top y \implies w^* = (X^\top X)^{-1}X^\top y$.

For Huber, the gradient involves $\text{clip}(r_i(w), -\delta, \delta)$, which is **nonlinear in $w$** — whether $|r_i| \leq \delta$ depends on $w$ itself. We cannot isolate $w$ algebraically.

Use numerical methods: IRLS, gradient descent, or L-BFGS (all work well since Huber is convex and smooth).

---

## 3. Add L2 regularization (3 pts)

$$\tilde{L}(w) = \sum_{i=1}^{n} \ell_\delta(r_i(w)) + \frac{\lambda}{2}\|w\|_2^2$$

$$
\boxed{\nabla_w \tilde{L}(w) = -\sum_{i=1}^{n} \text{clip}(r_i(w), -\delta, \delta) \cdot x_i + \lambda w}
$$

**Effect of $\lambda$:**
- Shrinks weights toward zero, adding stability beyond Huber's outlier robustness
- Ensures unique solution when $d > n$ (underdetermined case)
- Larger $\lambda$ increases bias but reduces variance

---

## 4. ML pipeline reflection (2 pts)

**(i) Optimization:** Huber is smooth and convex (unlike L1), so gradient methods converge reliably. The bounded gradient ($\leq \delta$) prevents explosive updates from outliers.

**(ii) Evaluation:** The learned $w$ is less influenced by training outliers, improving generalization. The parameter $\delta$ controls the transition: small $\delta$ → median regression (L1-like), large $\delta$ → mean regression (L2-like).
