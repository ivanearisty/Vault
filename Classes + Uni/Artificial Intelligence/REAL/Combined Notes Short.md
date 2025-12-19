# Lecture 7
Natural Language Processing + Tokenization (BPE) + Word2Vec + RNNs
## Part 3: Natural Language Processing Fundamentals
**1. Tokenization** Converting words into numbers. The simplest form treats each word as a token, but this isn't optimal (discussed in detail below).
**2. Part-of-Speech (POS) Tagging** Classifying words grammatically (noun, verb, adjective, etc.). Example: "London" → noun, "is" → verb
**3. Lemmatization** Reducing words to their base form (e.g., "running" → "run")
**4. Dependency Parsing** Creating a tree structure from text where:
- A root node is identified
- Words connect to other words showing relationships
- Example: "London is the capital" → "London" is the subject, "capital" is an attribute
**5. Named Entity Recognition (NER)** Identifying and classifying entities in text:
- Countries, dates, organizations, names
- Libraries like spaCy provide this in a single line of code
- Errors occur (e.g., "Dr. Maryland" misidentified as the state)
**6. Coreference Resolution** Linking pronouns/references to their antecedents across potentially long distances in text. Example: Connecting "it" back to "loan" mentioned sentences earlier. This capability is crucial for understanding context over long passages.

---

## Part 4: Tokenization — Byte Pair Encoding (BPE)
### Word-Level vs. Subword-Level Tokenization
**Word-level:** Each word becomes a token
- Problem: Vocabulary becomes extremely large
- Unknown words create issues
**Subword-level (BPE):** Most modern LLMs use this approach
- Originated from 1970s-80s compression research (floppy disk era)
- Goal: Minimize total bits needed to represent text

### Why Tokenizer Choice Matters
- Same text with GPT-2 tokenizer → 186 tokens
- Same text with GPT-4 tokenizer → fewer tokens (better compression)

**Implications:**
1. **Context window:** Better compression = longer effective context
2. **Performance:** GPT-2 was poor at coding because its tokenizer created many tokens for code; newer tokenizers include coding patterns
3. **Unknown tokens:** BPE greatly reduces unknown subwords compared to word-level tokenization
4. **Spelling mistakes:** Even misspelled words break into subwords, maintaining some meaning (though correction mechanisms are separate)

### Tokenizer-Model Pairing
The tokenizer and model must be matched. A vocabulary created for one model may not work well with another.

---

## Part 5: Word Embeddings — Word2Vec
### The Problem with One-Hot Encoding
With vocabulary size V (e.g., 100,000 words):
- One-hot encoding: Each word is a V-dimensional vector with a single 1

**Problem:** Words with similar meanings have zero dot product:
- `hotel` · `motel` = 0 (they're orthogonal)
- This doesn't capture semantic similarity
### The Goal
Create a vector space where **semantically similar words are close together**.

The professor demonstrated the TensorFlow Embedding Projector:

- 10,000 vocabulary entries mapped to 200-dimensional space
- Visualized in 3D using PCA
- Query "car" → nearest neighbors: "driver," "cars," "automobile," "race," "vehicle"

### Word2Vec Architecture
**Distributional Hypothesis (Firth, 1957):**

> "A word's meaning is given by words that frequently appear close by."

**Skip-gram Model Setup:**
- Given a center word W_t, predict nearby context words (window size C)
- Example: Window of ±2 words around the center

**Mathematical Formulation:**
The probability model:
$$P(\text{context} \mid \text{center word}, \theta) = \prod_{-m \le j \le m, j \neq 0} P(w_{t+j} \mid w_t, \theta)$$
**Optimization:** Maximum Likelihood Estimation
$$L(\theta) = \mathbb{E}_{w_t \sim P_{\text{data}}}[\log P_{\text{model}}(\text{context} \mid w_t, \theta)]$$
### Network Architecture
 ![[Screenshot 2025-12-19 at 3.05.08 AM.png | 300]]

**Key Components:**
1. **Input:** One-hot encoded center word (V-dimensional)
2. **Embedding matrix W:** V × D dimensions — projects to D-dimensional space
3. **Hidden representation Z:** The actual embedding (what we want!)
4. **Lifting matrix W':** D × V — lifts back to vocabulary space
5. **Softmax:** Produces probability distribution over all V words

**Training Process:**
- For each center word, predict multiple context words (4 branches if window = ±2)
- Joint optimization: Sum of cross-entropy losses for all context predictions
- Both W and W' matrices are trained simultaneously

**Final Output:** After training, freeze the **W matrix** (V × D). This is the embedding matrix that gets shared/uploaded for others to use.

### Context-Free Nature
These Word2Vec embeddings are **context-free**: the vector for "bank" is the same whether it appears near "river" or "money." Contextual embeddings (like BERT) came later with Transformers.

---
## Part 6: Language Models & Recurrent Neural Networks

### From Embeddings to Language Models
A **language model** predicts the next token given previous tokens:
$$P_{\text{model}}(w_t \mid w_{t-1}, w_{t-2}, \dots, w_{t-n}, \theta)$$
The context (previous tokens) can be very large in modern systems (100,000+ tokens).
### Why RNNs?
Need architectures that:
- Handle sequential data naturally
- Maintain a notion of **state** that evolves with each new input
- Process variable-length sequences

1. **Time series prediction** beyond NLP
2. **Connection to state space models** (Hidden Markov Models, Kalman filters)
3. **Competitive architectures** to transformers that are still being researched
### The State and Memory Concept
The RNN models a function F that depends on:
- Current input X(t) (the arrival of a token)
- Previous hidden state H(t-1)
Since H(t) depends on H(t-1), which depends on H(t-2), and so on, RNNs have **memory**—the question is how long this memory survives (which connects to gradient flow).
### From Kalman Filters to RNNs
The professor drew a parallel to the Kalman filter's state concept:
- State S_t (e.g., position, velocity of a vehicle)
- State depends on previous state S_{t-1} and actions A_t

For RNNs:
- State becomes **hidden state H_t**
- "Action" is the arrival of the next token X_t
- The function relating states doesn't change over time (unlike fully dynamic models)

![[Pasted image 20251219031040.png | 400]]
### The Recurrent Neuron
Sigmoidal Neuron
$$\text{output} = \sigma(\mathbf{W}^T \mathbf{X} + b)$$
Recurrent Neuron
$$\mathbf{H}_t = \tanh(\mathbf{U}^T \mathbf{X}_t + \mathbf{W} \mathbf{H}_{t-1} + b)$$


**Key differences:**
- Uses **tanh** instead of sigmoid (outputs range [-1, 1] instead of [0, 1])
- **U:** weights for current input
- **W:** weights for previous hidden state (the recurrence!)
- **H_{t-1}:** stored in memory, retrieved for each computation
**Trainable Parameters:** U (vector), W (scalar for single neuron), b (bias)

### Unrolling the RNN
A feedback loop can be "unrolled" into a feedforward-like structure:
- At time 1: Input X_1 → Hidden state H_1
- At time 2: Input X_2 + H_1 → Hidden state H_2
- ...
- At time 50: Input X_50 + H_49 → Final hidden state H_50 = ŷ

This reveals the RNN as a very deep network with **shared weights** across time steps.

**Sequential Processing:** H_2 needs H_1, H_3 needs H_2, etc. — inherently sequential (limitation addressed by Transformers later).

### Time Series Prediction Example
**Problem:** Given 50 commodity prices, predict the 51st.
**Challenge:** A single hidden state (scalar) cannot capture all factors affecting price:
- Macroeconomic indicators
- Company-specific factors (revenue, cash flow)
- Market sentiment
**Solution:** The hidden state must be a **vector**, not a scalar.

### From Single Neuron to RNN Layer
Just as we went from a single sigmoidal neuron to dense layers, we go from a single recurrent neuron to **RNN layers**:
1. **Make hidden state a vector H_t**
2. **Decouple hidden state dimensionality from output dimensionality**
    - Hidden state: Many dimensions (captures latent factors)
    - Output: May be 1 dimension (the predicted price)

**Hidden state dimensionality:** Must be chosen based on the complexity of latent factors in your problem

---
# Lecture 8
LSTM + Seq2Seq Translation + Beam Search + BLEU + Intro to Transformers
## 5. Long Short-Term Memory (LSTM) Architecture

The LSTM was developed to solve the gradient flow problem by introducing controlled gates and a **highway** for gradient propagation.
### Key Innovation: Two Hidden States
1. **S(t)**: Long-term memory state (lives on the highway)
2. **H(t)**: Short-term memory state
### The Gate Mechanism
LSTMs introduce three gates, all using **sigmoidal activation** (output between 0 and 1):
**1. Input Gate:**
- Controls what new information enters the cell
- Multiplies incoming information with [0,1] values
- Softly selects what to keep vs. discard from new inputs
**2. Forget Gate:**
- Controls what to remember/forget from long-term memory
- Modulates the S pathway (the only thing affecting the highway)
- Determines how much of S(t-1) to propagate
**3. Output Gate:**
- Controls how much of H(t) propagates to the next step
- Determines what's revealed to the rest of the network

### The Highway Concept
The S(t) state travels on a "highway"—a direct path with minimal interference:
- Only the forget gate modulates it
- Gradient can flow through this highway relatively unimpeded
- This is similar to **skip connections in ResNets** (another highway network)

![[Pasted image 20251219031705.png | 300]]

---

## 6. Language Modeling Task with RNNs

The simple RNN language model architecture:
1. **Input**: Token W(t-n) (from context)
2. **Embedding Layer**: Maps to X(t-n)
3. **RNN/LSTM Layer**: Processes with hidden state from previous step
4. **Classification Head**: Produces Y_hat (posterior over vocabulary V)

This unrolled architecture produces predictions at each step, allowing us to train by comparing Y_hat with ground truth tokens.

**Character-level RNN:** The professor referenced Andrei Karpathy's famous code (from his Stanford days) that demonstrates character-level language modeling—starting from garbage predictions and gradually learning to predict the next character correctly.

---

## 7. Neural Machine Translation (Sequence-to-Sequence)

![[Pasted image 20251219031905.png | 500]]

### Encoder-Decoder Architecture
Neural machine translation exemplifies **sequence-to-sequence** models with two components:

**Encoder:**
- Processes source language tokens (X1, X2, X3)
- Produces a final hidden state H3 called the **thought vector (φ)**
- The thought vector captures the entire meaning of the input sequence

**Decoder:**
- Receives the thought vector as initial state
- Generates target language tokens (X'1, X'2, X'3) one at a time
- Uses special tokens: **SOS (Start of Sentence)** and **EOS (End of Sentence)**

### Teacher Forcing
**During Training:**
- Feed the **ground truth** tokens to decoder inputs
- We have supervision at every step
**During Inference:**
- Feed the **previous predicted token** to the next step
- No ground truth available; blue arrows connect predictions to next inputs

### The Reverse Order Trick
Tokens are often fed in **reverse order** (X3, X2, X1) to the encoder. Why?
**Grammar alignment:** Many languages follow Subject-Verb-Object order. The first word of the source is often the first word of the target. By feeding the first word last:
- It's the most "recent" information when forming the thought vector
- Gradient flow to this information is strongest
- Better footing for generating the first output word

Bidirectional RNNs compute:
- **φ_f**: Thought vector from forward direction
- **φ_r**: Thought vector from reverse direction
- **φ**: Concatenation of both, capturing both preceding and following context

### Limitation of Single Thought Vector
The entire decoding process depends on **one vector** (φ). This was a major limitation that attention mechanisms (covered next) would address.

---
## 8. Beam Search (Maximum Likelihood Sequence Estimation)

### Beam Search Solution
Instead of tracking only the best prediction:
- Maintain **log probabilities** of **K branches** (K is a hyperparameter)
- Track multiple candidate sequences simultaneously
- At the end (EOS token), select the sequence with highest total likelihood
**Tradeoff:** Beam search is computationally expensive—branches grow quickly. K is typically kept small (e.g., 2-5).
This is also known as:
- **MLSE** (Maximum Likelihood Sequence Estimation)
- **Viterbi algorithm** (named after the inventor)

---
## 9. Evaluation: BLEU Metric
Compare machine translation (C) with human translations:
**Precision-based formula:** True Positives / (True Positives + False Positives)

---
## 10. Introduction to Transformers
![[Pasted image 20251219032300.png]]
### 1. Eliminating Recurrent Connections
No more H(t) depending on H(t-1):
- Serial architecture → **Parallel architecture**
- Enables processing all tokens simultaneously
- Dramatic speedup in training and inference
### 2. Positional Encoding
Since transformers process all tokens in parallel:
- They are **permutation invariant** by default
- Must explicitly inject position information
- Position encodings tell the model where each token is in the sequence
### 3. Contextual Embeddings via Attention
The most important innovation: **Attention mechanism** creates context-dependent embeddings.
**The Problem:** In Word2Vec, "bears" has ONE embedding regardless of context:
- "I love bears" (animal)
- "He bears the pain" (tolerance/endurance)
- "Bears won the game" (sports team - Chicago Bears)

**The Solution:** Attention mechanism pushes tokens to **different positions** in D-dimensional space based on surrounding context.

### Simple Attention Mechanism (Preview)
![[Pasted image 20251219032409.png | 500]]
Given tokens arranged in matrix X (D × T, where T = context size):
**Step 1: Compute Score Matrix** S = X · X^T (T × T matrix)
This dot product measures how similar/related each pair of tokens is.
**Step 2: Apply Softmax (row-wise)** A = softmax(S)
Creates **attention weights** with properties:
- A_ij ≥ 0
- Sum over j of A_ij = 1 (each row sums to 1)
**Step 3: Create Contextual Embeddings** X_hat = A · X
Or equivalently: X_hat = softmax(X · X^T) · X

**What attention accomplishes:**
- Started at one location in D-dimensional space (context-free embedding)
- Ended at **another location** in D-dimensional space (contextual embedding)
- The attention mechanism **pushed** the token based on weights from other tokens

**Current limitation of simple attention:** No learnable parameters—the mapping is deterministic based on context-free embeddings. 

---
# Lecture 9 
Transformer Self-Attention (QKV, Masking, Multi-Head, LayerNorm/MLP blocks, Positional Embeddings, MoE)
## Part 2: Transformer Self-Attention Mechanism
![[Pasted image 20251219032602.png]]
## Part 3: Query, Key, Value (Q, K, V) Parameterization
### Intuition Through Grammar Roles
Think of each token as needing to communicate three pieces of information:
1. **Key (K)**: "What am I?" — Conveys the token's identity/role (e.g., "I am an object," "I am a verb")
2. **Query (Q)**: "What am I looking for?" — What kind of token would help me understand my meaning (e.g., "I'm looking for a verb")
3. **Value (V)**: "Where do I start?" — The starting point for the transformation, which may already have some notion of meaning
### Linear Projections for Q, K, V
Each is computed via separate trainable matrices:
- **Q = X · W_Q** (Query matrix)
- **K = X · W_K** (Key matrix)
- **V = X · W_V** (Value matrix)
All three matrices have dimensions T × D. The W matrices are learned during training.
### Why Scale by √D?
Without scaling, softmax outputs become very sparse (only 1-2 tokens attend to each other). With scaling:
- The attention distribution is smoother
- Many tokens can contribute to positioning
**Analytical justification**: If inputs have variance 1, the dot product increases variance by factor D. Dividing by √D restores variance to 1.

---
## Part 4: Masking for Decoder Architectures

In **decoder-only** (autoregressive) models:
- During training: All tokens are available
- During inference: Only past tokens are available

To avoid "cheating" during training, we implement **masking**:
- When at position 8, only receive attention from positions 1-7
- When at position 7, only from positions 1-6, etc.

**Implementation**: Set attention weights for future tokens to 0 (or equivalently, set the input to softmax to a very large negative number like -∞, which softmax converts to 0).
This is why decoder attention is called **Masked Self-Attention**.

---

## Part 5: Single-Head to Multi-Head Self-Attention
![[Pasted image 20251219032927.png]]
### Why Multiple Heads?
Just as CNNs need multiple filters to extract different spatial patterns, transformers need multiple attention heads to extract different **temporal/sequential correlation patterns**.
Each head might focus on different grammatical or semantic relationships (though the network learns these without explicit labels).
### Multi-Head Self-Attention (MHSA)
- Run H single-head attention mechanisms **in parallel**
- Each head h has its own projection matrices: W_Q^(h), W_K^(h), W_V^(h)
- **Concatenate** all head outputs
- Pass through a **mixing matrix** (D × D) to produce final V̂ (T × D)
The number of heads H varies by architecture (early transformers used 8; modern ones may use more).
**Key Properties**:
1. Heads work in parallel to extract representations across different patterns
2. Final output is a linear combination of all single-head outputs
3. Typical architectures have 8-32+ heads

---

## Part 6: Layer Normalization and the Complete Transformer Block
![[Pasted image 20251219033040.png]]
### Block A: Multi-Head Self-Attention with Residuals

The MHSA block is decorated with:
1. **Layer Normalization** at the input
2. **Skip connection** (residual connection) around the MHSA

```
X → LayerNorm → MHSA ─┬→ Z
        ↑              │
        └──────────────┘ (skip connection)
```

**Key insight from backpropagation**: The gradient of a neuron's output with respect to weights W is **proportional to the inputs X**. This means:
- If inputs have very small values → small gradients → slow learning
- If inputs have very different scales → uneven learning

**Batch normalization** standardizes activations across a batch:
1. Compute mean μ and variance σ² across the batch dimension
2. Standardize: X̂ = (X - μ) / √(σ² + ε)
3. Scale and shift with **trainable parameters** γ and β: Output = γ · X̂ + β
The γ and β let the network learn the optimal distribution for each layer.

### Layer Normalization
Problem with batch normalization in transformers:
- Context sizes are huge → limited batch sizes due to VRAM constraints
- During inference, requests may come one at a time
- Can't extract reliable statistics from tiny batches

**Solution**: Layer normalization computes mean and standard deviation **across the feature dimension** (D) rather than the batch dimension.
This works even with batch size = 1.
### Block B: MLP with Nonlinearity
![[Pasted image 20251219033219.png | 300]]
Up to this point, all operations were **linear**. We need nonlinearity.

Block B:
1. **Layer Normalization** on Z
2. Pass through **MLP** (Multi-Layer Perceptron) with nonlinear activation (like GELU or ReLU)
3. **Skip connection** around the MLP

```
Z → LayerNorm → MLP ─┬→ V̂
        ↑            │
        └────────────┘ (skip connection)
```

### Complete Transformer Layer
One **Masked Transformer Layer (MTL)** = Block A + Block B in series.
A complete transformer **body** stacks multiple MTLs:
- MTL₁ → MTL₂ → ... → MTL_L
- Typical architectures use ~32 layers
### The Transformer Head
After the body builds representations, we attach a **head** for the specific task:
1. Linear layer with matrix W
2. **Softmax** across vocabulary size
This produces:
- **ŷ**: Posterior probability distribution over all tokens in vocabulary
- Select next token via greedy decoding or beam search

---
## Part 7: Positional Embeddings
### The Problem
Without positional information, transformers are **permutation invariant**—shuffling input tokens gives the same output. Word order matters in language!
### Solution: Add Positional Encodings
For token i with context-free embedding X_i:
```
X̃_i = X_i + R_i
```

Where R_i is the **positional encoding vector** for position i.
### Why Addition Instead of Concatenation?
1. Concatenation would increase dimension D (more computation)
2. Addition works if R_i doesn't drastically disrupt X_i (values bounded between -1 and 1)
3. Linear operations preserve the sum: W(X + R) = WX + WR
### The Fourier Method (Sinusoidal Positional Encoding)
The formula uses sinusoids of decreasing frequency:
- Element n of position i's encoding:
    - If n is even: sin(i / L^(n/D))
    - If n is odd: cos(i / L^((n-1)/D))
Where L relates to context size T.
**Advantage**: Values stay bounded in [-1, 1], providing smooth gradations rather than discrete jumps.
### Alternative: Learnable Positional Embeddings
Instead of fixed sinusoids, learn the positional vectors as parameters. Both approaches work; the Fourier method has the advantage of potentially generalizing to longer sequences than seen during training.

---
## Part 8: Mixture of Experts (MoE)
### Connection to Ensemble Methods
In residual networks (ResNets), the class studied **ensemble methods** with weak predictors that, in aggregate, produce predictions much better than individual ones.
**Best case**: Each ensemble member makes **uncorrelated mistakes** **Worst case**: All members make the **same mistake**
### What is Mixture of Experts?
MoE is a **conditional ensemble** where different "experts" (specialized sub-networks) handle different parts of the data distribution.
When combining K experts/predictors, the mean squared error of the ensemble depends on:
- **V**: the variance of individual predictors
- **C**: the correlation of their mistakes
**Best case (C = 0)**: When experts make **uncorrelated mistakes**, the ensemble error is V/K – error decreases linearly with the number of experts
**Worst case (C = V)**: When experts make the same mistakes (perfectly correlated), having K experts is no better than having 1
### Comparison with Standard Ensembles
- **Standard ensemble**: All weak predictors try to model the entire P_data distribution
- **Mixture of Experts**: Each expert specializes in a **partition** of P_data
### Connection to Mixture of Gaussians
A mixture of Gaussians models complex distributions as:
**Key difference from ensembles**: The gating network G is **trainable**—it learns which experts should handle which inputs.
### Why MoE Matters Now
MoE provides significant efficiency benefits:
- Not all experts need to be activated for every input (sparse activation)
- Reduces memory requirements on expensive accelerators
- Popularized by **DeepSeek** and other recent architectures
```
ŷ = Σᵢ Gᵢ(x) · Fᵢ(x)
where Σᵢ Gᵢ(x) = 1, Gᵢ(x) ≥ 0
```

# Lecture 10
Vision Transformers + Propositional Logic + Wumpus World + KB Ops + Model Checking
## Part 4: Vision Transformers (ViTs)
### Tokenization in Vision
Unlike NLP where tokens are words/subwords, in ViTs:
- Images are divided into a **grid of patches** (typically 16×16 pixels per patch)
- Each patch becomes a token
- For a 224×224 image with 16×16 patches, you get 196 tokens
**Interesting finding**: Whether you arrange the patch tokens in a specific spatial order or randomly shuffle them makes little difference to performance – the positional embeddings handle the spatial information.
### Attention Visualization in ViTs
The attention matrix A can be visualized to understand what each query attends to:
- White = high attention (close to 1)
- Black = low attention (close to 0)
## Part 6: Propositional Logic Fundamentals
### Syntax vs. Semantics
**Syntax**: How to represent logical sentences
- Propositional symbols (evaluate to true/false)
- Operators: negation (¬), conjunction (∧), disjunction (∨), implication (→), biconditional/double implication (↔)
**Semantics**: How to determine if a sentence is true
- Based on **models** (assignments of truth values to all symbols)
- A model **satisfies** a sentence when it evaluates to true
### Models
A **model** is a specific assignment of truth values to propositional symbols.
**Example**:

- Symbols: R (rain), W (wet)
- One model: {R = true, W = true}
- This model satisfies the sentence "R → W" (if it rains, it's wet)

**Multiple models**: A sentence can be satisfied by multiple models. The set of all models satisfying a sentence is denoted M(sentence).
## Part 7: The Wumpus World – A Logical Reasoning Demonstration

### The Game Setup
A classic AI problem from the 1970s demonstrating logical reasoning:
**Environment**: 4×4 grid world containing:
- **Agent**: Starts at (1,1), can move, turn, and sense
- **Wumpus (monster)**: Located in one cell, is smelly
- **Pits**: Fatal if entered, cause breeze in adjacent cells
- **Gold**: The goal – grab it and exit
**Perception**:
- **Stench (S)**: Sensed in cells adjacent to the Wumpus
- **Breeze (B)**: Sensed in cells adjacent to pits
- **Glitter**: Sensed when gold is in the current cell

**Constraint**: The agent is conservative – it cannot move unless it is **certain** the next cell is safe.
### Building the Knowledge Base
The Knowledge Base (KB) stores rules and inferences as logical sentences:
**Initial Rules (before any movement):**
- R0: ¬W₁₁ (no Wumpus at start)
- R1: ¬P₁₁ (no pit at start)
- R2: B₁₁ ↔ (P₁₂ ∨ P₂₁) (breeze at 1,1 iff pit at adjacent cell)
- R3: B₂₁ ↔ (P₁₁ ∨ P₂₂ ∨ P₃₁) (similar rule for cell 2,1)
- Similar rules for stench and other cells...

**Key insight**: Rules act as **constraints**. More rules in the KB make it harder to find satisfying models, but they enable more precise inference.

---
## Part 8: Knowledge Base Operations
### The Two Operations: Tell and Ask
**Tell(KB, sentence)**: Store a sentence in the knowledge base
Three possible responses:
1. **"I knew that"** (Entailment) – The sentence was already implied by KB
2. **"I didn't know that, updating"** (Contingency) – New information added
3. **"I don't believe that"** (Contradiction) – Sentence conflicts with KB
**Ask(KB, sentence)**: Query the knowledge base
Three possible responses:
4. **True** – KB entails the sentence
5. **False** – KB entails the negation
6. **"I don't know"** – KB entails neither the sentence nor its negation
## Part 9: Introduction to Model Checking
### The Problem
Given a knowledge base KB at some state (e.g., timestamp t=4 in Wumpus World), determine the response to a query like "¬P₁₂" (there is no pit at location 1,2).
### Model Checking Algorithm
**Step 1**: List all propositional symbols involved in the rules
- Example symbols: B₁₁, B₂₁, P₁₂, P₂₁, P₂₂, P₃₁ (7 symbols)
**Step 2**: Enumerate all possible models
- With 7 binary symbols: 2⁷ = 128 possible models
- Each row represents a different truth assignment
**Step 3**: For each model, evaluate whether it satisfies the KB
- The KB is satisfied when ALL rules (R1 ∧ R2 ∧ R3...) evaluate to true
- Rules are conjunctions – each adds a constraint
**Step 4**: Among models that satisfy the KB, check the query
- If ¬P₁₂ is true in all satisfying models → Response is **True**
- If ¬P₁₂ is false in all satisfying models → Response is **False**
- If mixed → Response is **Unknown**
# Lecture 11
PDDL + MDPs (definitions, grid world, value functions, Bellman, prediction vs control)
## PDDL: Planning Domain Definition Language
PDDL is a **domain-specific language (DSL)** for specifying planning problems.
### Two Required Files:
1. **Domain File** (domain.pddl) — Specifies:
    - Types (classes)
    - Predicates (state descriptions)
    - Action schemas (operators with preconditions and effects)
2. **Problem File** (problem.pddl) — Specifies:
    - Objects (instances of types)
    - Initial state
    - Goal state

## Markov Decision Processes (MDPs)
![[Pasted image 20251219034403.png]]

![[Screenshot 2025-12-19 at 3.46.31 AM.png]]

## Formal MDP Definition
An MDP is defined as a 5-tuple: **M = (S, P, R, A, γ)**

|Component|Description|
|---|---|
|**S** (script)|Set of all possible states|
|**P**|Transition model (probability of moving between states)|
|**R**|Reward function|
|**A**|Set of all possible actions|
|**γ** (gamma)|Discount factor (0 ≤ γ ≤ 1)|

## MDP Dynamics
The **MDP dynamics** is a probability distribution:
$$P(S', R | S, A)$$

This represents: "Given I'm in state S and take action A, what's the probability of ending up in state S' and receiving reward R?"
### Deriving Component Models:
**Transition Model** (from marginalization): $$P(S' | S, A) = \sum_R P(S', R | S, A)$$
**Reward Model**: $$P(R | S, A) = \sum_{S'} P(S', R | S, A)$$
## The Grid World Example
**Environment characteristics**:
- Grid of cells (states)
- Agent can move: Up, Down, Left, Right
- **Stochastic transitions**:
    - 80% probability: Move in intended direction
    - 10% probability: Move left of intended direction
    - 10% probability: Move right of intended direction
- Hitting a wall means staying in current state
- Two **terminal states**: +1 (goal) and -1 (pit)

**Why negative rewards for non-terminal states?**
If all states had even slightly positive rewards, the agent would wander forever collecting rewards instead of reaching the goal. Small negative rewards (e.g., -0.04) motivate the agent to reach the terminal state efficiently.

---
## Transition Model as a Table/Tensor
For each action, we need a table showing:
```
P(S' | S, A=action)
```

**Structure**:
- Rows: Current states (S)
- Columns: Next states (S')
- Values: Transition probabilities

Since there are 4 actions, we have 4 such tables — forming a 3D tensor.

**Example** for A=Up in state S₁₁:
- P(S₁₂ | S₁₁, Up) = 0.8 (moved up successfully)
- P(S₁₁ | S₁₁, Up) = 0.1 (hit wall, stayed)
- P(other | S₁₁, Up) = 0.1 (moved sideways)

---
## Reward Function
### Two-Parameter Version:
$$R(s, a) = \mathbb{E}[R_t | S_{t-1}=s, A_{t-1}=a]$$
"Expected reward received after executing action a from state s"

**Expansion**: $$R(s, a) = \sum_r \sum_{s'} r \cdot P(s', r | s, a)$$
### Three-Parameter Version:

$$R(s, a, s') = \mathbb{E}[R_t | S_{t-1}=s, A_{t-1}=a, S_t=s']$$
This version includes dependency on which state we end up in.

---
## Return (Cumulative Discounted Reward)
The **return** Gₜ is what the agent tries to maximize:
$$G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + ... = \sum_{k=0}^{\infty} \gamma^k R_{t+1+k}$$
### The Discount Factor γ:

| γ value | Agent behavior                                 |
| ------- | ---------------------------------------------- |
| γ ≈ 0   | Myopic — focuses on immediate rewards only     |
| γ ≈ 1   | Far-sighted — considers long-term consequences |

**Intuition**: Like net present value of money — "$1 million today is worth more than $1 million next year." Future rewards are uncertain, so we discount them.
### Why Discounting?
1. Mathematical convenience (ensures finite sums)
2. Reflects uncertainty about the future
3. Models preference for immediate rewards
4. Avoids infinite returns in continuing tasks
---
## Policy
A **policy** π is a mapping from states to actions, expressed as a probability distribution:
$$\pi(a|s) = P(A_t = a | S_t = s)$$
### Types of Policies:
**Stochastic policy**: Probability distribution over actions
- Example: π(up|s) = 0.25, π(down|s) = 0.25, π(left|s) = 0.25, π(right|s) = 0.25
**Deterministic policy**: Always takes the same action in a given state
- P(aᵢ|s) = 1 for some specific action aᵢ
**Important note**: A stochastic policy does NOT make the MDP stochastic. The stochasticity of the MDP comes from the transition model itself.

---
## Value Functions
### State-Value Function Vπ(s)
"The expected return starting from state s and following policy π thereafter"
$$V^\pi(s) = \mathbb{E}_\pi[G_t | S_t = s]$$
**Expanded**: $$V^\pi(s) = \mathbb{E}_\pi[R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + ... | S_t = s]$$
**Interpretation**: For every state in the environment, we assign a number (value) representing how good it is to be in that state under policy π.
**Example**: In a room where the goal is to exit through a door:
- States near the door have high value
- States far from the door have lower value

---
## The Bellman Expectation Equation
The fundamental recursive relationship for value functions:
$$V^\pi(s) = \mathbb{E}_\pi[R_{t+1} + \gamma V^\pi(S_{t+1}) | S_t = s]$$
**In words**: The value of a state equals the expected immediate reward plus the discounted value of the next state.
### Backup Tree Visualization:

```
           s (current state)
           │
    ┌──────┼──────┐
    │      │      │
   a₁     a₂     a₃    ← Actions (chosen by policy π)
    │      │      │
   ─┼─    ─┼─    ─┼─   ← State transitions (governed by P(s'|s,a))
  ╱ │ ╲  ╱ │ ╲  ╱ │ ╲
 s' s' s' s' s' s' s'  ← Possible next states
```
**Key insight**: The Bellman equation connects the value of the current state to the values of possible successor states.

---
## Prediction vs. Control
### Prediction Problem:
Given a policy π, evaluate V^π(s) for all states.
"What is the value of each state if I follow this policy?"
### Control Problem:
Find the optimal policy π* that maximizes value for all states.
"What is the best policy to follow?"
**Process**:
1. Start with some policy
2. Evaluate its value function (prediction)
3. Improve the policy based on the value function (control)
4. Repeat until convergence to optimal policy π*

# Lecture 12
MDPs (value/Q, Bellman expectation/optimality, policy iteration, MC, TD, TD(λ), value iteration example)
## 1. Markov Decision Processes (MDPs) - Foundations
**The MDP Tuple**: An MDP is defined by five components:
$$\mathcal{M} = (\mathcal{S}, \mathcal{P}, \mathcal{R}, \mathcal{A}, \gamma)$$
Where:
- **𝒮 (State Space)**: The set of all possible states the environment can be in
- **𝒫 (Transition Model)**: Probability of transitioning between states given actions
- **ℛ (Reward Function)**: The set of rewards or reward function
- **𝒜 (Action Space)**: The set of all possible actions the agent can take
- **γ (Discount Factor)**: A value between 0 and 1 that discounts future rewards
### 1.3 The Transition Model
The transition model P(s'|s, a) describes the probability of ending up in state s' given that you were in state s and took action a.
**Key Properties**:
- The environment is **stochastic** - same action from same state may lead to different outcomes
- The Markov property: The future depends only on the current state, not the history
**Example (Grid World)**: If you're in a grid cell and choose to go "up":
- With probability 0.8, you actually go up
- With probability 0.1, you slip left
- With probability 0.1, you slip right
This is captured in transition probability tables - one for each action.
### 1.4 The Reward Function
The reward function can take two forms:
**Two-parameter version**: R(s, a)
- Expected reward received after executing action a from state s
**Three-parameter version**: R(s, a, s')
- Expected reward received after executing action a from state s and landing in state s'
**Mathematical Definition**: $$R(s, a) = \mathbb{E}[R_t | S_t = s, A_t = a]$$
### 1.5 The Policy
A **policy π** is the agent's strategy - it defines which action to take in each state.
**Stochastic Policy**: π(a|s) = P(A_t = a | S_t = s)
- Gives a probability distribution over actions for each state
**Deterministic Policy**: Directly maps states to actions
- π(s) = a
**Important Clarification from Lecture**: A stochastic policy does NOT make the MDP stochastic. The stochasticity of the MDP comes from the transition model itself.
### 1.6 Episodes and Trajectories
**Episode**: A complete interaction sequence from start to termination
- From t = 0 to t = T-1
**Trajectory (τ)**: The sequence of experiences over an episode $$\tau = (S_0, A_0, R_1, S_1, A_1, R_2, ..., S_T)$$
**Termination** occurs when:
- Agent reaches a terminal state
- Agent decides to terminate
- Running out of time (finite horizon problems)
### 1.7 The Return
The **return G_t** is the cumulative discounted reward from time t onwards:

$$G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + ... = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$$
**Why discount?**
- Mathematical convenience (ensures finite returns for infinite horizons)
- Represents uncertainty about the future
- Models preference for immediate rewards
- Common values: γ = 0.9 or γ = 0.99
---
## 2. Value Functions
Value functions are the **objective functions** we optimize in MDPs. They estimate "how good" it is to be in a given state or to take a given action.
### 2.1 State Value Function V(s)
The **state value function** V^π(s) answers: "What is the expected return starting from state s and following policy π?"
$$V^\pi(s) = \mathbb{E}_\pi[G_t | S_t = s]$$
**Expanded form**: $$V^\pi(s) = \mathbb{E}_\pi[R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + ... | S_t = s]$$

**Intuition**: Think of a grid world where you need to exit a room. States closer to the door have higher value because you expect to receive rewards sooner.
**Visual Representation**: In a grid world, the value function is a matrix where every state/cell has a value number.

### 2.2 State-Action Value Function Q(s, a)
The **state-action value function** (Q-function) Q^π(s, a) answers: "What is the expected return starting from state s, taking action a, and then following policy π?"

$$Q^\pi(s, a) = \mathbb{E}_\pi[G_t | S_t = s, A_t = a]$$
**Why Q is "better" than V for acting optimally**: The Q function directly tells us the value of each action, making it easier to select optimal actions. With V alone, you need to know the transition model to compare action values.

---
## 3. Bellman Equations
The Bellman equations are fundamental recursive relationships that connect the value of a state to the values of successor states. 
### 3.1 Bellman Expectation Equation for V
This equation connects the value of the current state to immediate reward plus discounted value of next states:
$$V^\pi(s) = \mathbb{E}_\pi[R_{t+1} + \gamma V^\pi(S_{t+1}) | S_t = s]$$
**Expanded form with explicit sums**: $$V^\pi(s) = \sum_a \pi(a|s) \sum_{s'} P(s'|s,a)[R(s,a,s') + \gamma V^\pi(s')]$$
**Interpretation**: The value of a state equals the expected immediate reward plus the discounted expected value of the next state.
### 3.2 The Backup Tree Concept
The professor emphasized understanding backup trees:
```
        (s)           ← Starting state
       / | \
      a1 a2 a3        ← Possible actions (determined by π)
     /|\ |  |\ 
   s' s'' s'''        ← Possible next states (determined by P)
```
- **Solid lines** (from state to action): Determined by policy π(a|s)
- **Dashed lines** (from action to next state): Determined by transition model P(s'|s,a)
- Each next state has its own value V(s')
- We "back up" values from successor states to compute current state value
### 3.3 Bellman Expectation Equation for Q
$$Q^\pi(s,a) = \mathbb{E}_\pi[R_{t+1} + \gamma Q^\pi(S_{t+1}, A_{t+1}) | S_t = s, A_t = a]$$
**Expanded**: $$Q^\pi(s,a) = \sum_{s'} P(s'|s,a)[R(s,a,s') + \gamma \sum_{a'} \pi(a'|s') Q^\pi(s',a')]$$
### 3.4 Bellman Optimality Equations (Nonlinear!)
When we want to find the **optimal** value function (V* or Q*), we introduce the **max** operator, which makes the equations **nonlinear**.
**For V***:
$$
V^*(s) = \max_{a} \sum_{s'} P(s' \mid s,a) \bigl[ R(s,a,s') + \gamma V^*(s') \bigr]
$$
**For Q***: 
$$
Q^*(s,a) = \sum_{s'} P(s' \mid s,a) \bigl[ R(s,a,s') + \gamma \max_{a'} Q^*(s',a') \bigr]
$$
**Why nonlinear?** The max operator is nonlinear - we can't solve these with simple matrix inversion like the expectation equations. We need iterative methods like **policy iteration** or **value iteration**.
### 3.5 Solving Bellman Equations
**Linear Case (Bellman Expectation)**:
- Can be written as: **V = R + γPV**
- Rearranges to: **(I - γP)V = R**
- Solution: **V = (I - γP)^(-1) R**

**However**, the professor strongly warned against matrix inversion in practice:
> "Every time you see matrix inversion, don't implement it - it will more likely blow up."
Instead, use **iterative methods** based on the fact that the Bellman operator is a **contraction**.
### 3.6 The Bellman Operator as a Contraction
A **contraction** mapping has the property that applying it repeatedly converges to a fixed point, regardless of starting point.
**Simple scalar example**: $$x_{k+1} = \gamma x_k + c$$
For γ < 1, this converges to x* = c/(1-γ)
**The Bellman operator is a contraction** with factor γ: $$V_{k+1}(s) = \sum_a \pi(a|s) \sum_{s'} P(s'|s,a)[R(s,a,s') + \gamma V_k(s')]$$
This iterative method is called **Iterative Policy Evaluation**.

---
## 4. Prediction vs Control Problems

### 4.1 Prediction Problem (Policy Evaluation)
**Goal**: Given a policy π, calculate V^π(s) for all states
**Question**: "How good is this policy?"
**Method**: Use Bellman Expectation equations iteratively
**Example**: Given a uniform random policy (equal probability for all actions), what is the value of each state?
### 4.2 Control Problem
**Goal**: Find the optimal policy π* that maximizes value
**Question**: "What is the best way to act?"
**Involves two sub-problems**:
1. Policy Evaluation: Evaluate current policy
2. Policy Improvement: Use greedy action selection to improve policy
**Methods**: Policy Iteration, Value Iteration

---
## 5. Policy Iteration
Policy Iteration is the key algorithm for solving MDPs (finding π*). It alternates between two steps until convergence.
### 5.1 The Algorithm
```
1. Initialize π arbitrarily (e.g., uniform random)
2. Repeat until convergence:
   a. Policy Evaluation: Compute V^π for current policy
   b. Policy Improvement: For each state, update policy to be greedy w.r.t. V^π
      π'(s) = argmax_a Σ_{s'} P(s'|s,a)[R(s,a,s') + γV^π(s')]
3. Return π* (optimal policy)
```
### 5.2 Policy Evaluation (Step 2a)
Use iterative policy evaluation:

$$V_{k+1}(s) = \sum_a \pi(a|s) \sum_{s'} P(s'|s,a)[R(s,a,s') + \gamma V_k(s')]$$
Repeat until V converges (change is below threshold).
### 5.3 Policy Improvement (Step 2b)
**Greedy Policy Improvement**: For each state, select the action that maximizes expected value:

$$\pi'(s) = \arg\max_a \sum_{s'} P(s'|s,a)[R(s,a,s') + \gamma V^\pi(s')]$$
This **eliminates suboptimal actions** from the policy.
### 5.4 Convergence Guarantee
> "The optimal policy may converge much sooner than the value function. Despite the fact that the value function may change from one iteration to another, the relative benefits or relative values between adjacent states that drive the decision-making do not change, and therefore the optimal policy may have converged much sooner."
### 5.5 Grid World Example
**Setup**: 4x4 grid, two terminal states (corners), reward of -1 for each step
**Iteration 0**:
- Initialize V = 0 for all states
- Policy = uniform random (0.25 probability for each direction)
**Iteration 1**:
- Evaluate policy → Get new V values
- Act greedily → Arrows now point toward terminal states
**Iteration 2+**:
- Continue until policy no longer changes
- Final result: Optimal arrows in each cell pointing toward shortest path to terminal
---
### 6. RL Fundamentals: Model-Free Learning
Unlike MDPs where $P(s'|s,a)$ and $R(s,a,s')$ are **known**, RL assumes the environment is a **black box**. We must learn $V$ and $Q$ purely through interaction (experience).
**The Backup Tree Distinction**
- **Dynamic Programming (MDP):** **Breadth-first**. Full backup. We know every possible next state and reward.
- **RL / Monte Carlo:** **Depth-first**. Sample backup. We take _one_ action and observe _one_ outcome (trajectory).
### 7. Monte Carlo (MC) Methods
MC learns from **complete episodes**. You must reach a terminal state to calculate the return ($G_t$) and update the value.
- Update Rule: Incremental mean (similar to Kalman Filter).
$$V(S_t) \leftarrow V(S_t) + \alpha [\underbrace{G_t}_{\text{Target}} - V(S_t)]$$
- **Pros:** Unbiased; simple; no model required.
- **Cons:** **High Variance**; must wait for episode end.
- **Analogy:** "Waiting until the car crashes to learn that 100mph in a corner is bad."
### 8. Temporal Difference (TD) Learning
TD methods **bootstrap**: they update estimates based on other estimates, without waiting for the episode to end.
8.1 TD(0) (One-Step)
- **Core Insight:** Combines **Sampling** (from MC) with **Bootstrapping** (from DP).
- Update Rule:
    $$V(S_t) \leftarrow V(S_t) + \alpha [\underbrace{R_{t+1} + \gamma V(S_{t+1})}_{\text{Target}} - V(S_t)]$$
## 9. Key Equations Summary

### MDP Definitions

$$G_t = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$$

$$V^\pi(s) = \mathbb{E}_\pi[G_t | S_t = s]$$

$$Q^\pi(s,a) = \mathbb{E}_\pi[G_t | S_t = s, A_t = a]$$

### Bellman Expectation Equations

$$V^\pi(s) = \sum_a \pi(a|s) \sum_{s'} P(s'|s,a)[R(s,a,s') + \gamma V^\pi(s')]$$

$$Q^\pi(s,a) = \sum_{s'} P(s'|s,a)[R(s,a,s') + \gamma \sum_{a'} \pi(a'|s') Q^\pi(s',a')]$$

### Bellman Optimality Equations
$$
V^*(s) = \max_{a} \sum_{s'} P(s' \mid s,a) \bigl[ R(s,a,s') + \gamma V^*(s') \bigr]
$$

$$
Q^*(s,a) = \sum_{s'} P(s' \mid s,a) \bigl[ R(s,a,s') + \gamma \max_{a'} Q^*(s',a') \bigr]
$$
---
## 10. Grid World Examples

### 10.1 Simple Two-State Example (from lecture)

**Setup**:

- Two states: S1 and S2
- From S1, taking action leads to S2 with reward +2
- From S2, taking action leads to S1 with reward 0
- Deterministic transitions (probability 1)

**Transition Matrix**:

```
P = [0 1]    (Identity transpose - go from one state to the other)
    [1 0]
```

**Reward Vector**: R = [2, 0]

**Solving with γ (discount factor)**:

Using V = R + γPV, we get:

- V(S1) = 2 + γ × 0 = 2 + γV(S2)
- V(S2) = 0 + γ × V(S1)

Solving: V(S1) = 2/(1-γ²), V(S2) = 2γ/(1-γ²)

For γ = 0.9: V(S1) ≈ 10, V(S2) ≈ 9

### 10.2 Grid World Policy Iteration Example

**Setup**: 4×4 grid
- Two terminal states (top-left and bottom-right corners)
- Reward = -1 for each step (encourages finding shortest path)
- Actions: Up, Down, Left, Right
- Deterministic transitions (but hitting wall = stay in place)
- Initial policy: Uniform random (0.25 probability each direction)

**Iteration Process**:
1. **Initialize**: V = 0 for all states, π = uniform random
2. **Policy Evaluation**: Calculate V^π using iterative Bellman equation    
3. **Policy Improvement**: Act greedily - select actions pointing toward higher-value neighbors
4. **Repeat** until policy stabilizes

**Key Insight**: After just a few iterations, the policy shows optimal arrows pointing toward terminal states via shortest paths.

# Value Iteration: The Recycling Robot
**Goal:** Calculate $V^*(High)$ and $V^*(Low)$ by checking the `max` option at every single step.
## 1. The Setup (Parameters)
- **States:** `High` (Full Battery), `Low` (Low Battery).
- **Discount Factor (**$\gamma$**):** $0.9$  
- **Rewards:**
    - Search: $+5$  
    - Wait: $+1$  
    - Recharge: $0$  
    - Empty Battery (Penalty): $-3$  
- **Probabilities:**
    - **From High:** If you Search, $70\%$ chance stay High ($\alpha=0.7$), $30\%$ drop to Low.
    - **From Low:** If you Search, $50\%$ chance stay Low ($\beta=0.5$), $50\%$ battery dies (reset to High).
## 2. Iteration 0: Initialization
We start knowing nothing.
- $V_0(High) = 0$  
- $V_0(Low) = 0$  
## 3. Iteration 1
We calculate the value for **every action** assuming future values are $0$, then pick the winner (`max`).
### **State: High**
- **Option A (Wait):** $1 + 0.9(0) = \mathbf{1}$  
- **Option B (Search):**
    - $0.7[5 + 0.9(0)] + 0.3[5 + 0.9(0)]$  
    - $0.7(5) + 0.3(5) = \mathbf{5}$  
- **Winner:** Search ($5$).
- **Update:** $V_1(High) = 5$  
### **State: Low**
- **Option A (Wait):** $1 + 0.9(0) = \mathbf{1}$  
- **Option B (Search):**
    - $0.5[5 + 0.9(0)] + 0.5[-3 + 0.9(0)]$  
    - $2.5 + (-1.5) = \mathbf{1}$  
- **Option C (Recharge):** $0 + 0.9(0) = \mathbf{0}$  
- **Winner:** Wait or Search (Tie, both $1$). Let's pick **Wait**.
- **Update:** $V_1(Low) = 1$  
**End of Iteration 1:**
- $V_1(High) = 5$  
- $V_1(Low) = 1$  
## 4. Iteration 2

Now we use the values from Iteration 1 ($V(H)=5, V(L)=1$).
### **State: High**
- **Option A (Wait):**
    - $1 + 0.9(\text{Old High Value})$  
    - $1 + 0.9(5) = 1 + 4.5 = \mathbf{5.5}$  
- **Option B (Search):**
    - $0.7[5 + 0.9(\mathbf{5})] + 0.3[5 + 0.9(\mathbf{1})]$  
    - $0.7(9.5) + 0.3(5.9)$  
    - $6.65 + 1.77 = \mathbf{8.42}$  
- **Winner:** Search ($8.42$).
- **Update:** $V_2(High) = 8.42$  
### **State: Low**
- **Option A (Wait):**
    - $1 + 0.9(\text{Old Low Value})$  
    - $1 + 0.9(1) = \mathbf{1.9}$  
- **Option B (Search):**
    - $0.5[5 + 0.9(\mathbf{1})] + 0.5[-3 + 0.9(\mathbf{5})]$  
    - $0.5(5.9) + 0.5(1.5)$  
    - $2.95 + 0.75 = \mathbf{3.7}$  
- **Option C (Recharge):**
    - $0 + 0.9(\text{Old High Value})$ _(Recharging sends you to High)_
    - $0 + 0.9(5) = \mathbf{4.5}$  
- **Winner:** Recharge ($4.5$).
- **Update:** $V_2(Low) = 4.5$  
**End of Iteration 2:**
- $V_2(High) = 8.42$  
- $V_2(Low) = 4.5$  

# Policy Iteration: The Recycling Robot
**Goal:** Find the optimal policy $\pi^*$ by alternating between **evaluating** a specific policy fully and **improving** it.
## 1. The Setup (Same Parameters)
- **States:** `High`, `Low`
- **Discount (**$\gamma$**):** $0.9$
- **Rewards:** Search ($+5$), Wait ($+1$), Recharge ($0$), Empty Battery ($-3$)
- **Dynamics:**
    - **High:** Search $\to$ $70\%$ High, $30\%$ Low.
    - **Low:** Search $\to$ $50\%$ Low, $50\%$ High (reset).
## 2. Initialization
We must start with an arbitrary Policy (not arbitrary values).
Let's choose a "lazy" policy:
- **$\pi_0(High)$:** **Wait**
- **$\pi_0(Low)$:** **Wait**
---
## 3. Iteration 1
### **Step A: Policy Evaluation**
We calculate the **true value** of staying in this policy forever. Since the policy is fixed, these are simple linear equations: $V(s) = R + \gamma V(s')$.
1. For High (Wait):$$V(High) = 1 + 0.9 V(High)$$
$$0.1 V(High) = 1 \implies \mathbf{V(High) = 10}$$
2. For Low (Wait):$$V(Low) = 1 + 0.9 V(Low)$$$$0.1 V(Low) = 1 \implies \mathbf{V(Low) = 10}$$
_Current Values:_ $V(H)=10, V(L)=10$.
### **Step B: Policy Improvement**
Now we run a "Greedy Check" (like one step of Value Iteration) to see if changing our action improves the value.
**Check State: High**
- **Current Policy (Wait):** $10$
- **Try Search:**
    - $0.7[5 + 0.9(10)] + 0.3[5 + 0.9(10)]$
    - $0.7(14) + 0.3(14) = \mathbf{14}$
- **Winner:** Search ($14 > 10$).
- **Update Policy:** $\pi(High) \leftarrow \text{Search}$
**Check State: Low*
- **Current Policy (Wait):** $10$
- **Try Search:**
    - $0.5[5 + 0.9(10)] + 0.5[-3 + 0.9(10)]$
    - $0.5(14) + 0.5(6) = 7 + 3 = \mathbf{10}$ (Tie)
- **Try Recharge:**
    - $0 + 0.9(V(High))$
    - $0 + 0.9(10) = \mathbf{9}$
- **Winner:** Wait (or Search). Let's stick with **Wait**.
- **Update Policy:** $\pi(Low) \leftarrow \text{Wait}$ (Unchanged)
**End of Iteration 1:**
- **New Policy $\pi_1$:** **Search** (High), **Wait** (Low).
## 4. Iteration 2
### **Step A: Policy Evaluation**
Calculate true values for $\pi_1$ (Search, Wait).
1. **For Low (Wait):**
    - Equation unchanged from before (points to itself).
    - $\mathbf{V(Low) = 10}$.
2. **For High (Search):**
    - $V(H) = 0.7[5 + 0.9V(H)] + 0.3[5 + 0.9V(L)]$
    - $V(H) = 3.5 + 0.63V(H) + 1.5 + 0.27(10)$
    - $V(H) = 5 + 0.63V(H) + 2.7$
    - $V(H) = 7.7 + 0.63V(H)$
    - $0.37V(H) = 7.7$
    - $\mathbf{V(H) \approx 20.81}$
_Current Values:_ $V(H) \approx 20.81, V(L) = 10$.
### **Step B: Policy Improvement**
Check if we can beat the current policy using these new, higher values.
**Check State: High**
- **Current (Search):** $20.81$
- **Try Wait:** $1 + 0.9(20.81) \approx 19.7$
- **Winner:** Search. (No Change)
**Check State: Low**
- **Current (Wait):** $1 + 0.9(10) = 10$
- **Try Search:**
    - $0.5[5 + 0.9(10)] + 0.5[-3 + 0.9(20.81)]$
    - $0.5(14) + 0.5(15.73) = 7 + 7.86 = \mathbf{14.86}$
- **Try Recharge:**
    - $0 + 0.9(V_{High})$
    - $0 + 0.9(20.81) = \mathbf{18.73}$
- **Winner:** Recharge ($18.73$).
- **Update Policy:** $\pi(Low) \leftarrow \text{Recharge}$
**End of Iteration 2:**
- **New Policy $\pi_2$:** **Search** (High), **Recharge** (Low).
## 5. Iteration 3 (Convergence)
If we evaluate $\pi_2$ (Search, Recharge), we find the final converged values ($V(H) \approx 39.4, V(L) \approx 35.4$). Performing the greedy check one last time confirms that no actions change.
- **Stable Policy:** Search (High), Recharge (Low).
- **Process Terminated.**
## Summary: The Differences

|**Feature**|**Value Iteration (VI)**|**Policy Iteration (PI)**|
|---|---|---|
|**Primary Focus**|Updates **Values** ($V$) directly.|Updates the **Policy** ($\pi$) explicitly.|
|**The "Evaluation" Step**|**1 sweep**: Updates value based on the previous estimate (truncated).|**Solve fully**: Calculates the _exact_ infinite-horizon value of the current policy (System of Equations).|
|**Complexity per Step**|Cheap (O(S²A)). Just one update.|Expensive (O(S³)). Requires solving linear equations.|
|**Number of Iterations**|Many iterations to converge.|Few iterations (often converges in <5 steps).|
|**The Strategy**|"I'll update my value guess little by little until it stabilizes."|"I'll pick a strategy, calculate exactly how good it is, switch to a better one, repeat."|

![[Models]]