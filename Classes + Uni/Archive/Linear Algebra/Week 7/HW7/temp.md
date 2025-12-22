Here is a detailed, step-by-step solution to Problem 4.

The problem provides two matrices, $A$ and $B$:
[cite_start]$A=\begin{pmatrix}1&1&0\\ 0&1&1\\ 1&2&1\end{pmatrix}$ [cite: 25] [cite_start]and $B=A^{t}=\begin{pmatrix}1&0&1\\ 1&1&2\\ 0&1&1\end{pmatrix}$ [cite: 26]

---

### (i) Find all $x\in\mathbb{R}^{3}$ such that $Bx=0$

[cite_start]This part asks for the null space of matrix $B$, denoted $N(B)$[cite: 27]. We need to solve the homogeneous system $Bx = 0$. We set up an augmented matrix $[B | 0]$ and find its reduced row-echelon form (RREF).

**1. Set up the augmented matrix:**
$$\left( \begin{array}{ccc|c} 1 & 0 & 1 & 0 \\ 1 & 1 & 2 & 0 \\ 0 & 1 & 1 & 0 \end{array} \right)$$

**2. Perform row operations to find the RREF:**
* $R_2 \leftarrow R_2 - R_1$
    $$\left( \begin{array}{ccc|c} 1 & 0 & 1 & 0 \\ 0 & 1 & 1 & 0 \\ 0 & 1 & 1 & 0 \end{array} \right)$$
* $R_3 \leftarrow R_3 - R_2$
    $$\left( \begin{array}{ccc|c} 1 & 0 & 1 & 0 \\ 0 & 1 & 1 & 0 \\ 0 & 0 & 0 & 0 \end{array} \right)$$

**3. Interpret the RREF:**
The matrix is now in RREF. The corresponding system of equations is:
* $x_1 + x_3 = 0 \implies x_1 = -x_3$
* $x_2 + x_3 = 0 \implies x_2 = -x_3$
* $x_3$ is a free variable.

**4. Write the general solution:**
Let $x_3 = t$, where $t$ is any scalar in $\mathbb{R}$. The solution vector $x$ is:
$$x = \begin{pmatrix} x_1 \\ x_2 \\ x_3 \end{pmatrix} = \begin{pmatrix} -t \\ -t \\ t \end{pmatrix} = t \begin{pmatrix} -1 \\ -1 \\ 1 \end{pmatrix}$$

**Solution (i):**
The set of all solutions $x \in \mathbb{R}^3$ such that $Bx=0$ is the set of all vectors of the form $t \begin{pmatrix} -1 \\ -1 \\ 1 \end{pmatrix}$ for any $t \in \mathbb{R}$. This is the subspace $\text{span}\left\{\begin{pmatrix} -1 \\ -1 \\ 1 \end{pmatrix}\right\}$.

---

### (ii) Does $Ax=(\begin{smallmatrix}1\\ 2\\ 5\end{smallmatrix})$ have a solution?

[cite_start]This part asks if the vector $b = \begin{pmatrix} 1 \\ 2 \\ 5 \end{pmatrix}$ is in the image (or column space) of $A$[cite: 30]. We can determine this by checking if the system $Ax=b$ is consistent. We set up the augmented matrix $[A | b]$ and row-reduce.

**1. Set up the augmented matrix:**
$$\left( \begin{array}{ccc|c} 1 & 1 & 0 & 1 \\ 0 & 1 & 1 & 2 \\ 1 & 2 & 1 & 5 \end{array} \right)$$

**2. Perform row operations:**
* $R_3 \leftarrow R_3 - R_1$
    $$\left( \begin{array}{ccc|c} 1 & 1 & 0 & 1 \\ 0 & 1 & 1 & 2 \\ 0 & 1 & 1 & 4 \end{array} \right)$$
* $R_3 \leftarrow R_3 - R_2$
    $$\left( \begin{array}{ccc|c} 1 & 1 & 0 & 1 \\ 0 & 1 & 1 & 2 \\ 0 & 0 & 0 & 2 \end{array} \right)$$

**3. Interpret the result:**
The last row of the row-reduced matrix corresponds to the equation:
$0x_1 + 0x_2 + 0x_3 = 2$, which simplifies to $0 = 2$.

This is a contradiction.

**Solution (ii):**
[cite_start]**No**, the system does not have a solution[cite: 31]. The system is inconsistent, which we can see because the row-echelon form of the augmented matrix $[A|b]$ has a pivot in the final (augmented) column. This means the vector $\begin{pmatrix} 1 \\ 2 \\ 5 \end{pmatrix}$ is not in the image of $A$.

---

### (iii) Find all solutions to $Ax=By$

[cite_start]This part asks for all vectors $x, y \in \mathbb{R}^3$ that satisfy the equation $Ax = By$ [cite: 32] (assuming the "$c$" in the prompt is a typo for "$x$"). We can rewrite this as $Ax - By = 0$. This is a homogeneous system of linear equations with 6 variables ($x_1, x_2, x_3, y_1, y_2, y_3$).

As suggested by the hint, we can solve this by finding the null space of the 3x6 matrix $[A | [cite_start]-B]$[cite: 33]. [cite_start]Alternatively, we can follow the hint to solve $[A|B]\begin{pmatrix}x\\ -y\end{pmatrix}=0$[cite: 33, 35]. Let's find the null space of the matrix $C = [A | B]$ and let the solution vector be $v = (x_1, x_2, x_3, -y_1, -y_2, -y_3)^t$.

**1. Set up the 3x6 augmented matrix $[C | 0]$:**
$C = [A | B] = \left( \begin{array}{ccc|ccc} 1 & 1 & 0 & 1 & 0 & 1 \\ 0 & 1 & 1 & 1 & 1 & 2 \\ 1 & 2 & 1 & 0 & 1 & 1 \end{array} \right)$

We solve $Cv = 0$:
$$\left( \begin{array}{cccccc|c} 1 & 1 & 0 & 1 & 0 & 1 & 0 \\ 0 & 1 & 1 & 1 & 1 & 2 & 0 \\ 1 & 2 & 1 & 0 & 1 & 1 & 0 \end{array} \right)$$

**2. Perform row operations to find the RREF:**
* $R_3 \leftarrow R_3 - R_1$
    $$\left( \begin{array}{cccccc|c} 1 & 1 & 0 & 1 & 0 & 1 & 0 \\ 0 & 1 & 1 & 1 & 1 & 2 & 0 \\ 0 & 1 & 1 & -1 & 1 & 0 & 0 \end{array} \right)$$
* $R_3 \leftarrow R_3 - R_2$
    $$\left( \begin{array}{cccccc|c} 1 & 1 & 0 & 1 & 0 & 1 & 0 \\ 0 & 1 & 1 & 1 & 1 & 2 & 0 \\ 0 & 0 & 0 & -2 & 0 & -2 & 0 \end{array} \right)$$
* $R_3 \leftarrow -\frac{1}{2}R_3$
    $$\left( \begin{array}{cccccc|c} 1 & 1 & 0 & 1 & 0 & 1 & 0 \\ 0 & 1 & 1 & 1 & 1 & 2 & 0 \\ 0 & 0 & 0 & 1 & 0 & 1 & 0 \end{array} \right)$$
* This is row-echelon form. Now we reduce to RREF (moving upwards).
* $R_2 \leftarrow R_2 - R_3$
* $R_1 \leftarrow R_1 - R_3$
    $$\left( \begin{array}{cccccc|c} 1 & 1 & 0 & 0 & 0 & 0 & 0 \\ 0 & 1 & 1 & 0 & 1 & 1 & 0 \\ 0 & 0 & 0 & 1 & 0 & 1 & 0 \end{array} \right)$$
* $R_1 \leftarrow R_1 - R_2$
    $$\left( \begin{array}{cccccc|c} 1 & 0 & -1 & 0 & -1 & -1 & 0 \\ 0 & 1 & 1 & 0 & 1 & 1 & 0 \\ 0 & 0 & 0 & 1 & 0 & 1 & 0 \end{array} \right)$$

**3. Interpret the RREF:**
The variables are $v = (x_1, x_2, x_3, -y_1, -y_2, -y_3)^t$.
* **Pivot columns:** 1, 2, 4 (corresponding to $x_1, x_2, -y_1$).
* **Free columns:** 3, 5, 6 (corresponding to $x_3, -y_2, -y_3$).

Let $x_3 = s$, $-y_2 = t$, and $-y_3 = u$, where $s, t, u \in \mathbb{R}$.
The equations are:
* $x_1 - x_3 - (-y_2) - (-y_3) = 0 \implies x_1 - s - t - u = 0 \implies x_1 = s + t + u$
* $x_2 + x_3 + (-y_2) + (-y_3) = 0 \implies x_2 + s + t + u = 0 \implies x_2 = -s - t - u$
* $(-y_1) + (-y_3) = 0 \implies -y_1 + u = 0 \implies -y_1 = -u$

**4. Write the general solution for $x$ and $y$:**
From the variables, we have:
* $x_1 = s + t + u$
* $x_2 = -s - t - u$
* $x_3 = s$

And for $y$:
* $-y_1 = -u \implies y_1 = u$
* $-y_2 = t \implies y_2 = -t$
* $-y_3 = u \implies y_3 = -u$

**Solution (iii):**
The solutions are all pairs of vectors $(x, y)$ of the form:
$$x = \begin{pmatrix} s + t + u \\ -s - t - u \\ s \end{pmatrix} \quad \text{and} \quad y = \begin{pmatrix} u \\ -t \\ -u \end{pmatrix}$$
for any scalars $s, t, u \in \mathbb{R}$.

We can also express this as a linear combination:
$x = s\begin{pmatrix} 1 \\ -1 \\ 1 \end{pmatrix} + t\begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix} + u\begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix}$
$y = s\begin{pmatrix} 0 \\ 0 \\ 0 \end{pmatrix} + t\begin{pmatrix} 0 \\ -1 \\ 0 \end{pmatrix} + u\begin{pmatrix} 1 \\ 0 \\ -1 \end{pmatrix}$

---

### (iv) Find a basis of im $A\cap im~B$

[cite_start]The hint states that $\text{im } A \cap \text{im } B$ consists of vectors $z$ such that $z = Ax = By$ for some $x, y$[cite: 38]. [cite_start]This means $z$ is the result of applying $A$ to an $x$ vector that is part of a solution from part (iii)[cite: 39].

**1. Use the general solution for $x$ from part (iii):**
$x = \begin{pmatrix} s + t + u \\ -s - t - u \\ s \end{pmatrix}$

**2. Compute $z = Ax$:**
$$z = Ax = \begin{pmatrix} 1 & 1 & 0 \\ 0 & 1 & 1 \\ 1 & 2 & 1 \end{pmatrix} \begin{pmatrix} s + t + u \\ -s - t - u \\ s \end{pmatrix}$$
$$z = \begin{pmatrix} 1(s + t + u) + 1(-s - t - u) + 0(s) \\ 0(s + t + u) + 1(-s - t - u) + 1(s) \\ 1(s + t + u) + 2(-s - t - u) + 1(s) \end{pmatrix}$$
$$z = \begin{pmatrix} (s+t+u) - (s+t+u) \\ (-s-t-u) + s \\ (s+t+u) - (2s+2t+2u) + s \end{pmatrix}$$
$$z = \begin{pmatrix} 0 \\ -t - u \\ (s-2s+s) + (t-2t) + (u-2u) \end{pmatrix}$$
$$z = \begin{pmatrix} 0 \\ -t - u \\ 0 - t - u \end{pmatrix} = \begin{pmatrix} 0 \\ -t - u \\ -t - u \end{pmatrix}$$

**3. Express $z$ as a span:**
We can factor out the scalar part:
$$z = (-t - u) \begin{pmatrix} 0 \\ 1 \\ 1 \end{pmatrix}$$
Since $s, t, u$ can be any real numbers, their combination $k = -t-u$ can also be any real number.
So, the intersection $\text{im } A \cap \text{im } B$ is the set of all vectors $z = k \begin{pmatrix} 0 \\ 1 \\ 1 \end{pmatrix}$ for $k \in \mathbb{R}$.

This is the span of the vector $\begin{pmatrix} 0 \\ 1 \\ 1 \end{pmatrix}$.

**Solution (iv):**
A basis for $\text{im } A \cap \text{im } B$ is $\left\{ \begin{pmatrix} 0 \\ 1 \\ 1 \end{pmatrix} \right\}$.

---

### (v) Find a basis of $L_{A}^{-1}(im~B)$

[cite_start]The hint states that $L_{A}^{-1}(\text{im } B)$ is the set of all $x \in \mathbb{R}^3$ such that $Ax = By$ for *some* $y \in \mathbb{R}^3$[cite: 41]. [cite_start]This is precisely the set of all $x$ vectors that are part of the general solution to the system $Ax = By$[cite: 42], which we found in part (iii).

**1. Recall the general solution for $x$ from part (iii):**
$$x = \begin{pmatrix} s + t + u \\ -s - t - u \\ s \end{pmatrix}$$
where $s, t, u \in \mathbb{R}$.

**2. Decompose $x$ into a linear combination of vectors:**
We separate the vector $x$ based on the free parameters $s, t,$ and $u$:
$$x = \begin{pmatrix} s \\ -s \\ s \end{pmatrix} + \begin{pmatrix} t \\ -t \\ 0 \end{pmatrix} + \begin{pmatrix} u \\ -u \\ 0 \end{pmatrix}$$
$$x = s \begin{pmatrix} 1 \\ -1 \\ 1 \end{pmatrix} + t \begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix} + u \begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix}$$

**3. Identify the spanning set:**
The set $L_{A}^{-1}(\text{im } B)$ is the subspace spanned by the vectors in this combination:
$$L_{A}^{-1}(\text{im } B) = \text{span} \left\{ \begin{pmatrix} 1 \\ -1 \\ 1 \end{pmatrix}, \begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix}, \begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix} \right\}$$

**4. Find a basis:**
A basis is a *linearly independent* spanning set. The vector $\begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix}$ is repeated, so the set is linearly dependent. We can remove the redundant vector. The remaining two vectors:
$v_1 = \begin{pmatrix} 1 \\ -1 \\ 1 \end{pmatrix}$ and $v_2 = \begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix}$
are linearly independent (they are clearly not scalar multiples of each other).

Therefore, these two vectors form a basis for the subspace.

**Solution (v):**
A basis for $L_{A}^{-1}(\text{im } B)$ is $\left\{ \begin{pmatrix} 1 \\ -1 \\ 1 \end{pmatrix}, \begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix} \right\}$.