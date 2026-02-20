# Problem 1: Warmup — Softmax + Matrix Gradients (15 pts)

## 1. Temperature-scaled softmax Jacobian (6 pts)

Let $z \in \mathbb{R}^n$ and define:
$$y_i = \text{softmax}_\tau(z)_i = \frac{e^{z_i/\tau}}{\sum_{j=1}^{n} e^{z_j/\tau}}, \quad \tau > 0$$

---

Let $S = \sum_{j=1}^{n} e^{z_j/\tau}$. Then $y_i = e^{z_i/\tau}/S$.

$$
\begin{gather}
\frac{\partial y_i}{\partial z_k} = \frac{\frac{1}{\tau} e^{z_i/\tau} \cdot \mathbf{1}[i = k] \cdot S - e^{z_i/\tau} \cdot \frac{1}{\tau} e^{z_k/\tau}}{S^2} \\ \\
= \frac{1}{\tau} \left( \frac{e^{z_i/\tau}}{S} \cdot \mathbf{1}[i = k] - \frac{e^{z_i/\tau}}{S} \cdot \frac{e^{z_k/\tau}}{S} \right) \\ \\
= \frac{1}{\tau} \left( y_i \cdot \mathbf{1}[i = k] - y_i y_k \right)
\end{gather}
$$

The term $y_i \cdot \mathbf{1}[i=k]$ forms $\text{diag}(y)$; the term $y_i y_k$ forms $yy^\top$.

$$\boxed{\frac{\partial y}{\partial z} = \frac{1}{\tau}\left(\text{diag}(y) - yy^\top\right)}$$

---

## 2. Gradient w.r.t. A and b in affine least-squares (6 pts)

Let $A \in \mathbb{R}^{d \times m}$, $b \in \mathbb{R}^d$, $x \in \mathbb{R}^m$, $t \in \mathbb{R}^d$ (fixed).

$$f(A, b) = \frac{1}{2}\|Ax + b - t\|_2^2$$

---

Let $r = Ax + b - t$. Since $f = \frac{1}{2}r^\top r$:

$$
\begin{gather}
\nabla_b f = \frac{\partial f}{\partial r} \cdot \frac{\partial r}{\partial b} = r \cdot 1 \\ \\
\boxed{\nabla_b f = Ax + b - t}
\end{gather}
$$

For $\nabla_A f$: $\frac{\partial f}{\partial A_{ij}} = r_i \cdot x_j$, which gives the outer product:

$$\boxed{\nabla_A f = (Ax + b - t)x^\top}$$

---

## 3. Softmax vs. Sigmoid: when and why? (3 pts)

**Softmax — multi-class classification** (e.g., classifying an image as cat/dog/bird):
- Classes are mutually exclusive; softmax outputs sum to 1
- Sigmoid would allow high probability for multiple classes, violating exclusivity

**Sigmoid — multi-label classification** (e.g., tagging a movie as action/comedy/romance):
- Labels are independent; a movie can have multiple genres
- Softmax would force probabilities to sum to 1, incorrectly constraining predictions

| Aspect | Softmax | Sigmoid |
|--------|---------|---------|
| Output constraint | $\sum_i p_i = 1$ | Each $p_i \in (0, 1)$ independently |
| Loss function | Categorical cross-entropy | Binary cross-entropy per label |
