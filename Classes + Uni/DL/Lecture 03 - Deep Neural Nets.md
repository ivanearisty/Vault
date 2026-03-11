# Lecture 3: Deep Neural Nets - Deep Learning

## Chapter 3: Learning Deep Networks: Tips and Tricks

*In which we encounter deep neural networks, their challenges, and how to fix them.*

### Overview

This lecture covers the practical aspects of training deep neural networks. The training process involves:

- Formulating network architecture with trainable parameters (weights and biases)
- Defining a suitable loss function
- Updating weights via backpropagation using forward and backward passes

The feedforward structure enables efficient computation in linear time relative to the number of parameters.

---

## Automatic Differentiation

Automatic differentiation (Autodiff) computes gradients for any differentiable function without using finite differences or symbolic calculus. Given a function $y = F(x)$ with $d$ features, the gradient is:

$$\nabla F(x) = \left[ \frac{\partial F}{\partial x_1}, \ldots, \frac{\partial F}{\partial x_d} \right]$$

**Key insight:** "Autodiff is not merely symbolic calculus...The output of Autodiff is _not_ a formula; rather, it can be viewed as a _function_ for computing derivatives 'under the hood'."

### Computation Graph

Autodiff systems perform two main tasks:

1. **Forward pass:** Build the computation graph, breaking down network evaluation into unitary scalar calculations layer by layer
2. **Backward pass:** Evaluate backprop equations using Vector Jacobian Products (VJP)

### Vector Jacobian Products

For a loss function $L$ and node $z$ with children, the backprop equation is:

$$\frac{\partial L}{\partial z} = \sum_{c_i \in \text{Children}(z)} \frac{\partial L}{\partial c_i} \cdot \frac{\partial c_i}{\partial z}$$

In matrix notation:
$$\bar{z} = J^T \bar{c}$$

where $J$ is the Jacobian matrix $\frac{\partial c_i}{\partial z_j}$.

**Practical note:** Autodiff never explicitly stores $J$ in memory (prohibitively expensive for sparse networks like CNNs). Instead, it evaluates the VJP recursively.

### Node-Based Message Passing

Each node knows its parents but not necessarily its children. During training:

- **Forward pass:** Nodes aggregate scalar messages from parent nodes
- **Backward pass:** Nodes receive partial gradients, aggregate them, multiply by the Jacobian, and send to all parent nodes

This modular approach enables efficient computation for any differentiable continuous-valued function.

---

## Parameter Management and GPUs

Deep learning computations heavily rely on matrix-vector products. These operations are "embarrassingly parallel" – each row can be calculated simultaneously.

### GPU Advantages

- Large number of processing cores with high memory bandwidth
- Exceptionally efficient at matrix-vector multiplication
- Minimize CPU-GPU data transfers (data transfer becomes the bottleneck)

**Standard practice:** Model parameters permanently stored on GPU; minibatches transferred one at a time. Overlapping computation with data transfers hides latency and maximizes core utilization.

---

## Variants of SGD: Momentum, RMSProp, and Adam

### Momentum

Standard gradient descent:
$$w_{t+1} = w_t - \eta \nabla F(w_t)$$

Problems with vanilla gradient descent include local minima and oscillatory behavior from pathological curvature.

**Momentum solution:** Add memory to gradient dynamics:

$$w_{t+1} = w_t - \eta v_t$$
$$v_t = \beta g_t + (1 - \beta) v_{t-1}$$

The momentum term $v_t$ is an exponentially weighted moving average (EWMA) of gradients:

$$v_t = \sum_{i=0}^{t-1} (1-\beta)^i \beta g_{t-i}$$

**Intuition:** Momentum reinforces consistent directions and reduces updates to rapidly changing gradients, resulting in faster convergence and reduced oscillation.

**Typical values:** $\beta = 0.9$ or $0.99$

- $\beta = 0.9$ considers the last ~10 iterations (good default)
- $\beta = 0.999$ considers the last ~1000 iterations (risks oscillations)

**Key benefit:** Provides quadratic acceleration for linear models.

### Preconditioning

Rather than fixed learning rate decay, use adaptive schemes that accumulate gradient variance:

$$g_t = \partial F(w_t)$$
$$s_t = s_{t-1} + g_t^2$$
$$w_{t+1} = w_t - \frac{\eta}{\sqrt{s_t} + \varepsilon} g_t$$

The $\varepsilon$ term prevents division by zero.

**Methods using preconditioning:** RMSProp, Adadelta, Adagrad (differences are minor)

### Adam

Combines momentum and preconditioning, adding memory to variance estimates:

$$g_t = \partial F(w_t)$$
$$v_t = \frac{\mu v_{t-1} + (1-\mu) g_t}{1 - \mu^t}$$
$$s_t = \frac{\beta s_{t-1} + (1-\beta) g_t^2}{1 - \beta^t}$$
$$w_{t+1} = w_t - \frac{\eta}{\sqrt{s_t} + \varepsilon} v_t$$

**Typical parameters:** $\mu = 0.9$, $\beta = 0.999$ (more momentum for variance)

Most deep learning frameworks implement these methods.

---

## Initialization

Careful weight initialization addresses two critical issues:

### 1. Vanishing/Exploding Gradients

For a linear network:
$$\hat{y} = W_L W_{L-1} \ldots W_1 x$$

The gradient of squared error loss with respect to $W_r$:
$$\partial_{W_r} L(W) = -(f_W(x) - y) W_L^T W_{L-1}^T \ldots W_2 W_1$$

This product of matrices can become numerically unstable (too large or too small) in very deep networks due to accumulated scaling from Jacobians.

### 2. Symmetry Breaking

Dense feedforward networks have permutation symmetry – neurons are initially indistinguishable. If all weights in a layer equal $c$, they remain identical after gradient updates, preventing shared learning.

Such configurations are saddle points, notoriously difficult to escape.

**Solution:** Initialize weights randomly.

### Xavier (Glorot) Initialization

Preserve "energy" in both forward and backward passes. For aggregated inputs:
$$Z_i = \sum_{j=1}^{n_{in}} W_{ij} X_j$$

This gives:
$$\text{var}(Z_i) = n_{in} \sigma^2 \text{var}(X_i)$$

Requiring $n_{in} \sigma^2 \approx 1$.

Similarly, backward pass analysis shows $n_{out} \sigma^2 \approx 1$.

**Standard scheme:** Sample weights from Gaussian distribution with mean zero and variance:
$$\sigma^2 = \frac{2}{n_{in} + n_{out}}$$

**Alternatives:** He initialization uses similar principles with different constants.

---

## Generalization

Modern neural networks contain many parameters. For example, state-of-the-art ImageNet object recognition models (as of 2019) have 928 million learnable parameters – far exceeding the 14 million training samples.

### Bias-Variance Tradeoff

As parameters increase:
- Bias decreases (more tunable knobs to fit data)
- Variance increases (greater overfitting)

The optimal point balances these competing forces.

**Note:** Deep neural networks exhibit "double descent," a phenomenon where performance improves again with very high parameter counts.

### Regularization Strategies

#### Bottleneck Architectures

Add narrower linear layers between wider layers to constrain expressiveness.

#### Early Stopping

Monitor training and validation error curves during learning. Stop when the generalization gap (difference between training and validation error) is minimized.

**Caveat:** Stochastic methods like SGD produce fluctuating curves, potentially causing premature stopping.

#### Weight Decay

Add L2 regularization to the standard training loss.

#### Dataset Augmentation

Artificially expand dataset size through transformations:
- Images: shift, rotate, flip, crop, shear, distort
- Domain-specific augmentations available in PyTorch and similar libraries

#### Dropout

Simulate a smaller model by stochastically dropping neurons. For each neuron $i$, introduce random binary mask $m_i$ (1 with probability $p$, 0 with probability $1-p$).

**Forward pass:**
$$h_i = m_i \phi(z_i)$$

**Backward pass:**
$$\partial_{z_i} \mathcal{L} = \partial_{h_i} \mathcal{L} \cdot m_i \cdot \partial_{z_i} \phi(z_i)$$

**Test time:** Scale all weights by $p$ to match expected values.

#### Transfer Learning

Leverage knowledge from different tasks to address limited training data. Train a network on a source task (e.g., general object recognition), then finetune on target task (e.g., medical image classification).

**Rationale:** Common learned features may transfer across datasets, reducing need to relearn all representations.
