# Problem 3: Adam vs. SGD with Momentum (15 pts)

Adam (scalar $w$) at step $t$:
$$
\begin{gather}
m_t = \beta_1 m_{t-1} + (1 - \beta_1)g_t, \quad v_t = \beta_2 v_{t-1} + (1 - \beta_2)g_t^2 \\
\hat{m}_t = \frac{m_t}{1 - \beta_1^t}, \quad \hat{v}_t = \frac{v_t}{1 - \beta_2^t} \\
w_{t+1} = w_t - \alpha \frac{\hat{m}_t}{\sqrt{\hat{v}_t} + \varepsilon}
\end{gather}
$$

---

## 1. First two Adam updates (6 pts)

Assume $m_0 = v_0 = 0$.

### $t = 1$

$$
\begin{gather}
m_1 = (1 - \beta_1)g_1, \quad v_1 = (1 - \beta_2)g_1^2 \\ \\
\hat{m}_1 = \frac{(1-\beta_1)g_1}{1-\beta_1} = g_1, \quad \hat{v}_1 = \frac{(1-\beta_2)g_1^2}{1-\beta_2} = g_1^2 \\ \\
\boxed{w_2 = w_1 - \alpha \frac{g_1}{|g_1| + \varepsilon}}
\end{gather}
$$

### $t = 2$

$$
\begin{gather}
m_2 = (1-\beta_1)(\beta_1 g_1 + g_2), \quad v_2 = (1-\beta_2)(\beta_2 g_1^2 + g_2^2) \\ \\
\hat{m}_2 = \frac{(1-\beta_1)(\beta_1 g_1 + g_2)}{1-\beta_1^2} = \frac{\beta_1 g_1 + g_2}{1+\beta_1} \\ \\
\hat{v}_2 = \frac{(1-\beta_2)(\beta_2 g_1^2 + g_2^2)}{1-\beta_2^2} = \frac{\beta_2 g_1^2 + g_2^2}{1+\beta_2} \\ \\
\boxed{w_3 = w_2 - \alpha \frac{\beta_1 g_1 + g_2}{(1+\beta_1)\left(\sqrt{\frac{\beta_2 g_1^2 + g_2^2}{1+\beta_2}} + \varepsilon\right)}}
\end{gather}
$$

---

## 2. Compare to momentum SGD (5 pts)

Momentum SGD: $u_t = \mu u_{t-1} + g_t$, $w_{t+1} = w_t - \alpha u_t$

### (a) Does effective step size depend on gradient magnitude?

**Momentum SGD:** Yes. $\Delta w = -\alpha u_t$ scales linearly with $|g|$.

**Adam:** No. The ratio $\hat{m}_t / \sqrt{\hat{v}_t} \approx \pm 1$ for consistent gradients, so effective step size is roughly $\alpha$ regardless of gradient scale.

### (b) Scaling gradients by constant $c$

**Momentum SGD:** $\Delta w \to c \cdot \Delta w$ — sensitive to scaling.

**Adam:** $\hat{m}_t \to c \hat{m}_t$ and $\sqrt{\hat{v}_t} \to |c| \sqrt{\hat{v}_t}$, so:
$$\frac{\hat{m}_t}{\sqrt{\hat{v}_t}} \to \text{sign}(c) \cdot \frac{\hat{m}_t}{\sqrt{\hat{v}_t}}$$
Approximately **scale-invariant** (when $|c|\sqrt{\hat{v}_t} \gg \varepsilon$).

### (c) Role of $\varepsilon$

- Prevents division by zero when $\hat{v}_t \approx 0$
- Caps maximum step size at $\alpha/\varepsilon$ for parameters with tiny gradient history

---

## 3. Noisy gradients + sparse features (4 pts)

### (a) Learning-rate adaptation

**Momentum SGD:** Single global $\alpha$ for all parameters.

**Adam:** Per-parameter rates via $\alpha / (\sqrt{\hat{v}_t} + \varepsilon)$. Large-gradient parameters get smaller rates; small-gradient parameters get larger rates.

### (b) Behavior on noisy/sparse data

**Noisy gradients:** Adam's $\hat{v}_t$ tracks variance — high noise increases $\hat{v}_t$, automatically shrinking step size. Momentum SGD has no such mechanism.

**Sparse features:** Rarely-updated parameters have small $\hat{v}_t$, giving them larger effective learning rates when gradients do arrive. This helps sparse features (e.g., word embeddings) learn quickly.
