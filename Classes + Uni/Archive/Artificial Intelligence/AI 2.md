# Artificial Intelligence I – Midterm Study Guide

## [[Log-Likelihood]]

### Definition
The **log-likelihood** is the natural logarithm of the likelihood function. If $L(\theta; \mathbf{y})$ is the likelihood of a parameter $\theta$ given data $\mathbf{y}$, the log-likelihood is $\ell(\theta;\mathbf{y}) = \ln L(\theta;\mathbf{y})$.

Because the logarithm is a **monotonically increasing function**, maximizing the log-likelihood $\ell(\theta;\mathbf{y})$ gives the same parameter estimate as maximizing the original likelihood $L(\theta;\mathbf{y})$. In practice this is convenient: a product of probabilities turns into a sum of log-probabilities, which is easier to work with. Working in log-space also avoids numerical underflow from multiplying many small probabilities.

### Maximum Likelihood Estimation (MLE)
In statistics and machine learning, model parameters are often chosen to maximize the log-likelihood of the observed data. Solving $\displaystyle \arg\max_\theta \ell(\theta;\mathbf{y})$ yields the **Maximum Likelihood Estimator (MLE)** of $\theta$.

Equivalently, one can minimize the **negative log-likelihood (NLL)**, which is a common loss function in machine learning. Many optimization libraries are built to perform minimization, making the NLL a practical choice. For example, in logistic regression, the **cross-entropy loss** is exactly the NLL of the observed labels under the model’s predicted probabilities.

### Key Points
-   **Log-likelihood vs. Likelihood**: Log-likelihood simplifies products into sums without changing the location of the maximum, making it easier to differentiate and analyze.
-   **Negative Log-Likelihood**: Defined as $-\ell(\theta; \mathbf{y})$, it converts a maximization problem into a minimization problem, widely used as a cost function in training algorithms.
-   **Interpretation**: Maximizing the log-likelihood means finding parameters under which the observed data is most probable.

---

## Binary Classification

### Overview
**Binary classification** is the task of predicting one of two possible classes for an example (e.g., positive/negative, spam/ham). A common approach is to model the probability that an input $\mathbf{x}$ belongs to class "1" and then use a threshold (typically 0.5) to assign a final label.

### Logistic Regression
**Logistic regression** is a foundational method for binary classification. It predicts the probability of the positive class using the **sigmoid (or logistic) function**. The model computes a linear combination $z = \theta^T x$ and applies the sigmoid $\sigma(z) = \frac{1}{1 + e^{-z}}$ to map the output to the range $[0,1]$. This output $\sigma(z)$ is interpreted as the probability $\mathbb{P}(y=1 \mid x)$.

### Training Logistic Classifiers (Maximum Likelihood)
The parameters $\theta$ are typically learned by maximizing the log-likelihood of the training data, which is equivalent to minimizing the logistic loss (NLL). For a single example with label $y \in \{0,1\}$ and predicted probability $p=\sigma(z)$, the log-loss is:
$$L(\theta) = -\Big[ y \log p + (1-y)\log(1-p)\Big]$$
Across $m$ examples, the overall cost function to minimize is the average log-loss:
$$J(\theta) = -\frac{1}{m}\sum_{i=1}^{m} \Big[ y^{(i)}\log p^{(i)} + (1 - y^{(i)})\log(1 - p^{(i)})\Big]$$
Minimizing $J(\theta)$ with an algorithm like gradient descent finds the optimal parameters $\theta$. This optimization problem is convex for logistic regression, guaranteeing a global minimum.

### Evaluation Metrics
Accuracy alone can be misleading, especially on imbalanced datasets. Other key metrics include:
-   **Accuracy**: The proportion of correct predictions.
    $$\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}$$
-   **Precision**: Of all positive predictions, the fraction that are actually positive. High precision means few false alarms.
    $$\text{Precision} = \frac{TP}{TP + FP}$$
-   **Recall (Sensitivity)**: Of all actual positive instances, the fraction correctly identified. High recall means few missed positives.
    $$\text{Recall} = \frac{TP}{TP + FN}$$
-   **F1 Score**: The harmonic mean of precision and recall, providing a single balanced measure.
    $$F1 = 2 \times \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}$$
-   **ROC Curve and AUC**: The Receiver Operating Characteristic (ROC) curve plots the True Positive Rate (Recall) against the False Positive Rate at various thresholds. The Area Under the Curve (AUC) summarizes this plot into a single number. An AUC of 1.0 indicates a perfect classifier, while 0.5 indicates performance no better than random guessing.

---

## Deep Neural Networks ([[DNN Examples]])

### Concept
A **deep neural network** is an artificial neural network with multiple **hidden layers** between the input and output. The "deep" aspect allows the network to learn complex, hierarchical features from data by composing many simple non-linear transformations.

### Network Architecture
Neurons are organized into an input layer, one or more hidden layers, and an output layer. In a **fully connected** or **dense layer**, every neuron is connected to every neuron in the next layer. Each neuron computes a weighted sum of its inputs, adds a bias, and applies a non-linear activation function.
-   **Activation Functions**: Common choices include the **ReLU (Rectified Linear Unit)**, defined as $\max(0,z)$, which helps mitigate the vanishing gradient problem. The **softmax** function is often used in the output layer for multi-class classification to produce a probability distribution.

### Forward and Backpropagation
DNNs learn by adjusting weights to minimize a loss function.
1.  **Forward Propagation**: Input data is passed through the network layer by layer to compute a prediction.
2.  **Backpropagation**: The gradient of the loss with respect to each weight is computed efficiently using the chain rule, starting from the output layer and moving backward.
3.  **Weight Update**: An optimizer, like **stochastic gradient descent (SGD)**, uses these gradients to update the weights in the direction that decreases the loss.

This iterative process allows the network to learn layered representations, where deeper layers represent increasingly abstract concepts.

### Key Considerations
Training deep networks requires large amounts of data and can be prone to **overfitting**. **Regularization** techniques like dropout and weight decay are used to combat this. The choice of activation function and weight initialization is also crucial for successful training.

---

## Convolutional Neural Networks (CNN)

### Purpose and Inspiration
A **Convolutional Neural Network (CNN)** is a type of DNN designed for grid-like data, such as images. It uses **convolutional layers** that apply learnable **filters (or kernels)** to local regions of the input, leveraging the spatial structure of the data. This design is inspired by the human visual cortex.

### Key Components of a CNN
A typical CNN architecture consists of a sequence of convolutional, activation, and pooling layers, followed by fully connected layers for classification.
-   **Convolutional Layer**: This is the core building block. It applies a set of filters across the input. Each filter detects a specific feature (like an edge or texture). A key feature is **weight sharing**, where the same filter is used across the entire input, drastically reducing the number of parameters and making the network equivariant to translations. The output is a set of **feature maps**. After convolution, a non-linear activation like **ReLU** is typically applied.
-   **Pooling Layer**: This layer reduces the spatial dimensions of the feature maps, which reduces computation and helps create **translational invariance**. **Max pooling** is the most common type, where the maximum value from a small region of a feature map is taken. This retains the most important information while abstracting its precise location.
-   **Fully Connected Layer**: After several convolution and pooling stages, the high-level feature maps are flattened into a 1D vector and fed into one or more fully connected layers. These layers combine the learned features to make a final prediction, often using a **softmax** activation for the final output.

### Why CNNs Excel in Vision
CNNs have far fewer parameters than a fully connected network of similar size due to local connectivity and weight sharing. Their architecture is tailored for images, encoding the assumption that local pixels are highly correlated and that features can appear anywhere in an image (**translational equivariance**). By stacking layers, CNNs automatically learn a hierarchy of features, from simple edges in early layers to complex object parts in deeper layers, eliminating the need for manual feature engineering.

---

## Kalman Filters

### Overview
The **Kalman filter** is a recursive algorithm that provides an optimal estimate of the state of a dynamic system from a series of noisy measurements. It assumes the system is linear and that all noise is Gaussian. It operates in a **predict-then-update cycle**: it predicts the next state and then corrects this prediction using the latest measurement.

### State-Space Model
The filter assumes the system can be described by two linear equations:
-   **State Transition Model**: Predicts the next state based on the current one.
    $x_k = A x_{k-1} + B u_{k-1} + w_{k-1}$
-   **Measurement Model**: Relates the hidden state to the observed measurement.
    $z_k = H x_k + v_k$
Here, $x_k$ is the state, $z_k$ is the measurement, $w_{k-1}$ is the process noise (covariance $Q$), and $v_k$ is the measurement noise (covariance $R$).

### Recursion
The filter maintains an estimate of the state $\hat{x}$ and its uncertainty (covariance $P$). It iterates through two steps:

#### 1. Prediction (Time Update)
Predict the next state and its covariance based on the system model. Uncertainty increases during this step.
$$\hat{x}_{k|k-1} = A\hat{x}_{k-1|k-1} + Bu_{k-1}$$
$$P_{k|k-1} = AP_{k-1|k-1}A^T + Q$$

#### 2. Update (Measurement Update)
Correct the prediction using the new measurement $z_k$. Uncertainty decreases during this step.
$$K_k = P_{k|k-1}H^T(HP_{k|k-1}H^T + R)^{-1}$$
$$\hat{x}_{k|k} = \hat{x}_{k|k-1} + K_k(z_k - H\hat{x}_{k|k-1})$$
$$P_{k|k} = (I - K_kH)P_{k|k-1}$$

The **Kalman gain** ($K_k$) is a crucial component that determines how much the prediction is corrected by the measurement. It balances the uncertainty of the prediction with the uncertainty of the measurement.

### Interpreting the Kalman Gain
-   If measurement noise ($R$) is low, the gain $K_k$ is high, and the filter trusts the new measurement more.
-   If prediction uncertainty ($P_{k|k-1}$) is high, the gain $K_k$ is also high, again favoring the measurement.
-   Conversely, if the measurement is noisy or the prediction is confident, the gain is low, and the filter relies more on its prediction.

### Applications
Kalman filters are used in tracking, navigation (GPS), robotics, and sensor fusion. For non-linear systems, variants like the **Extended Kalman Filter (EKF)** and **Unscented Kalman Filter (UKF)** are used. For non-Gaussian systems, the **Particle Filter** is an option.