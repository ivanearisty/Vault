# Lecture 1: Introduction - Deep Learning

**12 minute read**

## Overview

### Why Machine Learning, and Why Now?

Machine learning and AI have become ubiquitous across scientific, engineering, and social disciplines. The dramatic progress in deep learning over the past five years stems from two key factors: computing devices and sensors now pervade daily life, and acquiring and processing data has become easier and more cost-effective than ever.

Popular metaphors compare AI to transformative resources ("the new electricity" or "the new oil"), reflecting its significant impact across multiple fields. However, many fundamental questions remain unanswered, and practitioners often deploy deep learning tools as unexplained "black boxes."

To establish deep learning on solid theoretical ground, we must build it from first principles, asking foundational questions about correctness, soundness, and efficiency—questions computer scientists would naturally pose.

### What is Machine Learning?

A useful conceptual framework: **Data → (Machine Learning System) → Actionable Information**

### Ingredients of a Machine Learning System

Three core computational components are necessary:

1. **Representation**: Features or a model encoding the data structure
2. **Measure of Goodness**: Loss function or cost function quantifying performance
3. **Optimization Method**: Training algorithm that improves the measure of goodness

After developing such a system, we can make predictions on new, unseen data during inference.

#### Application Example: Image Classification

1. A model mapping images to class predictions (e.g., cat vs. dog)
2. A loss function measuring correctness of predictions
3. An optimization approach using gradient descent on model parameters

---

## Vector Spaces

Data often consists of numerical attributes associated with objects of interest. Consider meteorological sensor readings measuring wind speed ($w$) and temperature ($t$), represented as tuples $(w,t)$.

Each tuple naturally maps to a point in a two-dimensional vector space. More generally, data with $d$ features becomes an element in $\mathbb{R}^d$.

### Vector Space Data Examples

1. **Sensor readings**: Weather data and similar continuous measurements
2. **Image data**: A $1024 \times 768$ RGB image as a vector in $\mathbb{R}^{2359296}$ (1024 × 768 × 3 dimensions)
3. **Time-series data**: Stock prices over 1000 days represented in 1000-dimensional space

---

## Properties of Vector Spaces

### Fundamental Operations

**Linearity**: Vectors $x = (x_1, \ldots, x_d)$ and $y = (y_1, \ldots, y_d)$ satisfy:
$$x + y = (x_1 + y_1, \ldots, x_d + y_d)$$

**Scaling**: A vector $x$ scaled by $\alpha \in \mathbb{R}$:
$$\alpha x = (\alpha x_1, \ldots, \alpha x_d)$$

### Norms

The **Euclidean (ℓ₂) norm** represents vector length:
$$\|x\|_2 = \sqrt{\sum_{i=1}^d x_i^2}$$

The **Manhattan (ℓ₁) norm** sums absolute values:
$$\|x\|_1 = \sum_{i=1}^d |x_i|$$

### Distances

The Euclidean distance between vectors $x$ and $y$:
$$\|x-y\|_2 = \sqrt{\sum_{i=1}^d (x_i - y_i)^2}$$

This generalizes to other norms as well.

### Similarities

The Euclidean **inner product** between $x$ and $y$:
$$\langle x, y \rangle = \sum_{i=1}^d x_i y_i$$

The **cosine similarity** normalizes this product:
$$\text{sim}(x,y) = \frac{\langle x, y \rangle}{\|x\|_2 \|y\|_2}$$

The inverse cosine yields the generalized angle between vectors.

---

## Warmup: Linear Models

**Regression problem**: Given observed data-label pairs $\{(x_1,y_1), (x_2,y_2), \ldots, (x_n,y_n)\}$, discover a function $f$ such that $y_i \approx f(x_i)$.

### Practical Examples

- Predicting stock prices from quarterly revenue
- Forecasting auto fuel efficiency from vehicle weight
- Estimating ride-sharing demand from population density

### Linear Regression Formulation

Assuming a linear functional relationship:
$$y_i = \langle w, x_i \rangle, \quad i=1,\ldots,n$$

where $w \in \mathbb{R}^d$ contains regression coefficients.

*Note: The intercept (bias term) is omitted for simplicity.*

### Loss Function

The **mean squared error (MSE)** loss:
$$L(w) = \frac{1}{2} \sum_{i=1}^n (y_i - \langle x_i, w \rangle)^2$$

Or in matrix form:
$$L(w) = \frac{1}{2} \|y - Xw\|^2$$

where $y = (y_1,\ldots,y_n)^T$ and $X$ is the $n \times d$ data matrix.

### Closed-Form Solution

The gradient of the loss function:
$$\nabla L(w) = -X^T (y - Xw)$$

Setting the gradient to zero and solving:
$$X^T X w = X^T y$$

**Normal equations solution**:
$$w^* = (X^T X)^{-1} X^T y$$

### Computational Challenges

1. **Existence**: The matrix $X^T X$ is invertible only if $n \geq d$; for $n < d$, the system is singular.
2. **Computation**: Matrix multiplication requires $O(dn^2)$ time, inversion requires $O(d^3)$ time, making this prohibitively slow for large datasets.

The next lecture introduces **gradient descent**, an algorithm resolving both issues.

---

## Classification and the Perceptron

The perceptron, developed in the early 1950s, represented an early artificial intelligence approach using our three-step recipe.

### Step 1: Representation

The perceptron learns parameters $w \in \mathbb{R}^d$ and bias $b \in \mathbb{R}$ producing binary predictions:
$$y = \text{sign}(\langle w, x \rangle + b)$$

Geometrically, the decision boundary is a hyperplane:
$$\langle w, x \rangle = 0$$

with $w$ perpendicular to this separating hyperplane.

### Step 2: Loss Function

For classification, mean squared error is inappropriate. Instead, the **Hamming distance** counts misclassifications:
$$L(w) = \frac{1}{n} \sum_{i=1}^n \mathbf{1}(y_i \neq \text{sign}(\langle w, x_i \rangle + b))$$

where $\mathbf{1}$ is the indicator function.

### Step 3: Optimization Problem

A fundamental obstacle emerges: neither the Hamming distance nor the sign function is differentiable. Standard gradient-based methods fail, and exhaustive search is computationally intractable.

**Stochastic gradient descent**, discussed in the next lecture, elegantly solves this challenge.

---

## From Linear Models to Neural Networks

Linear regression and perceptron learning share a common structure: computations organized as layers in directed graphs.

### Linear Regression Computational Flow

1. Input $x$ is dotted with weight vector $w$: $\langle w, x \rangle$
2. Predicted output compared with label using squared error:
$$l(y, w^T x) = 0.5 \|y - w^T x\|^2_2$$
3. Training optimizes $w$ to minimize this loss

### Logistic Regression (k-class) Computational Flow

1. Input $x$ is dotted with $k$ weight vectors $w_j$
2. Intermediate output $z = (\langle w_1, x \rangle, \ldots, \langle w_k, x \rangle)$ passes through **softmax**:
$$\hat{y}_j = \frac{\exp(\langle w_j, x \rangle)}{\sum_{j=1}^k \exp(\langle w_j, x \rangle)}$$
3. Cross-entropy loss compares predictions with one-hot labels:
$$l(y, \hat{y}) = -\sum_{i=1}^k y_i \log(\hat{y}_i)$$

### Unified Graph Perspective

Both procedures emerge as computational graphs with shared characteristics:

- Directed, acyclic, feedforward structure
- Edges associated with learnable weights
- Iterative updates via gradient descent

Modern deep learning extends this framework by stacking multiple layers, enabling the hierarchical feature learning that drives contemporary AI systems.
