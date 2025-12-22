# Deep Neural Network Notes

## Example 1: Single Neuron (Sigmoid Activation)

This example walks through a forward and backward pass for a single neuron using the sigmoid activation function.
### Network Diagram

```
      [ w_0, w_1, w_2 ]
            |
x -> z = w^T*x -> a = σ(z)
```

### Setup

Let's define the weight and input vectors:

  * **Weights ($w$)**: $w = \begin{bmatrix} w_0 \\ w_1 \\ w_2 \end{bmatrix} = \begin{bmatrix} 2 \\ -3 \\ 1 \end{bmatrix}$
  * **Inputs ($x$)**: $x = \begin{bmatrix} x_0 \\ x_1 \\ 1 \end{bmatrix} = \begin{bmatrix} -1 \\ -2 \\ 1 \end{bmatrix}$

### Forward Pass

> [\!info] Goal
> Calculate the neuron's output activation, $a$.

1.  **Calculate the linear combination ($z$):**
    $$z = w^T x = w_0x_0 + w_1x_1 + w_2x_2$$
    $$z = (2)(-1) + (-3)(-2) + (1)(1) = -2 + 6 + 1 = 5.0$$

2.  **Apply the sigmoid activation function ($\sigma$):**
    $$a = \sigma(z) = \frac{1}{1 + e^{-z}}$$
    $$a = \sigma(5.0) = \frac{1}{1 + e^{-5}} \approx 0.993$$

-----

### Backward Pass (Backpropagation)

> [\!info] Goal
> Calculate the gradient of the output activation with respect to the weights, $\frac{\partial a}{\partial w}$.

1.  **Apply the Chain Rule:**
    $$\frac{\partial a}{\partial w} = \frac{\partial a}{\partial z} \cdot \frac{\partial z}{\partial w}$$

2.  **Calculate $\frac{\partial a}{\partial z}$:** The derivative of the sigmoid function is $\sigma(z)(1 - \sigma(z))$.
    $$\frac{\partial a}{\partial z} = a \cdot (1 - a) = 0.993 \cdot (1 - 0.993) \approx 0.0069$$

3.  **Calculate $\frac{\partial z}{\partial w}$:**
    $$\frac{\partial z}{\partial w} = \frac{\partial}{\partial w}(w_0x_0 + w_1x_1 + w_2x_2) = \begin{bmatrix} x_0 \\ x_1 \\ x_2 \end{bmatrix} = \begin{bmatrix} -1 \\ -2 \\ 1 \end{bmatrix} = x$$

4.  **Combine the gradients:**
    $$\frac{\partial a}{\partial w} = \frac{\partial a}{\partial z} \cdot x = 0.0069 \cdot \begin{bmatrix} -1 \\ -2 \\ 1 \end{bmatrix} = \begin{bmatrix} -0.0069 \\ -0.0138 \\ 0.0069 \end{bmatrix}$$

-----

## Simple DNN: Backpropagation Walkthrough

This example outlines the forward and backward passes for a simple 2-layer network.

### Network Architecture

The network consists of five sequential steps:

1.  **Fully Connected Layer**: Input $x^{(1)}$ and weights $W^{(1)}$ produce $z^{(1)}$.
2.  **ReLU Activation**: Produces $a^{(1)}$.
3.  **Fully Connected Layer**: Input $a^{(1)}$ and weights $W^{(2)}$ produce $z^{(2)}$.
4.  **Softmax Activation**: Produces the final prediction $\hat{y}$.
5.  **Cross-Entropy Loss**: Produces the loss $L$.

```
x^(1) -> [z^(1) = W^(1)x^(1)] -> ReLU -> a^(1) -> [z^(2) = W^(2)a^(1)] -> Softmax -> y_hat -> [CE Loss] -> L
```

### Forward Pass Equations

| Step | Symbolic Equation                  |
| :--- | :--------------------------------- |
| (1)  | $z^{(1)} = W^{(1)}x^{(1)}$          |
| (2)  | $a^{(1)} = \max(0, z^{(1)})$        |
| (3)  | $z^{(2)} = W^{(2)}a^{(1)}$          |
| (4)  | $\hat{y} = \text{softmax}(z^{(2)})$ |
| (5)  | $L = \text{CE}(y, \hat{y})$         |

-----

### Backward Pass Equations

> [\!info] Goal
> Find the gradients of the loss $L$ with respect to all network parameters ($W^{(1)}$, $W^{(2)}$). Bias terms have been omitted for simplicity.

| Step | Description                                      | Symbolic Equation                                                                                               |
| :--- | :----------------------------------------------- | :-------------------------------------------------------------------------------------------------------------- |
| (5)  | Gradient of Loss w.r.t. Loss                     | $\frac{\partial L}{\partial L} = 1.0$                                                                           |
| (4)  | Gradient w.r.t. pre-softmax output $z^{(2)}$     | $\frac{\partial L}{\partial z^{(2)}} = \hat{y} - y$                                                               |
| (3a) | Gradient w.r.t. second layer weights $W^{(2)}$   | $\frac{\partial L}{\partial W^{(2)}} = (\hat{y} - y) {a^{(1)}}^T$                                                 |
| (3b) | Gradient w.r.t. first layer activation $a^{(1)}$ | $\frac{\partial L}{\partial a^{(1)}} = {W^{(2)}}^T (\hat{y} - y)$                                                 |
| (2)  | Gradient w.r.t. pre-ReLU output $z^{(1)}$        | $\frac{\partial L}{\partial z^{(1)}} = \frac{\partial L}{\partial a^{(1)}} \cdot \mathbb{I}(z^{(1)} > 0)$        |
| (1)  | Gradient w.r.t. first layer weights $W^{(1)}$    | $\frac{\partial L}{\partial W^{(1)}} = \frac{\partial L}{\partial z^{(1)}} {x^{(1)}}^T$                          |

![[Pasted image 20251016170229.png]]
![[Pasted image 20251016170237.png]]
