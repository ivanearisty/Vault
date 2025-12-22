# Formulas

This note covers the foundational concepts of Deep Neural Networks, including the core math, common architectures, and training procedures.

---

## 🧠 Key Formulas & Common Facts

Here are some fundamental equations and concepts that are useful to know.

### Activation Functions & Their Derivatives

* **Sigmoid**: Squeezes values between 0 and 1. Useful for binary classification output layers.
    * **Formula**: $\sigma(z)=\frac{1}{1+e^{-z}}$
    * **Derivative**: $\sigma'(z) = \sigma(z)(1 - \sigma(z))$
* **ReLU**: The most common activation function.
    * **Formula**: $\phi(z) = \max(0, z)$
    * **Derivative**: $\phi'(z) = \begin{cases} 1 & \text{if } z > 0 \\ 0 & \text{if } z \leq 0 \end{cases}$
* **Tanh**: Squeezes values between -1 and 1 (zero-centered).
    * **Formula**: $\tanh(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}$
    * **Derivative**: $\tanh'(z) = 1 - \tanh^2(z)$

### The Gradient Vanishing/Exploding Problem

In deep networks, gradients are multiplied across many layers during [[backpropagation]].
* **Vanishing Gradients**: If gradients are small (<1), their product can become exponentially small, leading to negligible weight updates in early layers and slow/stalled training. Sigmoid and Tanh are particularly susceptible to this when their inputs are saturated.
* **Exploding Gradients**: If gradients are large (>1), their product can become exponentially large, leading to unstable training and oscillating weights.

Techniques like **[[ReLU]]**, **[[Batch Normalization]]**, and **[[Residual Networks (ResNets)]]** are primary solutions to this problem.

---

## 🔢 Core Forward & Backward Equations (1 Hidden Layer)

This template describes a simple Multi-Layer Perceptron (MLP) with one hidden layer.

### Forward Propagation

1.  **Linear Step (Hidden Layer)**: $z^{(1)} = W^{(1)}x + b^{(1)}$
2.  **Activation (Hidden Layer)**: $h = \phi(z^{(1)})$ (where $\phi$ is an activation function like [[ReLU]])
3.  **Linear Step (Output Layer)**: $z^{(2)} = W^{(2)}h + b^{(2)}$
4.  **Activation (Output Layer)**: For multi-class classification, we use [[Softmax]].
    $$\hat{y}_k=\dfrac{e^{z^{(2)}_k}}{\sum_j e^{z^{(2)}_j}}$$

### Loss (Cross-Entropy)

For a single example, the [[Cross-Entropy Loss]] is:
$$\mathcal{L} = -\sum_{k} y_k \log \hat{y}_k$$
where $y$ is the one-hot encoded true label.

### Backpropagation

These are the essential gradients needed to update the network parameters using [[Gradient Descent]].

* **Output Layer Gradient**: The gradient of the loss with respect to the output logits $z^{(2)}$ has a very simple form when using Softmax and Cross-Entropy Loss.
    $$\frac{\partial \mathcal{L}}{\partial z^{(2)}}=\hat{y}-y$$
* **Hidden Layer Gradient**: This is found by chaining the gradient from the output layer back through the weights $W^{(2)}$ and the activation function $\phi$.
    $$\frac{\partial \mathcal{L}}{\partial z^{(1)}}=\big((W^{(2)})^\top(\hat{y}-y)\big)\odot \phi'(z^{(1)})$$
    where $\odot$ is the element-wise product.
* **Parameter Gradients**: Gradients for the weights and biases.
    $$\frac{\partial \mathcal{L}}{\partial W^{(2)}}=(\hat{y}-y)h^\top,\quad \frac{\partial \mathcal{L}}{\partial b^{(2)}}=\hat{y}-y$$
    $$\frac{\partial \mathcal{L}}{\partial W^{(1)}}=\Big(\frac{\partial \mathcal{L}}{\partial z^{(1)}}\Big) x^\top,\quad \frac{\partial \mathcal{L}}{\partial b^{(1)}}=\frac{\partial \mathcal{L}}{\partial z^{(1)}}$$

---

## 📈 Activation Functions (Nonlinearities)

An [[Activation Function]] introduces non-linearity into the model.

* **ReLU**: $\phi(z)=\max(0,z)$. The default choice. Avoids vanishing gradients and promotes sparse activations.
* **Leaky/Parametric ReLU**: A small positive slope for negative inputs ($0.01z$ or $\alpha z$). Helps prevent "dying ReLU" problem.
* **Tanh / Sigmoid**: Bounded functions. Can saturate and cause vanishing gradients, slowing down training. Best for shallow networks or specific output requirements.
* **GELU/Swish**: Smooth approximations of ReLU. Often perform better in very deep models like Transformers.

> **Why use a nonlinearity?**
> To allow the network to learn complex, non-linear relationships. Without them, a stack of linear layers would collapse into a single, equivalent linear layer, limiting the model's expressive power. This capability is explained by the [[Universal Approximation Theorem]].

---

## 🛠️ Initialization & Normalization

Proper initialization and normalization are crucial for stable training.

### Weight Initialization

* **He Initialization (for ReLU)**: Draws weights from a normal distribution with variance scaled by the number of input units ("fan-in"). Helps keep activation variances near 1.
    $$W_{ij}\sim \mathcal{N}(0, \tfrac{2}{\text{fan\_in}})$$
* **Xavier/Glorot Initialization (for Tanh)**: A compromise between fan-in and fan-out.
    $$W_{ij}\sim \mathcal{N}(0, \tfrac{2}{\text{fan\_in}+\text{fan\_out}})$$

### Batch Normalization

Normalizes the activations of a layer for each mini-batch.
$$\hat{h}=\dfrac{h-\mu}{\sqrt{\sigma^2+\epsilon}},\quad y=\gamma\hat{h}+\beta$$
* **Why it helps**: It stabilizes the distribution of activations, which reduces **[[Internal Covariate Shift]]**. This allows for higher learning rates, acts as a form of regularization, and makes the network less sensitive to initialization.

---

## ⚖️ Regularization Techniques

[[Regularization]] techniques prevent overfitting by discouraging model complexity.

* **Weight Decay (L2)**: Adds a penalty term $\lambda \|W\|_2^2$ to the loss function. This encourages the network to learn smaller, smoother weights.
* **Dropout (p)**: During training, randomly sets a fraction `p` of activations to zero at each update step. This prevents neurons from co-adapting and forces the network to learn more robust features. It's like training an ensemble of smaller networks.
* **Early Stopping**: Monitor validation loss and stop training when it begins to increase.
* **Data Augmentation**: Create more training data by applying realistic transformations (e.g., rotating, cropping images).
* **Label Smoothing**: Replaces hard labels (e.g., `[0, 1, 0]`) with soft labels (e.g., `[0.05, 0.9, 0.05]`) to prevent the model from becoming overconfident.

---

## 🔗 Residual Learning ([[Residual Networks (ResNets)]])

A key architecture for building very deep networks.

* **Core Idea**: Instead of learning a direct mapping $H(x)$, the block learns a **residual function** $F(x) = H(x) - x$. The output is then $y = x + F(x)$.
* **Rationale**: It's easier for a layer to learn to output zero (i.e., learn the identity) than to learn the identity mapping itself. This "identity skip-connection" allows gradients to flow more easily through the network, mitigating the vanishing gradient problem and enabling the training of networks with hundreds or thousands of layers.
* **Bottleneck Block**: A common ResNet block that uses `1x1` convolutions to reduce channel dimensions, applies a standard `3x3` convolution, and then uses another `1x1` to restore dimensions. This reduces computation and parameters, allowing for even deeper models.

---

## 📏 Losses for Representation Learning

These losses are used to learn meaningful embeddings (vector representations) of data without explicit labels, often for tasks like face recognition or image retrieval.

### [[Contrastive Loss]] (for pairs)

$$L = \mathbb{1}[y_i=y_j]\|f(x_i)-f(x_j)\|_2^2 + \mathbb{1}[y_i\neq y_j]\max\big(0, m - \|f(x_i)-f(x_j)\|\big)^2$$
* **Explanation**: It pulls embeddings of similar items together (`y_i=y_j`) and pushes embeddings of dissimilar items apart by at least a margin `m`.

### [[Triplet Loss]] (for anchor-positive-negative triplets)

$$L = \max\big(0, \|f(a)-f(p)\|_2^2 - \|f(a)-f(n)\|_2^2 + \alpha\big)$$
* **Explanation**: It ensures that the distance between an anchor `a` and a positive example `p` is smaller than the distance between the anchor `a` and a negative example `n` by at least a margin $\alpha$.

---

## 🎯 Empirical vs. True Risk

This is a core concept in [[Statistical Learning Theory]].

* **Empirical Risk**: The average loss over the **training set**.
    $$L_{emp}(g)=\frac{1}{m}\sum_{i=1}^m \ell(g(x_i),y_i)$$
* **True Risk**: The expected loss over the **true data distribution**.
    $$L_{true}(g)=\mathbb{E}_{(x,y)\sim \mathcal{D}}[\ell(g(x),y)]$$
* **Why it Matters**: The goal of training is to find a model with low true risk, but we can only measure empirical risk. [[Generalization]] is the ability of a model to achieve low true risk, and overfitting is when a model has low empirical risk but high true risk.

---

## 🔄 The Training Loop

The standard algorithm for training a neural network.

1.  **Sample**: Draw a minibatch of data $(x, y)$ from the training set.
2.  **Forward Pass**: Compute the model's prediction $\hat{y}$ and the loss $\mathcal{L}(\hat{y}, y)$.
3.  **Backward Pass**: Compute the gradients of the loss with respect to all model parameters ($\frac{\partial \mathcal{L}}{\partial W}, \frac{\partial \mathcal{L}}{\partial b}$) using [[backpropagation]].
4.  **Update**: Adjust the model's parameters using an optimizer like [[SGD]] or [[Adam]]. For example: $W \leftarrow W - \eta \frac{\partial \mathcal{L}}{\partial W}$.
5.  **Repeat**: Continue for a set number of epochs, possibly adjusting the learning rate (`LR schedule`) or using `early stopping`.