Here is the solution rewritten to sound more like a direct, practical student submission. I’ve trimmed the wordy explanations and focused on the calculation steps and logic flow.

## Problem 1

### Matrix $A_1 = \begin{pmatrix} 2 & 2 \\ -1 & 0 \end{pmatrix}$

**1. Real Canonical Form**

First, find the characteristic polynomial:
$$p(\lambda) = \det(\lambda I - A_1) = (\lambda-2)\lambda - (-2) = \lambda^2 - 2\lambda + 2$$

Roots (eigenvalues) are:
$$\lambda = \frac{2 \pm \sqrt{4-8}}{2} = 1 \pm i$$

We have complex eigenvalues $a \pm bi$ with $a=1, b=1$. By **Theorem 12.10**, the real canonical form is:
$$\boxed{C = \begin{pmatrix} 1 & 1 \\ -1 & 1 \end{pmatrix}}$$

**2. Finding P**

We need an eigenvector for $\lambda = 1+i$. Solve $(A_1 - (1+i)I)v = 0$:
$$\begin{pmatrix} 1-i & 2 \\ -1 & -1-i \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}$$

From row 2: $-x + (-1-i)y = 0 \Rightarrow x = -(1+i)y$.
Let $y=1$, then $x = -1-i$.
$$v = \begin{pmatrix} -1-i \\ 1 \end{pmatrix} = \begin{pmatrix} -1 \\ 1 \end{pmatrix} + i\begin{pmatrix} -1 \\ 0 \end{pmatrix}$$

Using **Lemma 12.11**, the change of basis matrix $P$ uses the real and imaginary parts as columns:
$$P = \begin{pmatrix} -1 & -1 \\ 1 & 0 \end{pmatrix}$$
$$P^{-1} = \frac{1}{1}\begin{pmatrix} 0 & 1 \\ -1 & -1 \end{pmatrix} = \begin{pmatrix} 0 & 1 \\ -1 & -1 \end{pmatrix}$$

**3. Computing $e^{A_1}$**

The exponential of the canonical form $C = I + J$ (where $J$ is the rotation part) is:
$$e^C = e^1 \begin{pmatrix} \cos 1 & \sin 1 \\ -\sin 1 & \cos 1 \end{pmatrix}$$

Then $e^{A_1} = P e^C P^{-1}$:
$$e^{A_1} = e \begin{pmatrix} -1 & -1 \\ 1 & 0 \end{pmatrix} \begin{pmatrix} \cos 1 & \sin 1 \\ -\sin 1 & \cos 1 \end{pmatrix} \begin{pmatrix} 0 & 1 \\ -1 & -1 \end{pmatrix}$$

Multiplying this out:
$$\boxed{e^{A_1} = e\begin{pmatrix} \cos 1 + \sin 1 & 2\sin 1 \\ -\sin 1 & \cos 1 - \sin 1 \end{pmatrix}}$$



### Matrix $A_2 = \begin{pmatrix} 0 & 1 \\ -4 & 4 \end{pmatrix}$

**1. Real Canonical Form**

$$p(\lambda) = \lambda(\lambda-4) - (-4) = \lambda^2 - 4\lambda + 4 = (\lambda-2)^2$$

Eigenvalue $\lambda = 2$ with algebraic multiplicity 2. Check the rank of $A - 2I$:
$$A - 2I = \begin{pmatrix} -2 & 1 \\ -4 & 2 \end{pmatrix}$$

Rank is 1, so geometric multiplicity is 1. Since $1 < 2$, it's not diagonalizable. The Jordan form is:
$$\boxed{J = \begin{pmatrix} 2 & 1 \\ 0 & 2 \end{pmatrix}}$$

**2. Finding P**

We need a generalized eigenvector chain.
First, pick $v_2$ such that $(A-2I)v_2 \neq 0$. Let $v_2 = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$.
Then calculate $v_1 = (A-2I)v_2$:
$$v_1 = \begin{pmatrix} -2 & 1 \\ -4 & 2 \end{pmatrix}\begin{pmatrix} 1 \\ 0 \end{pmatrix} = \begin{pmatrix} -2 \\ -4 \end{pmatrix}$$

Let $v_1 = \begin{pmatrix} 1 \\ 2 \end{pmatrix}$
Solve $(A-2I)v_2 = v_1$:
$$\begin{pmatrix} -2 & 1 \\ -4 & 2 \end{pmatrix}v_2 = \begin{pmatrix} 1 \\ 2 \end{pmatrix} \Rightarrow \text{Solution: } v_2 = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$$

So $P = \begin{pmatrix} 1 & 0 \\ 2 & 1 \end{pmatrix}$ and $P^{-1} = \begin{pmatrix} 1 & 0 \\ -2 & 1 \end{pmatrix}$.

**3. Computing $e^{A_2}$**

For a Jordan block, $e^{J_2(2)} = e^2 \begin{pmatrix} 1 & 1 \\ 0 & 1 \end{pmatrix}$.
$$e^{A_2} = P e^J P^{-1} = \begin{pmatrix} 1 & 0 \\ 2 & 1 \end{pmatrix} \begin{pmatrix} e^2 & e^2 \\ 0 & e^2 \end{pmatrix} \begin{pmatrix} 1 & 0 \\ -2 & 1 \end{pmatrix}$$

$$\boxed{e^{A_2} = e^2\begin{pmatrix} -1 & 1 \\ -4 & 3 \end{pmatrix}}$$



## Problem 2

### i
Let $P$ be the "reverse identity" permutation matrix $(e_n, e_{n-1}, \dots, e_1)$.
Note that $P = P^{-1} = P^t$.

If we compute $P^{-1} J_n(\lambda) P$, we are effectively reversing the rows and then reversing the columns.
- The diagonal $\lambda$'s stay on the diagonal.
- The super-diagonal of 1s (positions $(i, i+1)$) flips to the sub-diagonal (positions $(i+1, i)$).

This structure is exactly the transpose $J_n(\lambda)^t$.

### ii
By **Theorem 12.4**, any matrix $A$ is similar to its Jordan form $J$:
$$A \sim J$$

$J$ is a block diagonal matrix. From part (i), each block $J_k(\lambda)$ is similar to its transpose $J_k(\lambda)^t$.
Therefore, the whole matrix $J$ is similar to $J^t$.
$$J \sim J^t$$

Since $(Q^{-1}AQ)^t = Q^t A^t (Q^{-1})^t$, similarity is preserved under transpose, so:
$$A \sim J \Rightarrow A^t \sim J^t$$

Combining these: $A \sim J \sim J^t \sim A^t$.
**Conclusion:** $\boxed{A \sim A^t}$.

### iii
Write $J_n(\lambda) = \lambda I + N$, where $N$ is the nilpotent matrix (0s on diagonal, 1s above).
Using Binomial Expansion for matrix powers:
$$J_n(\lambda)^N = \sum_{k=0}^{n-1} \binom{N}{k} \lambda^{N-k} N^k$$

Since $|\lambda| < 1$, as $N \to \infty$, the term $\lambda^{N-k}$ goes to 0 much faster than the binomial coefficient grows.
Specifically, for any fixed $k$: $\lim_{N \to \infty} \binom{N}{k} \lambda^{N-k} = 0$.

Since the sum has a finite number of terms ($n$ terms), the whole sum goes to 0.
$\boxed{\lim_{N \to \infty} J_n(\lambda)^N = 0}$.

### iv
From **Theorem 12.4**, $A = PJP^{-1}$.
Then $A^N = P J^N P^{-1}$.
$J^N$ is just the diagonal matrix of the powers of individual Jordan blocks:
$$J^N = \text{diag}(J_{k_1}(\lambda_1)^N, \dots, J_{k_m}(\lambda_m)^N)$$

Since all $|\lambda_i| < 1$, by part (iii), every block $J_{k_i}(\lambda_i)^N \to 0$.
So $J^N \to 0$.
Thus $\lim_{N \to \infty} A^N = P \cdot 0 \cdot P^{-1} = 0$.



## Problem 3

### i
We are given $Ae_1 = \sum a_{i1}e_i$.
Consider the polynomial $p(x) = a_{11} + a_{21}x + \dots + a_{n1}x^{n-1}$.
Evaluate at operator $N$ acting on $e_1$:
$$p(N)e_1 = a_{11}e_1 + a_{21}Ne_1 + a_{31}N^2e_1 + \dots$$

Since $N$ is the shift operator where $N^k e_1 = e_{k+1}$, this becomes:
$$= a_{11}e_1 + a_{21}e_2 + a_{31}e_3 + \dots + a_{n1}e_n$$

This matches $Ae_1$ exactly.

### ii
We know $e_k = N^{k-1}e_1$.
Since $N$ commutes with any polynomial of itself ($N p(N) = p(N) N$) and $AN=NA$:

$$Ae_k = A(N^{k-1}e_1) = N^{k-1}(Ae_1)$$
Substitute result from (i):
$$= N^{k-1}(p(N)e_1) = p(N)(N^{k-1}e_1) = p(N)e_k$$

### iii
We showed $Ae_k = p(N)e_k$ for every basis vector $e_1, \dots, e_n$.
Since linear operators are determined by their action on the basis, $A = p(N)$.



## Problem 4

### i
Let $A_k = \begin{pmatrix} 0 & 1 \\ 1/k & 0 \end{pmatrix}$.
The eigenvalues are $\pm \sqrt{1/k}$. Since $1/k \neq 0$, these are distinct, so $A_k$ is diagonalizable.
Limit as $k \to \infty$:
$$A_\infty = \begin{pmatrix} 0 & 1 \\ 0 & 0 \end{pmatrix}$$
This is a standard Jordan block for $\lambda=0$. It is not diagonalizable (rank 1, but requires rank 0 to be diagonal zero matrix).

### ii
Let $A_k = \begin{pmatrix} 1 & 1/k \\ 0 & 1 \end{pmatrix}$.
For any $k$, eigenvalues are both 1. $A_k - I \neq 0$, so geometric multiplicity is 1. Not diagonalizable.
Limit as $k \to \infty$:
$$A_\infty = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I$$
The identity matrix is diagonal (and thus diagonalizable).

### iii

We can approximate any matrix with a diagonalizable one by tweaking the eigenvalues slightly so they don't repeat.

Take any $A$. Write it in Jordan Form: $A = P J P^{-1}$.
Let $J$ have diagonal entries $\lambda_1, \dots, \lambda_n$. Some might be repeated.
Create a perturbed matrix $J_\epsilon$ by adding small distinct values to the diagonal:
$$J_\epsilon = J + \text{diag}(\epsilon, 2\epsilon, \dots, n\epsilon)$$

Now $J_\epsilon$ is upper triangular with diagonal entries $\lambda_i + i\epsilon$. For small enough $\epsilon$, these are all distinct.
Distinct eigenvalues $\Rightarrow$ Diagonalizable.

Define $A_\epsilon = P J_\epsilon P^{-1}$.
$A_\epsilon$ is diagonalizable, and as $\epsilon \to 0$, $A_\epsilon \to A$.
Thus, diagonalizable matrices are dense in $M_n(\mathbb{C})$.