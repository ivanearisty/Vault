### **1. The Problem: Why do we need Word2Vec?**

Before Word2Vec, we used **One-Hot Encoding**. You must understand why this fails.

- **The Representation:** A vocabulary of size $V$ (e.g., 100,000 words) implies every word is a vector of length 100,000 with a single `1` and zeros elsewhere.
    
- **The Reasoning Failure:**
    
    - **Orthogonality:** In this space, every word is orthogonal (90°) to every other word.
        
    - **Dot Product:** `hotel · motel = 0`.
        
    - **The Issue:** The dot product is zero, implying **no similarity**. But semantically, "hotel" and "motel" are very similar. One-hot encoding cannot capture meaning.
        

### **2. The Core Intuition: The Distributional Hypothesis**

If you are asked to explain the _theory_ behind Word2Vec, cite **Firth (1957)**:

> _"A word's meaning is given by words that frequently appear close by."_

- **Reasoning:** If "bank" appears near "money" and "investment," and "fund" also appears near "money" and "investment," then "bank" and "fund" must have similar meanings.
    
- **The Goal:** Create a vector space (Embedding Space) where semantically similar words are **geometrically close** (high dot product).
    

### **3. The Architecture: The "Fake" Task**

Word2Vec is trained to solve a dummy prediction task. We don't actually care about the prediction; we care about the **weights learned** while trying to solve it.

- **The Model (Skip-gram):**
    
    - **Input:** A center word $W_t$.
        
    - **Task:** Predict the probability of context words (words around it, e.g., $W_{t-2}, W_{t+2}$).
        
- The Flow:
    
    $$\text{Input (One-hot, } V \text{)} \xrightarrow{\text{Matrix } W} \text{Embedding (} D \text{)} \xrightarrow{\text{Matrix } W'} \text{Output (} V \text{)}$$
    
- **The Bottleneck:** The middle layer (dimension $D$, e.g., 300) is much smaller than $V$ (e.g., 100,000). This forces the model to compress information into a "dense" vector.
    

### **4. The "Secret Sauce": The Matrices**

This is a high-probability concept for the exam.

- **Matrix W ($V \times D$):**
    
    - This projects the one-hot vector into the embedding space.
        
    - **Reasoning:** This matrix _is_ the lookup table. Row $i$ of this matrix is the Word2Vec embedding for word $i$.
        
- **Matrix W' ($D \times V$):**
    
    - This is the "Lifting Matrix." It maps the embedding back to the vocabulary size to calculate probabilities.
        
- **Training Objective:** We want to maximize the probability of the _actual_ context words appearing given the center word.
    
- **The Artifact:** After training, we **throw away W'** and the rest of the model. We only keep **Matrix W**. This is the file you download when you "use Word2Vec."
    

### **5. Critical Limitations (Reasoning Question)**

The TA mentioned "reasoning," so be prepared to argue why Word2Vec is limited compared to Transformers.

- **Context-Free Nature:** Word2Vec generates **static** embeddings.
    
- **The "Bank" Problem:**
    
    - Sentence A: "I sat on the river **bank**."
        
    - Sentence B: "I went to the **bank** to deposit money."
        
- **Word2Vec's Failure:** In Word2Vec, the vector for "bank" is the **same** in both sentences. It is an average of all meanings (river + finance).
    
- **Contrast:** This motivates **Transformers/Attention**, which generate _contextual_ embeddings (moving the vector based on neighbors).
    

### **6. Practice Reasoning Questions**

Try to answer these mentally to check your readiness:

1. **Q:** If I train Word2Vec on a dataset of financial news, and you train it on a dataset of National Geographic magazines, will our vector for "jaguar" be the same?
    
    - **A:** No. In finance, "jaguar" will appear near "stock," "car," "luxury." In NatGeo, "jaguar" will appear near "jungle," "predator," "prey." The embedding coordinates depend entirely on the **co-occurrence patterns** in the specific training data.
        
2. **Q:** Why do we typically use the dot product to measure similarity in Word2Vec space?
    
    - **A:** During training, the model tries to maximize the probability of co-occurring words. The math involves taking the dot product of the center word and context word vectors ($u \cdot v$). Therefore, words that appear together often are "pushed" to have a high dot product.
        
3. **Q:** Why is the hidden layer dimension $D$ important?
    
    - **A:** It creates a "bottleneck." If $D$ were equal to $V$, the model could just memorize identity mappings. By making $D \ll V$, the model is forced to learn efficient, dense representations that capture semantic relationships (compression).
        