# Lecture 2: Neural Nets - Deep Learning

## Neural Networks: Architecture, Training, and Prediction

This lecture explores what neural networks are, how to train them, and prediction methodology once trained.

### Introduction to Neural Network Concepts

Neural networks build upon shallow models like linear regression and perceptrons, viewed as computational primitives called neurons. These form building blocks arranged in directed acyclic graphs (DAGs) where nodes represent inputs/outputs and edges represent weighted connections.

**Key Loss Functions:**

The logistic loss for binary classification is expressed as:
$$L(w) = -(y\log(p)) + (1-y)\log(1-p)$$

For multi-class scenarios, cross-entropy loss applies:
$$L(w) = -\sum_{i=1}^{k} y_i \log(p_i)$$

The softmax function converts model logits to probabilities:
$$p_i = \frac{e^{z_i}}{\sum_{j=1}^{k} e^{z_j}}$$

### Neural Network Architecture

A neuron's functional form is:
$$z = \sigma\left(\sum_{j} w_j x_j + b\right)$$

Where activation functions introduce nonlinearity. Common options include sigmoid, hyperbolic tangent, ReLU (rectified linear unit), hard threshold, and sign functions. ReLU is preferred currently due to avoiding vanishing gradient problems.

**Layer Types Include:**
- Dense layers (unconstrained weights)
- Convolutional layers (filter-based)
- Pooling layers (downsampling)
- Batch normalization
- Recurrent layers
- Residual layers
- Attention layers

A 3-layer network has the form:
$$\begin{aligned} z_1 &= \sigma^{1}(W^{1} x + b^{1}) \\ z_2 &= \sigma^{2}(W^{2} z_1 + b^{2}) \\ y &= \sigma^{3}(W^{3} z_2 + b^{3}) \end{aligned}$$

### Universal Approximation Theorem

Neural networks function as "universal approximators." A single-hidden-layer feedforward network with finitely many neurons can approximate any continuous function (Cybenko, 1989). However, this requires potentially exponential neuron counts. Modern deep learning trades width for depth—maintaining manageable width while adding layers.

**Three Key Algorithmic Questions:**
1. How to choose layer widths, types, and quantities?
2. How to learn network weights?
3. How does training generalization extend to new examples?

### Gradient Descent

Gradient descent minimizes loss functions through iterative descent. Starting at estimate $w_k$, the algorithm updates:
$$w_{k+1} = w_k - \alpha_k \nabla L(w_k)$$

Where $\alpha_k$ is the learning rate (step size). For smooth, convex functions like mean-squared error, this approach reliably converges.

**Linear Regression Example:**
$$w_{k+1} = w_k + \alpha_k \sum_{i=1}^n (y_k - \langle w_k,x_i \rangle) x_i$$

Per-iteration cost is $O(nd)$, but large datasets present challenges since computing full gradients repeatedly becomes expensive.

### Stochastic Gradient Descent (SGD)

Rather than computing gradients over all data, SGD approximates using random subsets $S$:
$$w_{k+1} = w_k + \alpha_k' \sum_{i \in S} (y_i - \langle w_k, x_i \rangle) x_i$$

Extreme case: using single random data points. Step-size should decay, typically as:
$$\alpha_k = C/k$$

**SGD Algorithm:**

1. Initialize $w_0 = 0$
2. Repeat: Choose random $(x_i, y_i)$, update:
$$w_{k+1} \leftarrow w_k + \alpha_k (y_i - \langle w_k, x_i \rangle) x_i$$

Per-iteration cost is only $O(d)$, though total iterations typically exceed full gradient descent. Mini-batch variants balance speed and accuracy.

### (S)GD and Neural Networks

For neural networks, stack all weights and biases as variable $W$, then optimize:
$$L(W) = \sum_{i=1}^n l(y_i, f(x_i)) + \lambda R(W)$$

Using minibatch SGD:
$$W^{t+1} = W^{t} - \alpha^{t} \left( \sum_{i \in S} \nabla l(y_i, f_W(x_i)) + \lambda \nabla R(W) \right)$$

**Ridge Regression Example (Single Neuron):**

$$\begin{aligned} z &= wx + b,~f(z) = \sigma(z) \\ L(w,b) &= 0.5 (y - \sigma(wx + b))^2 + \lambda w^2 \end{aligned}$$

Gradients require chain rule application:
$$\begin{aligned} \frac{\partial L}{\partial w} &= (\sigma(wx + b) - y) \sigma'(wx + b) x + 2 \lambda w \\ \frac{\partial L}{\partial b} &= (\sigma(wx + b) - y) \sigma'(wx + b) \end{aligned}$$

This becomes tedious and redundant for complex networks.

### The Backpropagation Algorithm

Backpropagation solves redundancy issues through structured computation graphs. Decompose operations sequentially:

$$\begin{aligned} z &= wx + b \\ u &= \sigma(z) \\ l &= 0.5 (y-u)^2 \\ r &= w^2 \\ L &= l + \lambda r \end{aligned}$$

**Forward Pass:** Compute each node value from its parents.

**Backward Pass:** For nodes $v_1, \ldots, v_N$, compute gradients in reverse:
$$\frac{\partial L}{\partial v_i} = \sum_{j \in \text{Children}(v_i)} \frac{\partial L}{\partial v_j} \frac{\partial v_j}{\partial v_i}$$

**Example Backward Computation:**

$$\begin{aligned} \partial_L L &= 1 \\ \partial_r L &= \lambda \\ \partial_l L &= 1 \\ \partial_u L &= u-y \\ \partial_z L &= \partial_u L \cdot \sigma'(z) \\ \partial_w L &= \partial_u L \cdot x + 2\lambda w \\ \partial_b L &= \partial_z L \end{aligned}$$

**Advantages:**
- Eliminates redundant computations
- Structured single-pass traversal
- Modular—architecture changes don't alter the algorithm
- Automatic differentiation libraries (PyTorch) implement this efficiently
