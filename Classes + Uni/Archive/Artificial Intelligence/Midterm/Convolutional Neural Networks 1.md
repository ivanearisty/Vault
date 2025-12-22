---
tags:
  - cnn
  - deep-learning
  - cheat-sheet
  - machine-learning
  - problem-solving
---
## Computations

### Output Spatial Size
To calculate the height ($H_{out}$) and width ($W_{out}$) of the output feature map from an input of size $H \times W$:
- **Given**: Kernel size ($K$), Padding ($P$), Stride ($S$)
- **Formula**:
  $$
  H_{out}=\left\lfloor\frac{H+2P-K}{S}\right\rfloor+1, \quad W_{out}=\left\lfloor\frac{W+2P-K}{S}\right\rfloor+1
  $$
 

### Parameter Count
To calculate the number of trainable parameters for a 2D convolution layer:
- **Given**: Kernel dimensions ($K_H \times K_W$), Input channels ($C_{in}$), Output channels ($C_{out}$)
- **Formula**:
  $$
  \text{Params} = (K_H \times K_W \times C_{in} \times C_{out}) + C_{out}
  $$
  The first part is the weights, and the second part is the bias for each output channel.

- **Input**: `64x64x32` ($H=64, W=64, C_{in}=32$)
- **Layer**: Conv `3x3` ($K=3$), stride 1 ($S=1$), pad 1 ($P=1$), with 64 filters ($C_{out}=64$).

1.  **Output Shape**:
    - $H_{out} = \lfloor\frac{64 + 2(1) - 3}{1}\rfloor + 1 = 63 + 1 = 64$.
    - $W_{out} = \lfloor\frac{64 + 2(1) - 3}{1}\rfloor + 1 = 63 + 1 = 64$.
    - **Result**: `64x64x64`

2.  **Parameters**:
    - Params = $(3 \times 3 \times 32 \times 64) + 64$
    - Params = $18,432 + 64 = 18,496$.


The receptive field is the size of the region in the input that produces a single feature in the output map.
### Quick Propagation Rule
You can track the receptive field size ($r$) and the "jump" ($j$) between adjacent receptive fields layer by layer.

- **Start (Input Layer)**: $r_0=1$, $j_0=1$.
- **For each new layer** with kernel size ($k$) and stride ($s$):
  - The new jump is the old jump multiplied by the stride: $j_{new} = j_{old} \cdot s$.
  - The new receptive field size is: $r_{new} = r_{old} + (k-1) \cdot j_{old}$.

### Explain the function of a residual block
- State that a residual block computes $y = x + F(x)$, where $x$ is the input (skip connection) and $F(x)$ is the output of the convolutional layers in the block.
- The block's goal is to learn the **residual** ($F(x)$), which is the difference between the target mapping $h(x)$ and the input $x$.
- This helps with optimization in very deep networks by preserving the gradient flow through the identity path ($x$).

### Why use a bottleneck in ResNet-50?
- **Explain the structure**: It's a sequence of `1x1` -> `3x3` -> `1x1` convolutions.
- **Explain the function**:
    - The first `1x1` convolution **reduces** the number of channels (compresses).
    - The `3x3` convolution processes features in this lower-dimensional space.
    - The final `1x1` convolution **restores** the number of channels (expands).
- **State the benefit**: This design provides nearly the same representational power as two full `3x3` convolutions but with a huge reduction in parameters and computational cost.

## Why Questions

- **Why residual connections?** They enable the training of much deeper networks by creating an identity path that preserves gradient flow, combating the vanishing gradient problem.
- **Why Global Average Pooling (GAP) instead of FC layers?** GAP has fewer parameters, which reduces overfitting, and it is more robust to spatial translations of the input.
- **Why Batch Normalization?** It stabilizes the distribution of activations between layers, which allows for faster convergence, permits higher learning rates, and acts as a regularizer.
- **Why non-linearity (e.g., ReLU)?** Without non-linear activation functions, a stack of linear layers (like convolution) would be equivalent to a single, combined linear layer, limiting the model's ability to learn complex patterns.
- **Why does parameter sharing lead to translation equivariance?** Because the same kernel (set of weights) is applied at every position, a shift in the input pattern will result in a corresponding shift in the output feature map.

## Common Pitfalls & Solutions 

- **Overfitting on Small Datasets**: This is a major concern when you have few training samples.
    - **Solution**: Use **data augmentation** (randomly rotating, shifting, zooming images) to create more training data from your existing samples. Also use dropout, weight decay, and transfer learning.
- **Vanishing/Exploding Gradients**: In very deep networks, gradients can become extremely small or large, preventing effective training.
    - **Solution**: Use residual connections, proper weight initialization, batch normalization, and non-saturating activation functions like ReLU.
- **Tiny Batch Sizes**: If your batch size is very small (e.g., 1 or 2), Batch Normalization can be unstable.
    - **Solution**: Consider using Group Normalization or Layer Normalization instead, or accumulate gradients over several small batches.