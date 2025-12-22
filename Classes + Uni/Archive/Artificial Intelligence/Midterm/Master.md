## 1. Log-Likelihood, NLL, and Cross-Entropy

### Intuition
The goal of **Maximum Likelihood Estimation (MLE)** is to find model parameters that make observed data most probable.
**Log-likelihood** avoids underflow and simplifies derivatives.
**Negative Log-Likelihood (NLL)** is what we minimize — this is equivalent to minimizing **Cross-Entropy (CE)**.

### Core Formulas
$$
\mathcal{L}(w)=\prod_i p(x^{(i)};w)
$$

$$
\ell(w)=\sum_i \log p(x^{(i)};w)
$$

$$
w^*=\arg\max_w \ell(w) = \arg\min_w [-\ell(w)] = \arg\min_w \text{NLL}
$$

### Connection to Information Theory
$$
\text{CE}(\hat p, p_\theta)=\text{KL}(\hat p | p_\theta)+H(\hat p)
$$
Minimizing CE is equivalent to minimizing KL divergence, which is equivalent to maximizing likelihood.

### Binary Cross-Entropy (Bernoulli Likelihood)
$$
L = -[y\log\hat y + (1-y)\log(1-\hat y)]
$$

### Multiclass Cross-Entropy (Softmax)
$$
L = -\sum_k y_k \log \hat y_k
$$

### Remember
- Cross-Entropy = NLL = Log-Loss.
- MSE can also be seen as NLL under a Gaussian assumption.
- Exam pattern: “Given a distribution, derive the MLE parameters” (e.g., mean/variance).

---

## 2. Binary Classification

### Core Idea
Predict a discrete label ($y \in \{0,1\}$) given an input ($x$). The model predicts the posterior probability: $\hat y = p(y=1|x) = \sigma(w^\top x)$.

### Logistic Regression Intuition
- It's a linear model, but it has a nonlinear output via the sigmoid function.
- It learns a decision boundary in the feature space.
- It uses a Bernoulli likelihood, which leads to a log-likelihood, which results in the binary cross-entropy loss function.

### Derivation
1.  **Likelihood:**
    $$
    p(y_i|x_i;w)=\hat y_i^{y_i}(1-\hat y_i)^{1-y_i}
    $$
2.  **Log-likelihood:**
    $$
    \log \mathcal{L}(w)=\sum_i[y_i\log\hat y_i+(1-y_i)\log(1-\hat y_i)]
    $$
3.  **Negative log-likelihood (Loss):**
    $$
    L(w)=-\frac{1}{m}\sum_i[y_i\log\hat y_i+(1-y_i)\log(1-\hat y_i)]
    $$

### Gradient
$$
\nabla_w L = \frac{1}{m}\sum_i (\hat y_i - y_i)x_i
$$

### Decision Boundary
$$
\text{class}=1 \text{ if } \hat y \ge 0.5
$$

### Metrics
| Metric | Formula | Notes |
| :--- | :--- | :--- |
| **Accuracy** | $(TP+TN)/(TP+TN+FP+FN)$ | Overall correctness |
| **Precision** | $TP/(TP+FP)$ | Avoids false positives |
| **Recall** | $TP/(TP+FN)$ | Detects all positives |
| **F1** | $2PR/(P+R)$ | Harmonic mean |
| **ROC-AUC** | TPR vs FPR | Separability measure |

### Key Insights
- BCE sharply penalizes confident but wrong predictions.
- Understand the connection between entropy and cross-entropy.
- L2 regularization makes the weight vector $w$ sparser.
- Logistic regression can be thought of as a linear model with a nonlinearity applied to the posterior.

---

## 3. Deep Neural Networks (DNN)

### Core Concept
A deep network stacks linear and nonlinear transformations to approximate complex functions.

### Forward Pass
$$
z^{[l]} = W^{[l]}a^{[l-1]} + b^{[l]}, \quad a^{[l]} = \phi(z^{[l]})
$$

$$
\hat y = a^{[L]}
$$

### Parameter Count
$$
\text{Params per layer} = n_l n_{l-1} + n_l
$$

### Backpropagation
- **Output layer:** $\delta^{[L]} = \hat y - y$
- **Hidden layers:**
$$\delta^{[l]} = (W^{[l+1]})^\top \delta^{[l+1]} \odot \phi'(z^{[l]})$$
- **Gradients:**
$$\nabla_W L = \delta^{[l]}(a^{[l-1]})^\top$$
$$\nabla_b L = \sum \delta^{[l]}$$

### Activation Functions
| Activation | Formula | Derivative | Use |
| :--- | :--- | :--- | :--- |
| **ReLU** | $\max(0, z)$ | $1 \text{ if } z>0$ | Default choice |
| **Sigmoid** | $1/(1+e^{-z})$ | $\sigma(1-\sigma)$ | Binary outputs |
| **Tanh** | $\tanh(z)$ | $1 - \tanh^2(z)$ | Centered activations |

### Initialization
| Method | For | Variance |
| :--- | :--- | :--- |
| **He** | ReLU | $2/n_{l-1}$ |
| **Glorot/Xavier**| Sigmoid/Tanh| $2/(n_{l-1} + n_l)$ |
*Proper initialization prevents vanishing/exploding gradients.*

### Optimizers
- **SGD**: $w = w - \eta \nabla L$
- **Momentum**: $v_t = \mu v_{t-1} + g_t$
- **Adam**: Adaptive moment estimation; stable for most networks.

### Regularization
| Technique | Purpose | Notes |
| :--- | :--- | :--- |
| **L2** | Prevent large weights | Adds $\lambda\|W\|^2$ to loss |
| **Dropout**| Prevent co-adaptation| Scale activations by $(1-p)$ during inference |
| **Early Stopping**| Stop before overfitting | Use validation loss to decide when to stop|
| **Batch Norm**| Normalize activations| Stabilizes training |

### Residual Connections
$$
h(x) = x + F(x)
$$
This allows the network to learn a residual mapping and helps solve the vanishing gradients problem. It's a key component in ResNets and other modern CNNs.

---

## 4. Convolutional Neural Networks (CNNs)

### What is a Convolution?
Each filter/kernel slides over the input to compute local dot products, which results in feature maps.

### Output Size
$$
H_{out} = \lfloor\frac{H + 2P - K}{S}\rfloor + 1
$$

$$
W_{out} = \lfloor\frac{W + 2P - K}{S}\rfloor + 1
$$

### Parameters
$$
\text{Params} = (K_H \times K_W \times C_{in} \times C_{out}) + C_{out}
$$

### Receptive Field Growth
$$
r_{new} = r_{old} + (k-1)j_{old}
$$

$$
j_{new} = j_{old} \times s
$$

### Residual & Bottleneck Blocks
- **Residual block**: $y = x + F(x)$.
- **Bottleneck (ResNet-50)**: A sequence of $1 \times 1 \to 3 \times 3 \to 1 \times 1$ convolutions. This reduces the number of parameters while preserving representational power.

### Common “Why” Questions
| Concept | Why It Matters |
| :--- | :--- |
| **Residual Connections**| Solves vanishing gradients, allowing for deeper models. |
| **Global Avg Pooling**| Fewer parameters than Flatten, less prone to overfitting. |
| **Batch Norm** | Stabilizes and accelerates training. |
| **ReLU** | Provides a simple, effective nonlinear representation. |
| **Parameter Sharing** | Enables translation equivariance. |
| **Bottleneck Block** | Allows for efficient feature transformation with fewer parameters. |

### Common Pitfalls
- **Overfitting**: Use data augmentation, dropout, or weight decay.
- **Tiny batch size**: Can cause batch norm instability.
- **Vanishing gradients**: Use residuals and ReLU.

---

## 5. Object Detection & Scene Understanding

### R-CNN → Fast R-CNN → Faster R-CNN
| Version | Key Innovation |
| :--- | :--- |
| **R-CNN** | Selective search for region proposals. |
| **Fast R-CNN** | Shared feature map + RoI pooling. |
| **Faster R-CNN**| Region Proposal Network (RPN) replaces selective search. |
*The RPN learns objectness and bounding box regression directly.*

### Loss Functions
$$
L = L_{cls} + \lambda [y \ge 1] L_{loc}
$$

$$
L_{cls} = -\log(\hat y_y), \quad
L_{loc} = \sum_i \text{Smooth}_{L1}(t^i - v^i)
$$

### Evaluation Metrics
- **IoU**: Intersection over Union, measures overlap ratio.
- **Precision-Recall Curves**: Visualizes the trade-off between precision and recall.
- **AP/mAP**: Average Precision / mean Average Precision; the area under the PR curve per class or averaged across all classes.
*A high IoU threshold results in a stricter match, which typically leads to lower recall but higher precision.*

### Key “Why” Questions
- **Why Smooth L1 loss?** It's more robust to outliers than L2 loss.
- **Why a background class?** It helps the model reject non-objects.
- **Why the RPN?** It allows for end-to-end training of the detector.
- **Why NMS per class?** To avoid suppressing detections of different object categories that happen to overlap.
- **Why mAP over accuracy?** It accounts for both classification and localization quality.

---

## 6. State Estimation (Kalman Filter)

### Core Idea
Estimate the true state of a system by fusing predictions from a model with noisy measurements.

### Two-Step Cycle
1.  **Predict**: Use the system model and any control inputs to predict the next state. This step increases uncertainty.
2.  **Update**: Fuse the new measurement with the prediction. This step reduces uncertainty.

### Key Equations
- **Prediction:**
$$\bar{x}_t = A x_{t-1} + B u_t$$
$$\bar{P}_t = A P_{t-1} A^\top + Q$$

- **Update:**
$$K_t = \bar{P}_t H^\top (H \bar{P}_t H^\top + R)^{-1}$$
$$x_t = \bar{x}_t + K_t(z_t - H\bar{x}_t)$$
$$P_t = (I - K_t H)\bar{P}_t$$

### Interpretation
- **Kalman Gain (K)**: Represents the weight given to the measurement versus the model's prediction.
- **Residual (Innovation)**: The difference between the actual measurement and the predicted measurement ($z_t - H\bar{x}_t$).
- **Belief**: The probability distribution of the system's state, which is assumed to be Gaussian.

### Bayes Foundation
The process follows the Bayes' rule paradigm: Posterior = Prior × Likelihood (normalized).
$$
bel(x_t)=\eta \cdot p(z_t|x_t) \cdot \overline{bel}(x_t)
$$
Because we assume Gaussian priors and Gaussian noise, the resulting posteriors are also Gaussian, making the updates computationally tractable.

---

## Summary Table: Quick Reference
| Topic | Core Formula | Key Idea |
| :--- | :--- | :--- |
| **Log-Likelihood**| $\ell = \sum_i \log p(x_i;w)$ | Maximize the fit of the model to the data. |
| **BCE** | $-[y\log\hat y + (1-y)\log(1-\hat y)]$ | Penalizes confident errors. |
| **DNN Gradient** | $\delta^{[l]} = (W^{[l+1]})^\top \delta^{[l+1]} \odot \phi'(z^{[l]})$ | The core of backpropagation. |
| **Conv Output** | $(H+2P-K)/S + 1$ | Compute output feature map size. |
| **Residual Block**| $y = x + F(x)$ | A "gradient highway" to enable deeper networks. |
| **RPN** | Anchors + objectness score | A faster, learnable way to generate proposals. |
| **Kalman Gain** | $K = P H^\top(HPH^\top + R)^{-1}$ | Balance trust between prediction and measurement. |