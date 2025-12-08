## Problem 1

### Matrix 1: $A = \begin{pmatrix} 5 & -3 \\ 6 & -4 \end{pmatrix}$

$$p_A(x) = \det(xI - A) = \det \begin{pmatrix} x-5 & 3 \\ -6 & x+4 \end{pmatrix}$$

$$= (x-5)(x+4) - (3)(-6)$$

$$= (x^2 - x - 20) + 18$$

$$= x^2 - x - 2$$

$$= (x-2)(x+1)$$

### Matrix 2: $B = \begin{pmatrix} 2 & 2 \\ -1 & 0 \end{pmatrix}$

$$p_B(x) = \det(xI - B) = \det \begin{pmatrix} x-2 & -2 \\ 1 & x \end{pmatrix}$$

$$= x(x-2) - (-2)$$

$$= x^2 - 2x + 2$$

### Matrix 3: $C = \begin{pmatrix} 0 & 1 \\ -4 & 4 \end{pmatrix}$

$$p_C(x) = \det(xI - C) = \det \begin{pmatrix} x & -1 \\ 4 & x-4 \end{pmatrix}$$

$$= x(x-4) - (-4)$$

$$= x^2 - 4x + 4$$

$$= (x-2)^2$$

The eigenvalues are repeated ($\lambda = 2$ with multiplicity 2).

We check the smallest power $k=1$:

$$C - 2I = \begin{pmatrix} 0 & 1 \\ -4 & 4 \end{pmatrix} - \begin{pmatrix} 2 & 0 \\ 0 & 2 \end{pmatrix} = \begin{pmatrix} -2 & 1 \\ -4 & 2 \end{pmatrix} \neq 0$$

Since $(C-2I) \neq 0$, $m_C(x) \neq x-2$. Therefore, it must be the next power.

$m_C(x) = (x-2)^2$

---

### Matrix 4 $D = \begin{pmatrix} 2 & 0 & 0 \\ 0 & 2 & 1 \\ 0 & 0 & 2 \end{pmatrix}$

Since $D$ is upper triangular, the determinant is the product of diagonal entries.

$$p_D(x) = (x-2)(x-2)(x-2) = (x-2)^3$$

The minimal polynomial is of the form $(x-2)^k$ where $1 \le k \le 3$.

- Check $k=1$:
    
    $$D - 2I = \begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & 1 \\ 0 & 0 & 0 \end{pmatrix} \neq 0$$
    
- Check $k=2$:
    
    $$(D - 2I)^2 = \begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & 1 \\ 0 & 0 & 0 \end{pmatrix} \begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & 1 \\ 0 & 0 & 0 \end{pmatrix} = \begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix} = 0$$

Since the square of the matrix (shifted by its eigenvalue) results in the zero matrix, the minimal degree is 2. 

$m_D(x) = (x-2)^2$

## Problem 2

### Part (i): How many real matrices $B$ satisfy $A = B^2$?

This corresponds to solving $\mu_i^2 = \lambda_i$ for each eigenvalue.

Since $\lambda_i > 0$, the equation $x^2 = \lambda_i$ has exactly two real solutions:
$$\mu_i = \sqrt{\lambda_i} \quad \text{and} \quad \mu_i = -\sqrt{\lambda_i}$$
For $B = P E P^{-1}$ to be a real matrix, given that $P$ is real, the diagonal matrix $E$ must be real. Thus, we must choose real values for $\mu_i$.
For each of the $n$ eigenvalues, we have 2 independent choices (positive or negative root).
$$\text{Total matrices} = \underbrace{2 \times 2 \times \dots \times 2}_{n \text{ times}} = 2^n$$

So, there are $2^n$ real matrices $B$.

### Part (ii): How many real matrices $B$ satisfy $A = B^3$?

This corresponds to solving $\mu_i^3 = \lambda_i$.

The equation $x^3 = \lambda_i$ has three solutions in the complex plane. However, for $\lambda_i > 0$, there is only one real solution:
$$\mu_i = \sqrt[3]{\lambda_i}$$
As established in part (i), for $B$ to be a real matrix (given real eigenvectors $P$), the eigenvalues $\mu_i$ must be real. If we chose a complex eigenvalue, $B$ would become complex.
We are forced to choose the single real cube root for every eigenvalue. There are no other choices.
$$\text{Total matrices} = 1 \times 1 \times \dots \times 1 = 1$$

So, there is 1 real matrix $B$.

### Part (iii): How many complex matrices $B$ satisfy $A = B^3$?

The equation $x^3 = \lambda_i$ has three distinct complex roots. If we let $r_i = \sqrt[3]{\lambda_i}$ be the real root and $\omega = e^{i 2\pi/3}$, the roots are:
$$r_i, \quad r_i\omega, \quad r_i\omega^2$$
For each of the $n$ eigenvalues, we can independently choose any of these 3 roots to build the diagonal matrix $E$.
$$\text{Total matrices} = \underbrace{3 \times 3 \times \dots \times 3}_{n \text{ times}} = 3^n$$

There are $3^n$ complex matrices $B$.

## Problem 3

![[Screenshot 2025-12-07 at 11.03.32 PM.png]]


### $T^{-1}$ is upper triangular

Saying that the matrix of $T$ is upper triangular in the basis $(v_1,\dots,v_n)$ is the same as saying:

$$
T(v_k) \in \operatorname{span}\{v_1,\dots,v_k\} \quad \text{for each } k.
$$

In other words, when you apply $T$ to anything in the first $k$ basis vectors’ span, the result stays inside that same subspace. It never creates components in $(v_{k+1},\dots,v_n)$.

But $T$ is invertible, so $T$ gives a bijection from $\operatorname{span}\{v_1,\dots,v_k\}$ onto itself. Therefore the inverse map $T^{-1}$ must also send $\operatorname{span}\{v_1,\dots,v_k\}$ into itself for every $k$.

That means that, with respect to the same basis, $T^{-1}$ also never sends a lower-index vector “upwards” to higher-index basis vectors. This is exactly the condition that says the matrix of $T^{-1}$ is upper triangular.

### The diagonal are entries $1/\lambda_j$

For each basis vector $v_j$, the upper triangular form of $T$ means:

$$
T(v_j) = \lambda_j v_j + (\text{} v_1, \dots, v_{j-1}).
$$

Apply $T^{-1}$ to both sides:

$$
v_j = T^{-1}(T(v_j)) = T^{-1}(\lambda_j v_j + \text{lower terms}).
$$

Because $T^{-1}$ is linear, this is

$$
v_j = \lambda_j T^{-1}(v_j) + \text{(} v_1, \dots, v_{j-1}).
$$

Now write

$$
T^{-1}(v_j) = a_j v_j + \text{(} v_1, \dots, v_{j-1}).
$$

$$
\lambda_j a_j = 1 \quad \Rightarrow \quad a_j = \frac{1}{\lambda_j}.
$$

Thus

$$
T^{-1}(v_j) = \frac{1}{\lambda_j} v_j + (\text{lower terms}),
$$

which means the diagonal entry in the $j$-th position of the matrix of $T^{-1}$ is $1/\lambda_j$.

## Problem 4

### (i) Nilpotent Matrices

A: 
If $A^k = 0$, then the only possible eigenvalue of $A$ is 0.

If $Av = \lambda v$, then applying $A^k$ gives

$$
A^k v = \lambda^k v = 0.
$$

The vector $v$ is not zero, so the only way this works is $\lambda = 0$.
So the characteristic polynomial of $A$ must be $x^n$, because all eigenvalues are 0.
By the Cayley–Hamilton Theorem, a matrix satisfies its own characteristic polynomial:

$$
A^n = 0.
$$

So any nilpotent matrix automatically becomes zero by the time you raise it to the $n$-th power.

B: 
If a nilpotent matrix is diagonalizable, then all of its diagonal entries (which are eigenvalues) must be 0.

So any diagonal form would look like:

$$
\text{diag}(0,0,\dots,0).
$$

Which is exactly the zero matrix. So the only diagonalizable nilpotent matrix is the zero matrix itself.

### (ii) Matrices satisfying $A^4 = A$

If
$$
A^4 = A.
$$
Then $A$ satisfies the polynomial:

$$
x^4 - x = 0.
$$
If we factor the polynomial:

$$
x^4 - x = x(x-1)(x-\omega)(x-\omega^2),
$$

where $\omega = e^{2\pi i/3}$.

All four roots are different. So any minimal polynomial that divides this also has only distinct linear factors.
A matrix whose minimal polynomial has no repeated roots is automatically diagonalizable.
Therefore, every matrix satisfying $A^4 = A$ is diagonalizable.

Eigenvalues must be roots of the polynomial $x^4 - x$.
So the possible eigenvalues are:

$$
0,\quad 1,\quad \omega,\quad \omega^2,
$$

where

$$
\omega = \frac{-1 + i\sqrt{3}}{2},\quad \omega^2 = \frac{-1 - i\sqrt{3}}{2}.
$$


### (iii) Formula for the Inverse Using the Minimal Polynomial

A matrix is invertible exactly when zero is not an eigenvalue.
But the roots of the minimal polynomial are the eigenvalues.
So:
* If $a_0 = 0$, that means $m_A(0)=0$, so 0 is a root $\rightarrow$ 0 is an eigenvalue $\rightarrow$ not invertible.
* If $a_0 \neq 0$, then 0 is not a root $\rightarrow$ $A$ has no zero eigenvalues $\rightarrow$ invertible.
So:

$$
A \text{ invertible } \iff a_0 \neq 0.
$$

Since $m_A(A) = 0$, plug $A$ into the polynomial:

$$
A^m + a_{m-1}A^{m-1} + \dots + a_1 A + a_0 I = 0.
$$
$$
a_0 I = -(A^m + a_{m-1}A^{m-1} + \dots + a_1 A).
$$
$$
a_0 I = -A(A^{m-1} + a_{m-1}A^{m-2} + \dots + a_1 I).
$$
$$
A^{-1} = -\frac{1}{a_0}\big(A^{m-1} + a_{m-1}A^{m-2} + \dots + a_1 I\big).
$$