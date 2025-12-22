## How to Prove Injectivity (One-to-One) 🎯

**The Goal:** Show that no two different vectors in the domain $V$ map to the same vector in the codomain $W$.

The most reliable method for linear maps is to prove that the **kernel** (or null space) only contains the zero vector. The kernel is the set of all vectors that $T$ maps to zero.

**Method: The Kernel Proof**

1.  **State your goal:** To prove $T$ is injective, we will show that $\ker(T) = \{0_V\}$, where $0_V$ is the zero vector in $V$.

2.  **Start the proof:** Assume you have a vector $v$ that is in the kernel of $T$.
    * "Let $v \in \ker(T)$."

3.  **Use the definition of the kernel:** By definition, this means $T(v) = 0_W$, where $0_W$ is the zero vector in $W$.

4.  **Show this forces $v$ to be the zero vector:** Use the specific properties of your transformation $T$ to show that the only way $T(v)$ can be zero is if $v$ itself is the zero vector.
    * This is the main step where you use the definition of $T$. Your goal is to logically arrive at the conclusion "$v = 0_V$."

5.  **Conclude:** "Since the only vector $v$ for which $T(v) = 0_W$ is $v = 0_V$, the kernel is trivial, and therefore, $T$ is injective."

**The Dimension Shortcut**
If you are working with finite-dimensional spaces, you can use the **Rank-Nullity Theorem**:
$$\text{rank}(T) + \text{nullity}(T) = \dim(V)$$
Injectivity means the nullity (the dimension of the kernel) is 0. So, $T$ is injective if and only if:
$$\text{rank}(T) = \dim(V)$$
This is a very fast way to check for injectivity if you know the rank of the transformation.

---

## How to Prove Surjectivity (Onto) 🗺️

**The Goal:** Show that the transformation $T$ can reach *every* vector in the codomain $W$. In other words, the image of $T$ is all of $W$.

**Method: The "Arbitrary Vector" Proof**

1.  **State your goal:** To prove $T$ is surjective, we will show that for any vector $w \in W$, there exists a vector $v \in V$ such that $T(v) = w$.

2.  **Start the proof:** Choose an arbitrary vector from the codomain $W$.
    * "Let $w$ be an arbitrary vector in $W$."

3.  **Find the input vector $v$:** This is the creative part of the proof. You must construct, or at least show the existence of, a vector $v$ in the domain $V$ that maps to your chosen $w$. You are essentially solving the equation $T(v) = w$ for $v$. The steps here will depend entirely on the definition of $T$.

4.  **Conclude:** "Since for any arbitrary $w \in W$, we have found a $v \in V$ such that $T(v) = w$, the transformation $T$ is surjective."

**The Dimension Shortcut**
For finite-dimensional spaces, surjectivity means the image of $T$ is the entire codomain $W$. This can only happen if their dimensions are equal. Since the rank is the dimension of the image, $T$ is surjective if and only if:
$$\text{rank}(T) = \dim(W)$$
This is a quick check if you can easily determine the rank of the transformation.

---

## Summary for Exam Day

| To Prove This... | You Must Show This... |
| :--- | :--- |
| **Injectivity** (One-to-One) | The **kernel** is trivial: $\ker(T) = \{0_V\}$. |
| | OR the **rank** equals the dimension of the **domain**: $\text{rank}(T) = \dim(V)$. |
| **Surjectivity** (Onto) | For any $w \in W$, there is a $v \in V$ such that $T(v)=w$. |
| | OR the **rank** equals the dimension of the **codomain**: $\text{rank}(T) = \dim(W)$. |