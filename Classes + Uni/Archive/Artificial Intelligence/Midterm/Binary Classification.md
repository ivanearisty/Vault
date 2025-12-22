## Problem Setup and Intuition

Binary classification predicts a **discrete label**  
$$y \in \{0, 1\}$$  
given an input vector  
$$x \in \mathbb{R}^n$$  

We want a model that outputs  
$$\hat{y} = p(y=1|x)$$  
interpreted as the **posterior probability** of the positive class.

> [!note] Discriminative vs Generative
> | Type | What it models | Example |
> |------|----------------|----------|
> | **Discriminative** | Decision boundary directly $g(x)\!\to\!y$ | Perceptron, SVM |
> | **Probabilistic Discriminative** | Posterior $p(C_k\|x)$ directly | **Logistic Regression** |
> | **Probabilistic Generative** | $p(x\|C_k)$ and prior $p(C_k)$ then Bayes rule | Naive Bayes, LDA |

---

## 2️⃣ Logistic Regression Model

### Log-Odds and Sigmoid Function

If $p$ is the probability of an event:
$$
\text{odds} = \frac{p}{1 - p}
$$

The **logit** (log-odds) transforms probabilities into linear space:
$$
\text{logit}(p) = \ln\frac{p}{1-p} = w^\top x
$$

The **sigmoid** maps linear outputs back to probabilities:
$$
\sigma(z) = \frac{1}{1 + e^{-z}}
$$

Thus:
$$
\hat{y} = p(y=1|x) = \sigma(w^\top x)
$$

---

### Posterior from Bayes’ Rule

$$
p(y=1|x) = \frac{p(x|y=1)p(y=1)}{p(x|y=1)p(y=1) + p(x|y=0)p(y=0)}
$$

Taking log-odds:
$$
\ln\frac{p(y=1|x)}{p(y=0|x)} = w^\top x
$$

> [!tip]
> Logistic regression is **linear in features**, but its **output is probabilistic** through the sigmoid.

---

## 3️⃣ Likelihood → Binary Cross-Entropy

Since \(y\) is discrete → use **Bernoulli likelihood**.

For one example:
$$
p(y_i|x_i;w) = \hat{y_i}^{y_i} (1-\hat{y_i})^{1-y_i}
$$

Full dataset (i.i.d.):
$$
\mathcal{L}(w) = \prod_{i=1}^{m} \hat{y_i}^{y_i} (1-\hat{y_i})^{1-y_i}
$$

Log-likelihood:
$$
\log\mathcal{L}(w) = \sum_{i=1}^{m} [y_i \log(\hat{y_i}) + (1-y_i)\log(1-\hat{y_i})]
$$

Minimize **negative log-likelihood → Binary Cross-Entropy (BCE):**
$$
L(w) = -\frac{1}{m}\sum_{i=1}^{m}\left[y_i\log(\hat{y_i}) + (1-y_i)\log(1-\hat{y_i})\right]
$$

> [!summary]
> **Intuition:**  
> - Penalizes wrong confident predictions exponentially.  
> - Encourages probabilistic calibration.

---

## 4️⃣ Gradient and Optimization

Gradient of BCE:
$$
\nabla_w L(w) = \frac{1}{m}\sum_{i=1}^{m} (\hat{y_i}-y_i)x_i
$$

Update rule (Gradient Descent):
$$
w := w - \eta \, \nabla_w L(w)
$$

> [!note]
> Works with:
> - **Batch GD:** all samples  
> - **Mini-batch / SGD:** subset of samples per update

---

## 5️⃣ Decision Boundary and Inference

During inference:

$$
\hat{y} = \sigma(w^\top x)
$$

Prediction rule:
$$
\text{class} =
\begin{cases}
1 & \text{if } \hat{y} \ge 0.5 \\
0 & \text{otherwise}
\end{cases}
$$

> [!tip]
> Threshold can be tuned for better precision–recall balance.

---

## 6️⃣ Evaluation Metrics

| Symbol | Meaning |
|---------|----------|
| TP | True Positive |
| TN | True Negative |
| FP | False Positive |
| FN | False Negative |

**Derived Metrics:**

$$
\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}
$$  
$$
\text{Precision} = \frac{TP}{TP + FP}
$$  
$$
\text{Recall (TPR)} = \frac{TP}{TP + FN}
$$  
$$
\text{F1} = 2 \cdot \frac{\text{Precision}\cdot\text{Recall}}{\text{Precision}+\text{Recall}}
$$  

> **ROC / AUC:** TPR vs FPR as threshold varies; AUC measures separability.

---

## 7️⃣ Common Exam-Style Prompts

> [!check] Likely Asked
> - **Write the likelihood and derive BCE** → start from Bernoulli, take log, negate.  
> - **Compute gradient** → $\nabla_w L = (\hat{y}-y)x$.  
> - **Why not Gaussian likelihood?** → y is discrete.  
> - **Meaning of sigmoid?** → converts linear score to probability.  
> - **Interpret output?** → $\hat{y} = P(y=1|x)$.  
> - **Compute confusion matrix + metrics** → given predictions & labels.

---

## 8️⃣ Visual Intuition

> [!info]
> - **Sigmoid curve:** squashes to (0,1).  
> - **Decision boundary:** $w^\top x = 0$.  
> - **Loss curve:** rises sharply for confident misclassifications.

---

## 9️⃣ Summary Table

| Concept    | Formula                                  | Notes                 |     |
| ---------- | ---------------------------------------- | --------------------- | --- |
| Model      | $\hat{y} = \sigma(w^\top x)$             | Posterior probability |     |
| Likelihood | $p(yx;w) = \hat{y}^y(1-\hat{y})^{1-y}$   | Bernoulli             |     |
| Loss       | $-[y\log\hat{y} + (1-y)\log(1-\hat{y})]$ | Cross-Entropy         |     |
| Gradient   | $(\hat{y}-y)x$                           | For GD/SGD            |     |
| Sigmoid    | $\frac{1}{1+e^{-z}}$                     | Maps to (0,1)         |     |
| Boundary   | $w^\top x=0$                             | Linear separator      |     |
| Metrics    | Accuracy, Precision, Recall, F1, AUC     | Evaluation            |     |

---

## 🧩 Bonus Derivation: Bernoulli → Log-Loss

> [!example] Step-by-Step
> 1. $p(y_i|x_i;w)=\hat{y}_i^{y_i}(1-\hat{y}_i)^{1-y_i}$
> 2. $\mathcal{L}(w)=\prod_i p(y_i|x_i;w)$
> 3. $\log\mathcal{L}=\sum_i [y_i\log\hat{y}_i+(1-y_i)\log(1-\hat{y}_i)]$
> 4. $L(w)=-\log\mathcal{L}$
> **Result:** Binary Cross-Entropy
> $L=-\sum_i[y_i\log\hat{y}_i+(1-y_i)\log(1-\hat{y}_i)]$
