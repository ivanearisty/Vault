# [[Deep Neural Networks]] — Midterm Cheat Sheet

**Why are dominant features bad?**

If one of the features of my input dominates the others, the gradient will be pointing towards dominating feature direction. then the gradient will be shooting towards that feature only. this is bad because then we can't move around that much to find a better local minimum.

## 1. Forward Pass, Shapes, and Parameter Counts

**Layer Equations ($L$ layers):**
- **Linear Combination:** $z^{[l]} = W^{[l]} a^{[l-1]} + b^{[l]}$, with initial activation $a^{[0]}=x$.
- **Activation:** $a^{[l]} = \phi^{[l]}(z^{[l]})$.
- **Output:**
    - **Regression:** $\hat{y}=a^{[L]}$
    - **Classification (Logits):** $o=z^{[L]}$, then $\hat{y}=\mathrm{softmax}(o)$ or $\hat{y}=\sigma(o)$ for binary.

**Shapes:**
If input activation $a^{[l-1]}\in\mathbb{R}^{n_{l-1}}$ and output activation $a^{[l]}\in\mathbb{R}^{n_l}$, then:
- **Weights:** $W^{[l]} \in \mathbb{R}^{n_l\times n_{l-1}}$
- **Biases:** $b^{[l]}\in\mathbb{R}^{n_l}$
- **Pre-activation:** $z^{[l]}\in\mathbb{R}^{n_l}$

**Parameter Count:**
- **Per Layer:** $\#\theta_l = n_l \cdot n_{l-1} + n_l$
- **Total:** $\sum_{l=1}^L (n_l n_{l-1} + n_l)$

> [!TIP] Exam Quick-Checks
> - Confirm the input dimension of a layer matches the previous layer’s output width.
> - Sum all weights **and** biases; don’t forget the output layer.
> - For a batch of size $m$, activation matrices have the shape $A^{[l]}\in\mathbb{R}^{n_l\times m}$.

---

## 2. Common Loss Functions

- **MSE (Regression):**
  $$\mathcal{L}=\frac{1}{m}\sum_i \| \hat{y}^{(i)}-y^{(i)}\|^2$$
- **Binary Cross-Entropy (for Sigmoid output):**
  $$\mathcal{L}=-\frac{1}{m}\sum_i \left[y^{(i)}\log \hat{y}^{(i)} + (1-y^{(i)})\log(1-\hat{y}^{(i)})\right]$$
- **Categorical Cross-Entropy (for Softmax output):**
  $$\mathcal{L}=-\frac{1}{m}\sum_i \log \frac{e^{o_{y_i}^{(i)}}}{\sum_k e^{o_k^{(i)}}}$$
  > [!WARNING] Numerical Safety
  > Use the log-sum-exp trick: $\log\sum_k e^{o_k} = c + \log \sum_k e^{o_k-c}$, where $c=\max_k o_k$.

---

## 3. Backpropagation Essentials

For a given loss $\mathcal{L}$ and activation function $\phi$:

- **Output Layer Gradient (Softmax + CE):** The gradient of the loss with respect to the output logits is remarkably simple.
  $$\delta^{[L]}=\nabla_{o}\mathcal{L}=\hat{y}-y$$
- **Hidden Layer Gradient ("Error"):** Propagate the error backward through the network.
  $$\delta^{[l]}=\left(W^{[l+1]}\right)^\top \delta^{[l+1]} \odot \phi' \left(z^{[l]}\right)$$

**Parameter Gradients:**
- **Weights:** $\displaystyle \frac{\partial \mathcal{L}}{\partial W^{[l]}}=\delta^{[l]}(a^{[l-1]})^\top$
- **Biases:** $\displaystyle \frac{\partial \mathcal{L}}{\partial b^{[l]}}=\sum_{\text{batch}}\delta^{[l]}$

> [!NOTE] Derivative Snippets (Memorize)
> - **ReLU:** $\phi(z)=\max(0,z) \implies \phi'(z)=\mathbb{1}[z>0]$
> - **Sigmoid:** $\sigma'(z)=\sigma(z)(1-\sigma(z))$
> - **Tanh:** $\tanh'(z)=1-\tanh^2(z)$

---

## 4. Initialization & Activation Functions

**He & Glorot (Xavier) Initialization:**
- **For ReLU-family:** $W_{ij}\sim \mathcal{N}\left(0,\frac{2}{n_{l-1}}\right)$ (**He**)
- **For Tanh/Sigmoid:** $W_{ij}\sim \mathcal{N}\left(0,\frac{2}{n_{l-1}+n_l}\right)$ (**Glorot**)

> [!QUESTION] Why use these?
> To keep the variance of activations and gradients roughly constant across layers, which prevents the vanishing or exploding gradients problem during training of deep networks.

> [!INFO] Activation Choices
> - **ReLU**: Default choice. Fast, promotes sparse gradients. Pair with He initialization.
> - **Leaky / Parametric ReLU**: Fixes the "dying ReLU" problem.
> - **GELU**: A smoother version of ReLU, often performs well in deep networks like Transformers.
> - **Avoid Sigmoid/Tanh in deep hidden layers**: They saturate and cause vanishing gradients.

---

## 5. Optimization Algorithms

- **SGD with Momentum:**
  $$v_t=\mu v_{t-1}+g_t$$
  $$\theta_{t+1}=\theta_t-\eta v_t$$

- **Adam (Adaptive Moment Estimation):**
  $$
  \begin{aligned}
  m_t &= \beta_1 m_{t-1}+(1-\beta_1)g_t \\
  v_t &= \beta_2 v_{t-1}+(1-\beta_2)g_t^2 \\
  \hat m_t &= \frac{m_t}{1-\beta_1^t} \\
  \hat v_t &= \frac{v_t}{1-\beta_2^t} \\
  \theta_{t+1} &= \theta_t-\eta \frac{\hat m_t}{\sqrt{\hat v_t}+\varepsilon}
  \end{aligned}
  $$

---

## 6. Regularization Techniques

- **L2 Regularization (Weight Decay):** Add $\lambda\| W\|_2^2$ to the loss. This encourages smaller weights, leading to smoother, less complex functions that often generalize better.
- **Dropout (Inverted):**
    - **During training:** Randomly set a fraction $p$ of activations to zero and scale the remaining ones by $\frac{1}{1-p}$.
    - **During inference:** Use the full network (no dropout, no scaling).
- **Early Stopping:** Monitor validation loss and stop training when it begins to increase, preventing overfitting.
- **Data Augmentation:** Create new training examples by applying transformations (e.g., rotation, noise) to existing data.

---

## 7. Batch Normalization (BN)

**During Training (on a mini-batch):**
1.  Calculate mini-batch mean and variance:
    $\mu_B = \frac{1}{m}\sum z_i, \quad \sigma_B^2 = \frac{1}{m}\sum (z_i-\mu_B)^2$
2.  Normalize:
    $\hat z_i=\frac{z_i-\mu_B}{\sqrt{\sigma_B^2+\epsilon}}$
3.  Scale and shift:
    $\text{BN}(z_i)=\gamma \hat z_i+\beta$

**During Inference:** Use running averages of $\mu$ and $\sigma^2$ calculated during training.

> [!QUESTION] Why does Batch Norm help?
> It stabilizes the distribution of activations, which smooths the loss landscape. This allows for higher learning rates ($\eta$) and makes optimization of deep networks more robust.

---

## 8. Residual Connections (ResNets)

A residual block computes $h(x) = x + F(x)$, forcing the stacked layers ($F(x)$) to learn the **residual mapping** $h(x) - x$.

> [!SUMMARY] One-liner for the Grader
> "Residual blocks learn the difference $F(x)=h(x)-x$. The identity skip-connection creates a 'gradient highway' that mitigates vanishing gradients and makes it easy for the block to learn an identity mapping, easing the optimization of very deep networks."
>
> **Bottleneck blocks** use a `1x1 -> 3x3 -> 1x1` convolution pattern to reduce parameters and computation, allowing for even deeper networks.

---

## 9. The 5-Step Backprop Recipe

1.  **Name every node and its shape.**
    - Linear: $z=W x + b$
    - Nonlinearity: $a=\phi(z)$
    - Classifier: $\hat y=\text{softmax}(z)$ or $\sigma(z)$
2.  **Perform the forward pass.** Calculate all intermediate values ($z, a, \hat y$) and the final loss $\mathcal{L}$.
3.  **Start from the loss.**
    - Softmax + CE: $\delta^{(\text{out})}=\frac{\partial L}{\partial z}=\hat y-y$
    - Sigmoid + BCE: $\delta^{(\text{out})}=\hat y-y$
    - MSE (Regression): $\delta^{(\text{out})}=\hat y-y$ (then chain through the output activation's derivative if there is one).
4.  **Use the Linear Layer Jacobian Pattern.** If $z=W x + b$ and you know the upstream gradient $\delta=\frac{\partial L}{\partial z}$, then:
    $$\boxed{ \nabla_W L=\delta x^\top, \quad \nabla_b L=\delta, \quad \nabla_x L=W^\top\delta }$$
5.  **Chain through activations.** Multiply element-wise by the activation's derivative:
    $$\delta_{\text{prev}}=(W_{\text{next}}^\top\delta_{\text{next}})\odot \phi'(z_{\text{prev}})$$

