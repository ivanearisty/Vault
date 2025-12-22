Of course. Here is the revised formatting for your Obsidian notes.

# Overview of Key Concepts
`#machine-learning` `#statistics` `#neural-networks` `#concepts`

This note provides an overview of several fundamental concepts: Log-Likelihood, Binary Classification, Deep Neural Networks (DNN), Convolutional Neural Networks (CNN), and the Kalman Filter.

---

## 🧠 Log-Likelihood
> [!info] Definition
> The **log-likelihood** of a statistical model is the natural logarithm of its likelihood function. If the likelihood $L(\theta|D)$ is the probability of observing data $D$ given parameters $\theta$, the log-likelihood is $\log L(\theta|D)$.

Using the log-likelihood is convenient because it transforms products of probabilities into sums, which greatly simplifies mathematical analysis. It's a central component of **Maximum Likelihood Estimation (MLE)**, where one finds the best-fitting model parameters by maximizing the log-likelihood.

In practice, machine learning models often minimize the **negative log-likelihood** as a loss function. This transformation also enhances numerical stability by preventing the underflow that can occur when multiplying many small probabilities together. In short, log-likelihood offers a robust and convenient metric for how well a model explains observed data.

---

## 🎯 Binary Classification
> [!info] Definition
> **Binary classification** is the task of predicting one of two possible classes for a given input. It deals with a binary outcome, such as yes/no, true/false, or positive/negative.

Examples include determining if an email is _spam_ or _not spam_, or if a patient has a _disease_ or not. It's a foundational task in many fields, from medical diagnostics to finance.

### Evaluating Performance
A classifier's predictions can be categorized into four outcomes:
-   **True Positives (TP):** Correctly predicted positive cases.
-   **True Negatives (TN):** Correctly predicted negative cases.
-   **False Positives (FP):** Incorrectly predicted positive cases (a "false alarm").
-   **False Negatives (FN):** Incorrectly predicted negative cases (a "miss").


These outcomes are often visualized in a **confusion matrix**. The trade-off between FP and FN is a critical consideration. In medical testing, for instance, a false negative (failing to detect a disease) is often far more dangerous than a false positive, so models are tuned accordingly.

---

## 🕸️ Deep Neural Networks (DNN)
> [!info] Definition
> A **Deep Neural Network (DNN)** is an artificial neural network with two or more hidden layers between its input and output layers. The practice of training these networks is known as **deep learning**.

Each layer in a DNN contains nodes that apply learned weights and activation functions to the data they receive. By stacking multiple layers, a DNN can learn **hierarchical feature representations**—early layers detect simple features, while deeper layers combine them into more complex patterns.

This depth allows DNNs to model highly complex functions and achieve impressive performance on tasks like image classification, natural language processing, and speech recognition. While training them requires significant data and computation, and they can be difficult to interpret (often called "black boxes"), their high accuracy has made them a cornerstone of modern AI.

---

## 🖼️ Convolutional Neural Networks (CNN)
> [!info] Definition
> A **Convolutional Neural Network (CNN)** is a type of DNN specifically designed to process data with a grid-like topology, such as images.

Instead of fully connected layers, CNNs use **convolutional layers** that apply sliding filters (kernels) to detect local features like edges, textures, or shapes.

### Core Architecture
A typical CNN architecture includes:
1.  **Convolutional Layers:** Extract feature maps by applying filters across the input.
2.  **Pooling Layers:** Down-sample the feature maps (e.g., using max pooling) to reduce dimensionality and increase robustness.
3.  **Fully Connected Layers:** Perform the final classification based on the extracted features.


By using **weight sharing** (reusing the same filter across an image), CNNs dramatically reduce the number of model parameters compared to fully connected networks. This makes them more efficient to train and acts as a form of regularization. CNNs have long been the standard for computer vision and have driven breakthroughs in image recognition, object detection, and medical image analysis.

---

## 🛰️ Kalman Filter
> [!info] Definition
> The **Kalman filter** is a powerful algorithm for estimating the state of a dynamic system from a series of noisy measurements. It continuously updates its estimate as new data arrives, accounting for uncertainty in both the system's dynamics and the measurements themselves.

It is a recursive, real-time algorithm ideal for applications like tracking and navigation.

### The Two-Step Process
The Kalman filter operates in a repeating cycle:
-   **Prediction (Time Update):** It uses a model of the system's dynamics to predict the next state and its uncertainty based on the previous estimate. Essentially, it asks: "Given where we were, where do we expect to be now?"
-   **Update (Measurement Update):** When a new measurement arrives, the filter corrects its prediction by calculating a weighted average of the predicted state and the new measurement. More certain information receives a higher weight.

The Kalman filter is **optimal** for linear systems with Gaussian noise, meaning no other linear estimator can achieve a lower mean squared error. It is fundamental to sensor fusion and tracking problems in fields like aerospace (navigation for the Apollo missions), autonomous vehicles, robotics, and economics. For non-linear systems, variants like the **Extended Kalman Filter (EKF)** and **Unscented Kalman Filter (UKF)** are used.