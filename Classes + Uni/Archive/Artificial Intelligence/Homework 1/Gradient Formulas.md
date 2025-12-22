# Question

We consider a 2-component Mixture of Gaussians (MoG) with 1-dimensional data. Show that the gradients you need for solving the estimation problem are as follows. Use LaTeX math in markdown format for this task.

The likelihood for a point $x_i$ is
$$p(x_i \mid \pi, \mu, \sigma^2) = \pi_1 \mathcal{N}(x_i \mid \mu_1, \sigma_1^2) + \pi_2 \mathcal{N}(x_i \mid \mu_2, \sigma_2^2).$$

The log-likelihood for $m$ data points is
$$\mathcal{L} = \sum_{i=1}^{m} \log \left( \sum_{k=1}^{2} \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right).$$

Define the responsibility of component $k$ for data point $i$:
$$\gamma_{ik} = \frac{\pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2)}{\sum_{j=1}^{2} \pi_j \mathcal{N}(x_i \mid \mu_j, \sigma_j^2)}$$

Then the gradients are:

* **Mean gradient**:
    $$\frac{\partial \mathcal{L}}{\partial \mu_k} = \sum_{i=1}^{m} \gamma_{ik} \frac{(x_i - \mu_k)}{\sigma_k^2}.$$

* **Variance gradient**:
    $$\frac{\partial \mathcal{L}}{\partial \sigma_k^2} = \frac{1}{2} \sum_{i=1}^{m} \gamma_{ik} \left[ \frac{(x_i - \mu_k)^2}{\sigma_k^4} - \frac{1}{\sigma_k^2} \right].$$

* **Mixture weights gradient** (with constraint $\sum_k \pi_k = 1$):
    $$\frac{\partial \mathcal{L}}{\partial \pi_k} = \sum_{i=1}^{m} \frac{\gamma_{ik}}{\pi_k}.$$

# Some Notes
## Likelihood of a point
- The model is a **mixture** of two Gaussian distributions
- $\mathcal{N}(x_i \mid \mu_k, \sigma_k^2)$ is the probability density function (PDF) for a normal distribution, representing the likelihood of $x_{i}$​ belonging to a specific component k with mean $μk$​ and variance $\sigma_{k}^{2}$.
- πk​ is the **mixture weight** for component k. It represents the prior probability that a data point comes from that component. For example, if π1​=0.7 and π2​=0.3, it means there's a 70% chance a data point belongs to the first component and a 30% chance it belongs to the second.
- The total likelihood of a point is the **sum of the likelihoods from each component**, weighted by their respective mixture weights.

## Log-likelihood for all points

- **Log-likelihood** is a mathematical transformation used to simplify calculations. Since the logarithm is a monotonically increasing function, maximizing the log-likelihood is equivalent to maximizing the original likelihood. This turns a product of probabilities into a sum of logarithms, which is much easier to differentiate.
- The outer summation​ assumes that the data points are independent and identically distributed (i.i.d.).
- The inner summation is the mixture of the two Gaussian components, as defined in the first equation.

## Responsibility of a component

- The **numerator**, $\pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2)$, is the joint probability of the data point xi​ and its being assigned to component k.
- The **denominator** is the sum of the joint probabilities over all components, which acts as a normalizing constant to ensure that the responsibilities for a single data point sum to 1.
- In simpler terms, $\gamma_{ik}$​ tells you the **proportion of the data point's total likelihood that is "responsible" for by component k**. 
# Answer

## Deriving Mean Gradient

$$
\begin{gather}
\text{We have the log-likelihood: } \\
\mathcal{L} = \sum_{i=1}^{m} \log \left( \sum_{k=1}^{2} \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right) \\ \\
\text{We need to take the derivative with respect to } \mu_{k}. \\ \\ 
\text{From the chain rule for logs, we know: } \\
\frac{d}{dx}\ln(f(x)) = \frac{f'(x)}{f(x)} \\
\\
1. \text{The derivate of a sum is the sum of the derivatives:} \\
\frac{\partial \mathcal{L}}{\partial \mu_k} = \sum_{i=1}^{m} \frac{\partial \mathcal{L}}{\partial \mu_k} \left[ \log \left( \sum_{k=1}^{2} \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right) \right] \\ \\
2. \text{Apply chain rule} \\ 
\frac{\partial \mathcal{L}}{\partial \mu_k} = \sum ^{m}_{i=1} \frac{1}{\left( \sum_{k=1}^{2} \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right) } \times \frac{\partial \mathcal{L}}{\partial \mu_k}\left( \sum_{k=1}^{2} \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right) \\ \\
3. \text{Differentiate inner sum} \\
\frac{\partial \mathcal{L}}{\partial \mu_k} = \sum ^{m}_{i=1} \frac{1}{\left( \sum_{k=1}^{2} \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right) } \times \left[ \pi_k \frac{\partial}{\partial \mu_k} \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right] \\ \\
4. \text{Differentiate the Gaussian} \\
\frac{\partial}{\partial \mu_k} \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) = \frac{\partial}{\partial \mu_k} \left[ \frac{1}{\sqrt{2\pi\sigma_k^2}} \exp \left( -\frac{(x_i - \mu_k)^2}{2\sigma_k^2} \right) \right] \\
\text{Using the chain rule,} \frac{d}{dx} e^{f(x)} = f'(x)e^{f(x)}: \\

= \frac{1}{\sqrt{2\pi\sigma_k^2}} \cdot \exp \left( -\frac{(x_i - \mu_k)^2}{2\sigma_k^2} \right) \cdot \frac{\partial}{\partial \mu_k} \left[ -\frac{(x_i - \mu_k)^2}{2\sigma_k^2} \right] \\
= \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \cdot \left[ -\frac{1}{2\sigma_k^2} \cdot 2(x_i - \mu_k) \cdot (-1) \right] \\
= \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \cdot \frac{x_i - \mu_k}{\sigma_k^2} \\
\\
5. \text{Substituing:} \\
\frac{\partial \mathcal{L}}{\partial \mu_k} = \sum_{i=1}^{m} \frac{1}{\sum_{j=1}^{2} \pi_j \mathcal{N}(x_i \mid \mu_j, \sigma_j^2)} \cdot \left[ \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \frac{x_i - \mu_k}{\sigma_k^2} \right] \\ \\
\frac{\partial \mathcal{L}}{\partial \mu_k} = \sum_{i=1}^{m} \frac{\pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2)}{\sum_{j=1}^{2} \pi_j \mathcal{N}(x_i \mid \mu_j, \sigma_j^2)} \cdot \frac{x_i - \mu_k}{\sigma_k^2} \\ \\ 
\text{ From the responsibility of component k for data point i:} \\
\gamma_{ik} = \frac{\pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2)}{\sum_{j=1}^{2} \pi_j \mathcal{N}(x_i \mid \mu_j, \sigma_j^2)} \\ \\
\frac{\partial \mathcal{L}}{\partial \mu_k} = \sum_{i=1}^{m} \gamma_{ik} \frac{x_i - \mu_k}{\sigma_k^2}
\end{gather}
$$
#note-to-self Review [calculus](https://www.khanacademy.org/math/multivariable-calculus)
## Derive the variance gradient

$$
\begin{gather}
\text{We need to show that: } \\
\frac{\partial \mathcal{L}}{\partial \sigma_k^2} = \frac{1}{2} \sum_{i=1}^{m} \gamma_{ik} \left[ \frac{(x_i - \mu_k)^2}{\sigma_k^4} - \frac{1}{\sigma_k^2} \right] \\
\text{is the variance gradient.} \\ \\

\text{We have the log-likelihood: } \\
\mathcal{L} = \sum_{i=1}^{m} \log \left( \sum_{k=1}^{2} \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right) \\ \\
\text{We need to take the derivative with respect to } \sigma^{2}_{k}. \\ \\ 
1. \text{The derivate of a sum is the sum of the derivatives:} \\
\frac{\partial \mathcal{L}}{\partial \sigma^{2}_{k}} = \sum_{i=1}^{m} \frac{\partial \mathcal{L}}{\partial \sigma^{2}_{k}} \left[ \log \left( \sum_{k=1}^{2} \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right) \right] \\ \\
2. \text{Apply chain rule} \\ 
\frac{\partial \mathcal{L}}{\partial \sigma_k^2} = \sum ^{m}_{i=1} \frac{1}{\left( \sum_{k=1}^{2} \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right) } \times \frac{\partial \mathcal{L}}{\partial \sigma_k^2}\left( \sum_{k=1}^{2} \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right) \\ \\
\end{gather}
$$
$$
\begin{gather}
3. \text{Differentiate the gaussian with respect to the variance}: \\
\mathcal{N} = (2\pi\sigma^{2}_{k})^{-1/2} \exp\left( -\frac{(x_{i}-\mu_{k})^{2}}{2\sigma^{2}_{k}} \right) \\
\frac{\partial \mathcal{N}}{\partial\sigma^{2}_{k}} = (2\pi\sigma^{2}_{k})^{-1/2} \exp\left( -\frac{(x_{i}-\mu_{k})^{2}}{2\sigma^{2}_{k}} \right) \\ 
=\underbrace{\left[ \frac{\partial}{\partial \sigma_k^2} (2\pi\sigma_k^2)^{-1/2} \right]}_{f'} \underbrace{\exp \left( -\frac{(x_i - \mu_k)^2}{2\sigma_k^2} \right)}_{g} + \underbrace{(2\pi\sigma_k^2)^{-1/2}}_{f} \underbrace{\left[ \frac{\partial}{\partial \sigma_k^2} \exp \left( -\frac{(x_i - \mu_k)^2}{2\sigma_k^2} \right) \right]}_{g'} \\

\end{gather}
$$
$$f' = \frac{\partial}{\partial \sigma_k^2} (2\pi\sigma_k^2)^{-1/2}$$Using the chain rule, where the outer function is $u^{-1/2}$ and the inner function is $u = 2\pi\sigma_k^2$:
$$f' = -\frac{1}{2}(2\pi\sigma_k^2)^{-3/2} \cdot \frac{\partial}{\partial \sigma_k^2}(2\pi\sigma_k^2)$$
$$f' = -\frac{1}{2}(2\pi\sigma_k^2)^{-3/2} \cdot (2\pi) = -\frac{1}{2\sigma_k^2} (2\pi\sigma_k^2)^{-1/2} = -\frac{1}{2\sigma_k^2} \mathcal{N}$$
Differentiating the second term, $g'$
$$g' = \frac{\partial}{\partial \sigma_k^2} \exp\left( -\frac{(x_{i}-\mu_{k})^{2}}{2\sigma^{2}_{k}} \right)$$Using the chain rule, where the outer function is $e^u$ and the inner function is $u = -\frac{(x_{i}-\mu_{k})^{2}}{2\sigma^{2}_{k}}$:
$$g' = \exp\left( -\frac{(x_{i}-\mu_{k})^{2}}{2\sigma^{2}_{k}} \right) \cdot \frac{\partial}{\partial \sigma_k^2}\left(-\frac{(x_{i}-\mu_{k})^{2}}{2\sigma^{2}_{k}}\right)$$$$g' = g \cdot \left[ -\frac{(x_{i}-\mu_{k})^{2}}{2} \cdot \frac{\partial}{\partial \sigma_k^2}(\sigma_k^2)^{-1} \right]$$$$g' = g \cdot \left[ -\frac{(x_{i}-\mu_{k})^{2}}{2} \cdot (-1)(\sigma_k^2)^{-2} \right]$$
$$g' = g \cdot \frac{(x_{i}-\mu_{k})^{2}}{2\sigma_k^4}$$
Now we have:
$$\frac{\partial \mathcal{N}}{\partial\sigma^{2}_{k}} = \underbrace{\left( -\frac{1}{2\sigma_k^2} \frac{1}{\sqrt{2\pi\sigma_k^2}} \right)}_{f'} \underbrace{\left( \exp\left(-\frac{(x_i - \mu_k)^2}{2\sigma_k^2}\right) \right)}_{g} + \underbrace{\left( \frac{1}{\sqrt{2\pi\sigma_k^2}} \right)}_{f} \underbrace{\left( \left(\exp\left(-\frac{(x_i - \mu_k)^2}{2\sigma_k^2}\right)\right) \frac{(x_{i}-\mu_{k})^{2}}{2\sigma_k^4} \right)}_{g'}$$
Rearranging and substituing:
$$\frac{\partial \mathcal{N}}{\partial\sigma^{2}_{k}} = f'g + fg' = \left( -\frac{1}{2\sigma_k^2} \mathcal{N} \right) +\left(  \mathcal{N} \left( \frac{(x_{i}-\mu_{k})^{2}}{2\sigma_k^4} \right) \right)$$Factor out the common term $\mathcal{N}$:
$$\frac{\partial \mathcal{N}}{\partial\sigma^{2}_{k}} = \mathcal{N} \left[ \frac{(x_{i}-\mu_{k})^{2}}{2\sigma_k^4} - \frac{1}{2\sigma_k^2} \right]$$Factoring out $\frac{1}{2\sigma_k^2}$: 
$$\frac{\partial \mathcal{N}}{\partial\sigma^{2}_{k}} = \frac{1}{2\sigma_k^2} \mathcal{N} \left[ \frac{(x_{i}-\mu_{k})^{2}}{\sigma_k^2} - 1 \right]$$

4. Susbtitute back and simplify:
$$
\frac{\partial \mathcal{L}}{\partial \sigma_k^2} = \sum ^{m}_{i=1} \frac{1}{\left( \sum_{k=1}^{2} \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right) } \times \frac{\partial \mathcal{L}}{\partial \sigma_k^2}\left( \sum_{k=1}^{2} \pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2) \right) \\ \\
$$

The derivative of the inner sum with respect to $\sigma_k^2$ affects only the $k$-th term, so it becomes $\pi_k \frac{\partial \mathcal{N}_k}{\partial \sigma_k^2}$. Substituting this in:
$$\frac{\partial \mathcal{L}}{\partial \sigma_k^2} = \sum_{i=1}^{m} \frac{1}{\sum_{j=1}^{2} \pi_j \mathcal{N}_j} \cdot \left[ \pi_k \frac{\partial \mathcal{N}_k}{\partial \sigma_k^2} \right]$$
$$\frac{\partial \mathcal{L}}{\partial \sigma_k^2} = \sum_{i=1}^{m} \frac{1}{\sum_{j=1}^{2} \pi_j \mathcal{N}_j} \cdot \left[ \pi_k \mathcal{N}_k \left( \frac{(x_i - \mu_k)^2}{2\sigma_k^4} - \frac{1}{2\sigma_k^2} \right) \right]$$

We rearrange the terms to match the definition of responsibility, $\gamma_{ik}$:
$$\frac{\partial \mathcal{L}}{\partial \sigma_k^2} = \sum_{i=1}^{m} \underbrace{\frac{\pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2)}{\sum_{j=1}^{2} \pi_j \mathcal{N}(x_i \mid \mu_j, \sigma_j^2)}}_{\gamma_{ik}} \cdot \left( \frac{(x_i - \mu_k)^2}{2\sigma_k^4} - \frac{1}{2\sigma_k^2} \right)$$
$$\frac{\partial \mathcal{L}}{\partial \sigma_k^2} = \frac{1}{2} \sum_{i=1}^{m} \gamma_{ik} \left[ \frac{(x_i - \mu_k)^2}{\sigma_k^4} - \frac{1}{\sigma_k^2} \right]$$

## Deriving the Mixture Weights Gradient

1. Apply the chain rule:

We start with the log-likelihood and differentiate with respect to $\pi_k$:
$$\frac{\partial \mathcal{L}}{\partial \pi_k} = \sum_{i=1}^{m} \frac{\partial}{\partial \pi_k} \log \left( \sum_{j=1}^{2} \pi_j \mathcal{N}(x_i \mid \mu_j, \sigma_j^2) \right)$$
$$\frac{\partial \mathcal{L}}{\partial \pi_k} = \sum_{i=1}^{m} \frac{1}{\sum_{j=1}^{2} \pi_j \mathcal{N}_j} \cdot \frac{\partial}{\partial \pi_k} \left( \sum_{j=1}^{2} \pi_j \mathcal{N}_j \right)$$

2. Differentiate the inner sum:
The derivative of the inner sum, $\pi_1 \mathcal{N}_1 + \pi_2 \mathcal{N}_2$, with respect to a specific $\pi_k$ is its coefficient, $\mathcal{N}_k$:   $$\frac{\partial}{\partial \pi_k} \left( \sum_{j=1}^{2} \pi_j \mathcal{N}_j \right) = \mathcal{N}(x_i \mid \mu_k, \sigma_k^2)$$

3. Substitute back and simplify:
$$\frac{\partial \mathcal{L}}{\partial \pi_k} = \sum_{i=1}^{m} \frac{\mathcal{N}(x_i \mid \mu_k, \sigma_k^2)}{\sum_{j=1}^{2} \pi_j \mathcal{N}(x_i \mid \mu_j, \sigma_j^2)}$$
To introduce the responsibility term we can multiply the numerator and denominator by $\pi_k$:
$$\frac{\partial \mathcal{L}}{\partial \pi_k} = \sum_{i=1}^{m} \frac{1}{\pi_k} \cdot \frac{\pi_k \mathcal{N}(x_i \mid \mu_k, \sigma_k^2)}{\sum_{j=1}^{2} \pi_j \mathcal{N}(x_i \mid \mu_j, \sigma_j^2)}$$
This gives us the final gradient for the mixture weights:
$$\frac{\partial \mathcal{L}}{\partial \pi_k} = \sum_{i=1}^{m} \frac{\gamma_{ik}}{\pi_k}$$

# More notes

### Mean Gradient

-   The formula is $\frac{\partial \mathcal{L}}{\partial \mu_k} = \sum_{i=1}^{m} \gamma_{ik} \frac{(x_i - \mu_k)}{\sigma_k^2}.$
-   This gradient can be seen as a **responsibility-weighted sum of errors**. Each data point $x_i$ "pulls" the mean $\mu_k$ towards it.
-   The strength of this pull is determined by two factors:
    1.  **Responsibility ($\gamma_{ik}$)**: If component *k* is highly responsible for point $x_i$ (i.e., $\gamma_{ik}$ is close to 1), that point has a strong influence on updating $\mu_k$. If $\gamma_{ik}$ is close to 0, the point has virtually no say.
    2.  **Error scaled by precision ($(x_i - \mu_k)/\sigma_k^2$)**: The term $(x_i - \mu_k)$ is the distance from the point to the mean. The gradient will push $\mu_k$ in the direction that closes this gap. This pull is amplified for components with smaller variance (higher precision, $1/\sigma_k^2$), as even small errors are significant for a "tight" cluster.

### Variance Gradient

-   The formula is $\frac{\partial \mathcal{L}}{\partial \sigma_k^2} = \frac{1}{2} \sum_{i=1}^{m} \gamma_{ik} \left[ \frac{(x_i - \mu_k)^2}{\sigma_k^4} - \frac{1}{\sigma_k^2} \right].$
-   This gradient works to make the variance $\sigma_k^2$ equal to the **average squared distance** of the points assigned to that component.
-   The term inside the brackets, $\frac{(x_i - \mu_k)^2}{\sigma_k^4} - \frac{1}{\sigma_k^2}$, compares the squared error of a point, $(x_i - \mu_k)^2$, to the current variance $\sigma_k^2$.
    -   If, on average, the points assigned to component *k* have a squared distance **greater** than the current variance, the gradient will be positive, pushing to **increase** $\sigma_k^2$ to better capture their spread.
    -   If their squared distance is **less** than the current variance, the gradient will be negative, pushing to **decrease** $\sigma_k^2$ because the current component is too "wide" for the data it represents.

### Mixture Weights Gradient

-   The formula is $\frac{\partial \mathcal{L}}{\partial \pi_k} = \sum_{i=1}^{m} \frac{\gamma_{ik}}{\pi_k}.$
-   The numerator, $\sum_{i=1}^{m} \gamma_{ik}$, can be interpreted as the **effective number of data points** assigned to component *k*. Let's call this $N_k$.
-   The gradient is therefore $\frac{N_k}{\pi_k}$. This tells us to increase the mixture weight $\pi_k$ if the component is responsible for a large number of points ($N_k$ is high) and decrease it if the component is responsible for few points.
-   Intuitively, if a component explains a large fraction of the data, its prior probability (mixture weight) should be increased. If it explains very little, its weight should be decreased.