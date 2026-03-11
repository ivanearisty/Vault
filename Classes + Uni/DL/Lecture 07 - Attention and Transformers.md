# Lecture 7: Attention Mechanisms and the Transformer - Deep Learning

## Motivation

**Context on RNNs:** Recurrent neural networks excel at tasks like next-token prediction and sequence classification, but struggle with applications such as neural machine translation (NMT) and sentence generation due to alignment issues between source and target languages.

Example comparing English ("How do you like the weather today?") with German ("Wie finden sie das Wetter heute?") highlights how words may appear in different orders and quantities—a challenge called "misalignment."

**The Core Problem:** RNNs are fundamentally "sequence-to-symbol" models that output tokens sequentially. In translation, outputs are entire sequences where each token may depend on multiple input positions with long-range dependencies.

**Proposed Solutions:**
1. Model entire sentences as tokens (infeasible due to combinatorial explosion)
2. Use bidirectional RNNs (helps but doesn't capture long-range dependencies)
3. Employ encoder-decoder architectures (improves but retains gradient issues)
4. Store all intermediate encoder states as context vectors (complex and opaque)

---

## Self-Attention

**Basic Definition:** Given input points $\{x_1, x_2, \ldots, x_n\}$, produce outputs $\{y_1, y_2, \ldots, y_n\}$ where each output is a weighted average of all inputs:

$$y_i = \sum_j W_{ij} x_j$$

The weights are derived from inputs (not learned network parameters). Using dot-products:

$$w_{ij} = x_i^T x_j$$

Then apply softmax for row-normalization:

$$W_{ij} = \frac{\exp(w_{ij})}{\sum_j \exp(w_{ij})}$$

**Key Distinctions from Prior Architectures:**
- Maps sets of inputs to sets of outputs, capturing inter-data interactions
- Not limited to past tokens only
- Initially parameter-free (learned features upstream)
- Permutation-equivariant: reordering inputs produces reordered outputs

**Interpretation:** Self-attention automatically groups similar words, useful for handling redundancy in natural language and capturing subject-object or subject-predicate relationships.

---

## Generalized Self-Attention with Query, Key, Value

Each data point plays three roles:

1. **Query**: "What am I looking for?"
2. **Key**: "What information do I represent?"
3. **Value**: "What do I contribute to outputs?"

Formalized with learnable projection matrices $W_q$, $W_k$, $W_v$:

$$q_i = W_q x_i, \quad k_i = W_k x_i, \quad v_i = W_v x_i$$

$$w_{ij} = \frac{q_i^T k_j}{\sqrt{d}}$$

$$W_{ij} = \text{softmax}(w_{ij})$$

$$y_i = \sum_j W_{ij} v_j$$

The scaling factor $1/\sqrt{d}$ prevents dot-products from becoming excessively large.

---

## Multi-Head Self-Attention

Multiple self-attention mechanisms are concatenated (analogous to multiple filters in CNNs), indexed by $r = 1, 2, \ldots$:

$$q_{ri} = W_{rq} x_i, \quad k_{ri} = W_{rk} x_i, \quad v_{ri} = W_{rv} x_i$$

$$w_{rij} = \frac{\langle q_{ri}, k_{rj} \rangle}{\sqrt{d}}$$

$$W_{rij} = \text{softmax}(w_{rij})$$

$$y_{ri} = \sum_j W_{rij} v_j$$

$$y_i = W_y \text{concat}[y_i^1, y_i^2, \ldots]$$

The operation is denoted as:

$$[y_1, y_2, \ldots, y_n] = \text{Att}([x_1, x_2, \ldots, x_n])$$

**Nomenclature Origins:** Query, key, and value terminology derives from key-value database structures. The term "self-attention" contrasts with earlier attention mechanisms in encoder-decoder architectures, where separate networks computed alignment weights. The 2017 paper "Attention is All You Need" demonstrated that self-attention alone suffices for understanding context in NLP.

---

## Transformers

The Transformer architecture, now foundational for BERT and GPT models, uses self-attention as its core component.

**Transformer Block Structure:**
1. Self-attention + residual connection
2. Layer Normalization
3. Feedforward MLP layers
4. Layer Normalization

This architecture is entirely feedforward—no recurrent units—preventing gradient vanishing/explosion issues inherent to RNNs. Network depth is independent of input sequence length.

---

## Addressing Permutation Equivariance

Since self-attention is permutation-equivariant, sentences like "Jack gave water to Jill" and "Jill gave water to Jack" produce identical feature representations. Positional information requires explicit encoding.

### Positional Encoding Methods

**One-hot encoding:** Possible but becomes cumbersome with sequence length.

**Integer positions:** Suffers from scale/dynamic range problems.

**Sinusoidal Encoding (Standard Approach):**

$$p_t = [\sin(\omega_1 t); \sin(\omega_2 t); \ldots; \sin(\omega_d t)]$$

where $\omega_k = 1/10000^{k/d}$

This approach maintains bounded values and applies to any sequence length or dimension, leveraging periodicity to encode position information without learning additional parameters.
