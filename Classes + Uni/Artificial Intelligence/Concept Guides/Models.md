Here is a universal cheat sheet generalized for **any** LLM architecture or exam question. It focuses on the fundamental principles of how these features function and interact, regardless of the specific model (Llama, GPT, Mistral, etc.).

### **I. Model Architecture & Training Features**

This section explains what physical components of a Transformer do and how changing them affects performance.

|**Feature**|**General Definition**|**Impact & Interdependencies**|
|---|---|---|
|**Parameter Count**|The total number of trainable weights in the model.|**Capacity Proxy:** Generally, more parameters = higher potential for reasoning and knowledge storage. However, performance depends heavily on having enough _Training Tokens_ to saturate these parameters.|
|**Layers (Depth)**|The number of sequential transformer blocks stacked on top of each other.|**Reasoning Steps:** Deeper models can perform more sequential steps of logic or abstraction.<br><br>  <br><br>**Trade-off:** increasing depth makes training harder (vanishing gradients) and increases inference latency linearly.|
|**Hidden / Latent Size (Width)**|The dimension of the vector representing a single token ($d_{model}$).|**Information Density:** Determines how much distinct semantic information (syntax, meaning, context) can be compressed into a single token representation. A wider model learns richer features.|
|**Attention Heads**|Parallel mechanisms that allow tokens to "look at" other tokens in the sequence.|**Relationship Tracking:** Each head captures a different type of relationship (e.g., one tracks syntax, another tracks pronouns).<br><br>  <br><br>**Interdependency:** More heads usually require a larger _Hidden Size_ so that each head has a sub-space to work in.|
|**FFN Dimension**|The size of the Feed-Forward Network within each layer (usually $4 \times d_{model}$).|**Knowledge Storage:** The FFN is where the model processes information _individually_ per token. It is often considered the primary location where static "facts" are stored in the weights.|
|**Vocabulary Size**|The number of unique tokens (sub-words) the model can recognize.|**Compression Efficiency:** A larger vocab means text is represented by fewer tokens (better compression).<br><br>  <br><br>**Interdependency:** Higher compression effectively _multiplies_ the **Context Window** (e.g., 4k tokens holds more actual text if the vocab is efficient).|
|**Context Length**|The maximum number of tokens the model can process in one forward pass.|**Working Memory:** Determines the maximum scope of "short-term memory" for RAG, document analysis, or conversation history.<br><br>  <br><br>**Cost:** Memory usage (KV Cache) scales quadratically with context length (unless optimizations like Flash Attention are used).|
|**Training Tokens**|The total amount of data the model was shown during pre-training.|**Generalization:** The most critical factor for performance. A smaller model trained on _more_ tokens (over-trained) often outperforms a larger model trained on fewer tokens.|

---

### **II. Inference & Sampling Metrics (Hyperparameters)**

These settings control the "Softmax" step—how the model converts its internal math into a final word choice.

#### **1. Temperature ($T$) $\rightarrow$ The "Risk" Slider**

- **Definition:** A scalar that divides the logits (raw scores) before the Softmax function.
    
- **Mechanism:**
    
    - **High ($>1.0$):** Flattens the probability distribution. Makes unlikely tokens more likely. **Result:** Creativity, diversity, higher risk of hallucination.
        
    - **Low ($<1.0$):** Sharpens the distribution. Highlights the single best answer and suppresses the rest. **Result:** Determinism, logic, repetition.
        
- **Rule of Thumb:** Use Low ($0.1$) for coding/math; High ($0.8+$) for creative writing.
    

#### **2. Top-k $\rightarrow$ The "Hard" Cutoff**

- **Definition:** Truncates the vocabulary to strictly the $k$ highest-probability tokens.
    
- **Mechanism:** Sorts all words by probability $\rightarrow$ Keeps top $k$ $\rightarrow$ Normalizes to 100% $\rightarrow$ Samples from that pool.
    
- **Limitation:** It is static. If $k=50$, it cuts off the 51st word even if it was a good option (valid context lost), or keeps the 50th word even if it was nonsense (garbage included).
    

#### **3. Top-p (Nucleus Sampling) $\rightarrow$ The "Smart" Cutoff**

- **Definition:** Truncates the vocabulary based on the cumulative probability threshold $p$.
    
- **Mechanism:** Sorts words by probability $\rightarrow$ Adds them one by one until the sum reaches $p$ (e.g., 0.90) $\rightarrow$ Discards the rest.
    
- **Advantage:** **Dynamic size.**
    
    - If the model is _sure_ (one word is 90%), the pool is just 1 word.
        
    - If the model is _unsure_ (many words are 10%), the pool expands to include them all.
        
- **Interaction:** Usually applied _after_ Temperature but _before_ final selection.
    

---

### **III. Cheat Sheet Summary for Exam**

- **Capacity:** Defined by Parameters (Weights).
    
- **Reasoning:** Defined by Depth (Layers).
    
- **Knowledge:** Stored in FFN Dimensions & Training Tokens.
    
- **Efficiency:** Driven by Vocab Size (Tokenizer) & Latent Density.
    
- **Inference:**
    
    - **Temp** = Distribution Shape (Flat vs. Sharp).
        
    - **Top-k** = Rank truncation (Fixed list size).
        
    - **Top-p** = Probability truncation (Dynamic list size).