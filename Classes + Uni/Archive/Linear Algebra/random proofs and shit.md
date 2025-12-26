## Sesquilinearity

One of the defining axioms of a complex inner product space is conjugate symmetry.
$$
\langle x,y \rangle = \overline{\langle y,x \rangle }
$$
This means that if you swap the order of the vectors, you must take the complex conjugate of the resulting number.

In complex inner product spaces, we cannot have linearity in both arguments at the same time if we want to preserve the concept of **length**.

If the inner product were linear in both arguments (bilinear), then the "length" of a complex vector would not always be a real, positive number.

If the inner product were linear in both slots, consider a vector $v$ and a complex scalar $i$:

- **Linear in 1st slot:** $\langle iv, v \rangle = i \langle v, v \rangle$
- **Linear in 2nd slot:** $\langle v, iv \rangle = i \langle v, v \rangle$
$$\langle iv, iv \rangle = i \cdot i \cdot \langle v, v \rangle = i^2 \langle v, v \rangle = -1 \langle v, v \rangle$$

The "length squared" of $iv$ would be a **negative number**. To ensure that $\langle v, v \rangle$ is always a positive real number (so we can take its square root to find distance), we _must_ have one slot be **conjugate-linear**.

To fix this, inner product is **sesquilinear** (meaning "one-and-a-half linear").

- **1st slot:** Standard linear.
- **2nd slot:** Conjugate linear.

With conjugate linearity in the second slot, the calculation for $iv$ becomes:

$$\langle iv, iv \rangle = i \cdot \bar{i} \cdot \langle v, v \rangle = i(-i) \langle v, v \rangle = 1 \langle v, v \rangle$$

Because $i \cdot \bar{i} = 1$, the length stays positive and real.


## Theorem 6.1

### A
$$
\begin{gather}
\langle x, y+z \rangle = \langle x,y \rangle + \langle x,z \rangle \\
\text{Proof:} \\
\langle x, y + z \rangle  = \overline{\langle y + z, x \rangle} = \overline{\langle y,x \rangle} + \overline{\langle z,x \rangle} = \langle x,y \rangle  + \langle x,z \rangle 
\end{gather}
$$
### B
$$
\begin{gather}
\langle x, cy \rangle = \overline{\langle cy, x \rangle } = \bar{c} \overline{\langle y,x \rangle } = \bar{c} \langle x,y \rangle
\end{gather}
$$
### C
$$
\begin{gather}
\langle x,0 \rangle = \sum_{i=1}^{n} x_{i}0 = \mathbf{0} \\
\langle 0,x \rangle = \sum_{i=1}^{n} 0x_{i} = \mathbf{0} \\
\end{gather}
$$
Since both are the $\mathbf{0}$ vector, they are equal to each other.
### D
$$
\langle x,x \rangle = \mathbf{0} \iff x = \mathbf{0} 
$$
Say we have that
$$
\langle x, x \rangle = \mathbf{0}
$$
Let $x = (a_{1}, a_{2}, \dots, a_{n})$
~~Then we can represent $\langle x,x \rangle$ as a summation of a's:~~
$$
\langle x,x \rangle = \sum ^{n}_{i=1}a_{i}a_{i} = \sum ^{n}_{i=1}a_{i}^{2}
$$

For a complex vector space, the inner product is defined using the **complex conjugate** ($\bar{a}$). Instead of just squaring the components, we multiply each component by its conjugate:

$$\langle x, x \rangle = \sum_{i=1}^{n} a_i \bar{a}_i$$

### Why this solves the problem

Recall that for any complex number $z = a + bi$, the product of the number and its conjugate is:

$$z\bar{z} = (a+bi)(a-bi) = a^2 + b^2 = |z|^2$$

Because $a$ and $b$ are real numbers, $a^2 + b^2$ is **always a non-negative real number**. It can only be zero if both $a$ and $b$ are zero.

### Refining your proof

Now, if we re-examine your summation with this adjustment:

1. Assume $\langle x, x \rangle = 0$.
    
2. By definition: $\sum_{i=1}^{n} |a_i|^2 = 0$.
    
3. Since $|a_i|^2 \geq 0$ for all $i$, the only way for the sum of non-negative real numbers to be zero is if every single term is zero:
    
    $$|a_1|^2 = 0, |a_2|^2 = 0, \dots, |a_n|^2 = 0$$
    
4. If $|a_i|^2 = 0$, then $a_i = 0$ for all $i$.
    
5. Therefore, $x = (0, 0, \dots, 0) = \mathbf{0}$.
    