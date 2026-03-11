# Lecture 4: Convolutional Neural Networks - Deep Learning

## Beyond Fully Connected Networks

Traditional fully connected neural networks face three major limitations when applied to image classification:

**Computational Cost**: A simple two-layer network processing 400×400 RGB images with 1000 hidden neurons for 10-class classification requires nearly 500 million trainable parameters. Parameter count scales roughly as $O(\tilde{d}^2 L)$, creating significant computational bottlenecks for deeper architectures.

**Loss of Context**: Flattening images into vectors destroys spatial relationships. The classifier should still perform well if an image shifts slightly, but standard fully connected networks lack mechanisms to preserve spatial correlations. Similar problems arise in audio and NLP applications where temporal structure matters.

**Confounding Features**: Real-world images contain occlusions, background clutter, and varying illumination conditions that curated datasets like MNIST don't represent. Networks lack sufficient training data to handle such variations robustly.

---

## Convolutional Neural Networks (CNNs)

### Definition

Convolution operates on two discrete signals. For input $x[t]$ and filter $w[t]$:

$$(x \star w)[t] = \sum_{\tau} x[t - \tau] w[\tau]$$

The key property is **equivariance with respect to shifts**: if the input shifts, the output shifts by the same amount. Limiting filter support ensures outputs depend only on local regions.

### Convolution Layers

Rather than full connectivity, convolution layers create sparse, local connections. Multiple input channels and output feature maps generalize the operation:

$$z_i = \sum_j x_j \star w_{ij}$$
$$h_i = \phi(z_i)$$

Learnable parameters scale as $O(\Delta^2 \times D \times F)$, where $\Delta$ is filter size, $D$ is input channels, and $F$ is output feature maps—independent of input dimensions.

**Practical considerations**:
- **Padding**: Zero-padding handles boundary conditions
- **Strides**: Subsampling by retaining every $n$th output
- **Pooling**: Max or average pooling reduces dimensionality while building robustness to small input shifts

### Training a CNN

Backpropagation works very well for training CNNs. While computation graphs differ due to sparse connectivity and weight sharing, the backpropagation approach translates directly to convolutional networks.

### Practical Deep Networks

**LeNet** (1998) established the foundational architecture. Subsequent advances included:

- **AlexNet** (2012): Deeper, more feature maps; catalyzed deep learning's resurgence
- **VGGNet** (2014): Introduced VGG blocks (serial combinations of convolution and max-pooling)
- **GoogleNet** (2014): All-convolutional architecture without fully connected layers
- **Residual Networks** (2015+): Enabled significantly deeper architectures

---

## Residual Networks

Rather than learning new features layer by layer, residual blocks model differences:

$$h_{l+1} = h_l + \sigma(W_l h_l)$$

Skip connections bypass weights through parallel paths. This architectural modification profoundly improves the optimization landscape, making residual networks the foundation of current state-of-the-art image recognition systems.
