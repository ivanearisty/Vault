# Lecture 6: Recurrent Neural Nets - Deep Learning

## Overview

This lecture introduces deep networks designed for processing sequential data, particularly text, transitioning from computer vision applications to natural language processing tasks.

---

## Recurrent Neural Networks

### Introduction to Text Processing Challenges

Standard feedforward networks struggle with text for two reasons:

1. **Representation Challenge**: Converting text characters/words into real-valued vectors requires careful encoding decisions beyond simple one-hot encoding.

2. **Dependency Challenge**: Text exhibits both short-range dependencies (word relationships) and long-range dependencies that convolutional networks cannot easily capture.

### Markov and N-gram Models

Before neural approaches, probabilistic language models dominated NLP. These estimate:

$$P(w) = P(w_1, w_2, \ldots, w_T)$$

Classical approaches factorize this using conditional probabilities. The first-order Markov assumption simplifies by assuming word likelihood depends only on the immediately preceding word, reducing probability estimations to $O(n^2)$ for a dictionary of $n$ words.

Extensions include bigram, trigram, and n-gram models, though computational costs increase dramatically with larger contexts.

### Recurrent Architectures

RNNs resolve time dependencies through latent "state" variables that encode historical information:

$$h^t = \sigma(Ux^t + Wh^{t-1})$$
$$y^t = \text{softmax}(Vh^t)$$

Key characteristics:
- Self-loops in hidden neurons create dependencies across time steps
- Weights $U$, $W$, $V$ remain constant across time steps (weight sharing)
- Networks can be "unrolled" to visualize information flow across timesteps

### Loss Functions and Metrics

**Training Loss**: Cross-entropy loss measures prediction accuracy:

$$l(y^t, g^t) = -\log y_{I(g)}^t$$

**Evaluation Metric**: Perplexity quantifies model prediction quality:

$$\text{Perplexity} = \exp\left(-\frac{1}{T} \sum \log y_{I(g)}^t\right)$$

Lower perplexity indicates better predictions. A value of 1 represents perfect certainty; a value equal to vocabulary size suggests random predictions.

### Backpropagation Through Time (BPTT)

Training uses standard gradient descent applied to unrolled networks. The critical challenge involves computing gradients with respect to recurrent weights:

$$\frac{\partial L}{\partial w} = \frac{1}{T} \sum \frac{\partial l^t}{\partial w}$$

Because $h^t$ depends on both $w$ and $h^{t-1}$, gradients involve multiplicative factors across timesteps, creating a geometric series effect:

- **Exploding gradients**: When multiplicative factors exceed 1 on average
- **Vanishing gradients**: When multiplicative factors fall below 1 on average

### Stabilizing RNN Training

**Gradient Clipping**: Normalizes gradient magnitude to prevent explosion:

$$g \leftarrow \alpha\frac{g}{\|g\|}$$

**Advanced Architectures**: Redesigning network structure to improve gradient propagation:

- **Gated Recurrent Units (GRU)**: Implements "reset" and "update" gates controlling memory retention
- **Long Short-Term Memory (LSTM)**: More sophisticated gating mechanisms
- **Bidirectional RNNs**: Process sequences in both directions

### GRU Mechanics

GRUs use two gating operations:

**Reset gate**: $r^t = \sigma(U_r x^t + W_r h^{t-1})$

**Update gate**: $z^t = \sigma(U_z x^t + W_z h^{t-1})$

Candidate state with reset:
$$\tilde{h}^t = \sigma(Ux^t + W(h^{t-1} \odot r^t))$$

Final state update:
$$h^t = h^{t-1} \odot z^t + \tilde{h}^t \odot (1 - z^t)$$

These mechanisms selectively retain or forget previous states, enabling learning of long-range dependencies.
