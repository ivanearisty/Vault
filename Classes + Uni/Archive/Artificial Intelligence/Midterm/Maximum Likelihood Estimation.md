---
tags:
  - statistics
  - machine-learning
  - mle
---

# 🧠 Maximum Likelihood Estimation (MLE)

> [!info] Goal
> Find the optimal parameters ($w^*$) for a given probability distribution ($p(x; w)$) that best explain the observed data.

Let the dataset be $X$, where each data point $x_i$ is drawn from the same distribution:
$$
X = \{x_1, x_2, \dots, x_n\}, \quad x_i \sim p(x; w)
$$

---

## Step-by-Step Guide

### Step 1: Write Down the Likelihood Function ($\mathcal{L}(w)$)
The **likelihood** is the probability of observing all your data points given the parameters ($w$). Assuming the data are **i.i.d.** (independent and identically distributed), the likelihood is the product of the individual probabilities.
$$
\mathcal{L}(w) = P(x_1, x_2, \dots, x_n | w) = \prod_{i=1}^{n} p(x_i; w)
$$
Our goal is to find the parameters $w$ that maximize $\mathcal{L}(w)$.

### Step 2: Take the Logarithm → Log-Likelihood ($\ell(w)$)
Working with products is computationally difficult. Taking the logarithm converts the product into a sum, which is much easier to differentiate. Since the $\log(\cdot)$ function is **monotonic**, maximizing $\mathcal{L}(w)$ is equivalent to maximizing the **log-likelihood** $\ell(w)$.
$$
\ell(w) = \log \mathcal{L}(w) = \log \left( \prod_{i=1}^{n} p(x_i; w) \right) = \sum_{i=1}^{n} \log p(x_i; w)
$$

### Step 3: Differentiate w.r.t. ($w$)
To find the maximum of the log-likelihood function, we take its derivative (or partial derivatives if $w$ is a vector) with respect to the parameters $w$.
$$
\frac{d\ell(w)}{dw}
$$

### Step 4: Set the Derivative Equal to Zero
The maximum (or minimum) of a function occurs at a **stationary point**, where its derivative is zero.
$$
\frac{d\ell(w)}{dw} = 0
$$

### Step 5: Solve for ($w$)
By solving the equation from the previous step for $w$, we find the parameter value(s) that maximize the log-likelihood. This solution is the **Maximum Likelihood Estimate (MLE)**.
$$
w^* = \text{solution from Step 4}
$$

---

## 📈 Example: MLE for the Mean of a Gaussian

Assume our data points are drawn from a **Gaussian (Normal) distribution** with a known variance $\sigma^2$ and an unknown mean $\mu$.
$$
x_i \sim \mathcal{N}(\mu, \sigma^2)
$$
The Probability Density Function (PDF) is:
$$
p(x_i; \mu) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{(x_i - \mu)^2}{2\sigma^2}\right)
$$

### 1. Likelihood Function
$$
\mathcal{L}(\mu) = \prod_{i=1}^{n} \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{(x_i - \mu)^2}{2\sigma^2}\right)
$$

### 2. Log-Likelihood Function
$$
\ell(\mu) = \sum_{i=1}^{n} \log \left[ \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{(x_i - \mu)^2}{2\sigma^2}\right) \right]
$$
Using log rules, $\log(ab) = \log a + \log b$ and $\log(e^x) = x$:
$$
\ell(\mu) = \sum_{i=1}^{n} \left[ \log\left(\frac{1}{\sqrt{2\pi\sigma^2}}\right) - \frac{(x_i - \mu)^2}{2\sigma^2} \right]
$$
Simplifying the constants:
$$
\ell(\mu) = -n \log(\sqrt{2\pi\sigma^2}) - \frac{1}{2\sigma^2} \sum_{i=1}^{n} (x_i - \mu)^2
$$

### 3. Derivative w.r.t. ($\mu$)
The first term is constant with respect to $\mu$, so its derivative is zero. Differentiating the second term:
$$
\frac{d\ell(\mu)}{d\mu} = -\frac{1}{2\sigma^2} \sum_{i=1}^{n} 2(x_i - \mu)(-1) = \frac{1}{\sigma^2} \sum_{i=1}^{n} (x_i - \mu)
$$

### 4. Set Derivative to Zero
$$
\frac{1}{\sigma^2} \sum_{i=1}^{n} (x_i - \mu) = 0
$$
Since $\sigma^2 > 0$, we only need the summation to be zero:
$$
\sum_{i=1}^{n} (x_i - \mu) = 0
$$

### 5. Solve for ($\mu$)
$$
\sum_{i=1}^{n} x_i - \sum_{i=1}^{n} \mu = 0 \implies \sum_{i=1}^{n} x_i - n\mu = 0
$$
$$
\mu^* = \frac{1}{n} \sum_{i=1}^{n} x_i
$$

> [!success] Result
> The Maximum Likelihood Estimate for the mean of a Gaussian distribution is simply the **sample mean**.
> $$
> \boxed{\mu^* = \frac{1}{n} \sum_{i=1}^{n} x_i}
> $$