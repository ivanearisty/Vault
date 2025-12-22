## Problem 1
### Part (i)

Show that $A \in M_{n\times n}(F)$ is invertible if and only if it has no 0 eigenvalue.

**Proof:**
1.  **Definition of Eigenvalue:** A scalar $\lambda \in F$ is an eigenvalue of $A$ if and only if $\det(\lambda I - A) = 0$
2.  If we test $\lambda = 0$, the condition becomes $\det(0I - A) = \det(-A) = (-1)^n \det(A) = 0$.
3. The constant term of the characteristic polynomial is determined by the determinant: $(-1)^n c_n(A) = p_A(0) = (-1)^n \det(A)$
4. A matrix $A$ is invertible if and only if $\det(A) \neq 0$.
5. Then:
    * If 0 is an eigenvalue, then $p_A(0) = 0$, implying $\det(A) = 0$, so $A$ is not invertible.
    * If $A$ is invertible, $\det(A) \neq 0$, implying $p_A(0) \neq 0$, so 0 is not an eigenvalue.
    * Thus, $A$ is invertible $\iff$ 0 is not an eigenvalue.

### Part (ii)

Let $A$ be invertible and $v$ be an eigenvector of $A$ with eigenvalue $\lambda$. Show $v$ is an eigenvector of $A^{-1}$ with eigenvalue $\lambda^{-1}$. Moreover, show $p_{A^{-1}}(x)=\frac{(-1)^{n}}{det(A)}x^{n}p_{A}(\frac{1}{x})$

**Proof regarding Eigenvectors:**
1.  We are given $Av = \lambda v$. Since $A$ is invertible, we know from Part (i) that $\lambda \neq 0$.
2.  Multiply both sides by $A^{-1}$:
    $$A^{-1}(Av) = A^{-1}(\lambda v)$$
3.  Simplify using associativity ($A^{-1}A = I$) and linearity:
    $$Iv = \lambda (A^{-1}v) \implies v = \lambda A^{-1}v$$
4.  Since $\lambda \neq 0$, we can divide by $\lambda$:
    $$A^{-1}v = \lambda^{-1}v$$
5.  Therefore, $v$ is an eigenvector of $A^{-1}$ corresponding to the eigenvalue $\lambda^{-1}$.

**Proof regarding Characteristic Polynomial:**
1.  We use the hint provided in the problem set: $xI-A^{-1}=-xA^{-1}(\frac{1}{x}I-A)$
2.  The characteristic polynomial of $A^{-1}$ is defined as $p_{A^{-1}}(x) = \det(xI - A^{-1})$
3.  Substitute the identity from the hint:
    $$p_{A^{-1}}(x) = \det\left(-xA^{-1}\left(\frac{1}{x}I - A\right)\right)$$
4.  Apply determinant properties $\det(XY) = \det(X)\det(Y)$ and $\det(cB) = c^n\det(B)$ for a scalar $c$ and $n \times n$ matrix $B$:
    $$p_{A^{-1}}(x) = \det(-x A^{-1}) \cdot \det\left(\frac{1}{x}I - A\right)$$
    $$p_{A^{-1}}(x) = (-x)^n \det(A^{-1}) \cdot p_A\left(\frac{1}{x}\right)$$
5.  Note $\det(A^{-1}) = \frac{1}{\det(A)}$ and $(-x)^n = (-1)^n x^n$.
6.  Then
    $$p_{A^{-1}}(x) = \frac{(-1)^n}{\det(A)} x^n p_A\left(\frac{1}{x}\right)$$

### Part (iii)

Let $A \in M_{n\times n}(F)$ be invertible. Show that $A$ is diagonalizable over $F$ if and only if $A^{-1}$ is diagonalizable over $F$.

**Proof:**
1.  **Definition:** A matrix is diagonalizable if there exists an invertible matrix $P$ and a diagonal matrix $D$ such that $D = P^{-1}AP$ Alternatively, $A$ is diagonalizable if and only if there exists a basis of $F^n$ consisting of eigenvectors of $A$
2.  **Forward Direction ($A \to A^{-1}$):**
    * Assume $A$ is diagonalizable. Then there exists a basis $\beta$ of eigenvectors $v_1, \dots, v_n$ with eigenvalues $\lambda_1, \dots, \lambda_n$.
    * From Part (ii), each eigenvector $v_i$ of $A$ is also an eigenvector of $A^{-1}$ with eigenvalue $\lambda_i^{-1}$ (valid since $A$ is invertible, so $\lambda_i \neq 0$).
    * Since the vectors $v_1, \dots, v_n$ form a basis for $F^n$, $A^{-1}$ possesses a basis of eigenvectors.
    * Therefore, $A^{-1}$ is diagonalizable.
3.  **Reverse Direction ($A^{-1} \to A$):**
    * Assume $A^{-1}$ is diagonalizable. Since $(A^{-1})^{-1} = A$, we can apply the same logic as above to show that $A$ is diagonalizable.
4.  **Conclusion:** $A$ is diagonalizable $\iff A^{-1}$ is diagonalizable.

## Problem 2

Show that there are no matrices $A, B \in M_{n\times n}(\mathbb{C})$ such that $AB - BA = I$

**Proof:**

1.  **Assumption:** Assume for the sake of contradiction that there exist matrices $A$ and $B$ such that their commutator equals the identity matrix:
    $$AB - BA = I$$

2.  **Apply Trace:** Take the trace ($tr$) of both sides of the equation.
    $$tr(AB - BA) = tr(I)$$

3.  **Evaluate the Left-Hand Side (LHS):**
    * According to **Proposition 10.6 (i)**, the trace function is linear. Therefore:
        $$tr(AB - BA) = tr(AB) - tr(BA)$$
	- For any $A, B \in M_{n\times n}(F)$, the trace of the product is commutative, meaning $tr(AB) = tr(BA)$
    * Substituting this back into the equation:
        $$tr(AB) - tr(AB) = 0$$
    * Thus, the trace of the commutator is always 0.

4.  **Evaluate the Right-Hand Side (RHS):**
    * The trace of the identity matrix $I$ is the sum of its diagonal entries. Since $I$ has ones on the diagonal and is size $n \times n$:
        $$tr(I) = 1 + 1 + \dots + 1 = n$$

5.  **Contradiction:**
    * Equating the LHS and RHS gives us:
        $$0 = n$$
    * Since we are working in finite dimensions ($n \times n$ matrices), we assume $n \geq 1$. Therefore, $0 = n$ is a contradiction.

There exist no matrices $A, B \in M_{n\times n}(\mathbb{C})$ such that $[A,B] = I$.

## Problem 3

### Part (i)

Show that if $A = \text{diag}(\lambda_1, \dots, \lambda_n)$ and $f(x) \in F[x]$, then $f(A) = \text{diag}(f(\lambda_1), \dots, f(\lambda_n))$

**Proof:**
1.  Let the polynomial be $f(x) = a_m x^m + a_{m-1} x^{m-1} + \dots + a_1 x + a_0 I$
2.  Consider the power $A^k$ for a diagonal matrix $A = \text{diag}(\lambda_1, \dots, \lambda_n)$. By the properties of matrix multiplication, raising a diagonal matrix to a power $k$ results in raising each diagonal entry to that power:
    $$A^k = \text{diag}(\lambda_1^k, \dots, \lambda_n^k)$$
3.  By the definition of $f(A)$, we substitute $A$ into the polynomial:
    $$f(A) = \sum_{k=0}^{m} a_k A^k = \sum_{k=0}^{m} a_k \text{diag}(\lambda_1^k, \dots, \lambda_n^k)$$
4.  Using the linearity of matrix addition and scalar multiplication:
    $$f(A) = \text{diag}\left(\sum_{k=0}^{m} a_k \lambda_1^k, \dots, \sum_{k=0}^{m} a_k \lambda_n^k\right)$$
5.  The term $\sum_{k=0}^{m} a_k \lambda_i^k$ is exactly the definition of evaluating the scalar polynomial $f(x)$ at $x = \lambda_i$.
6.  Therefore, $f(A) = \text{diag}(f(\lambda_1), \dots, f(\lambda_n))$.


### Part (ii)

Show that if $A \in M_{n \times n}(F)$ is diagonalizable over $F$, then $f(A)$ is also diagonalizable over $F$

**Proof:**
1.  Since $A$ is diagonalizable, there exists an invertible matrix $P$ and a diagonal matrix $D$ such that $A = P D P^{-1}$.
2.  From **Lemma 10.15**, if $A$ is similar to $D$ via $P$, then $f(A)$ is similar to $f(D)$ via the same $P$:
    $$f(A) = f(P D P^{-1}) = P f(D) P^{-1}$$
3.  From Part (i) proven above, since $D$ is diagonal, the matrix $f(D)$ is also a diagonal matrix.
4.  Therefore, $f(A)$ is similar to a diagonal matrix $f(D)$, which means $f(A)$ is diagonalizable.

### Part (iii)

Let $A = \begin{pmatrix} 5 & -3 \\ 6 & -4 \end{pmatrix}$ and $f(x) = x^{2025} + x$. 
Compute $f(A)$, $\text{tr}(f(A))$, and $\det(f(A))$.

First, we find the eigenvalues by solving the characteristic equation $\det(A - \lambda I) = 0$.
$$\det \begin{pmatrix} 5 - \lambda & -3 \\ 6 & -4 - \lambda \end{pmatrix} = (5 - \lambda)(-4 - \lambda) - (-18)$$
$$= \lambda^2 - \lambda - 20 + 18 = \lambda^2 - \lambda - 2 = (\lambda - 2)(\lambda + 1)$$
The eigenvalues are $\lambda_1 = 2$ and $\lambda_2 = -1$.

Next, find the eigenvectors:
* **For $\lambda_1 = 2$:**
    $$(A - 2I)v = \begin{pmatrix} 3 & -3 \\ 6 & -6 \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix} \implies x = y$$
    Eigenvector $v_1 = \begin{pmatrix} 1 \\ 1 \end{pmatrix}$.
* **For $\lambda_2 = -1$:**
    $$(A + I)v = \begin{pmatrix} 6 & -3 \\ 6 & -3 \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix} \implies 2x = y$$
    Eigenvector $v_2 = \begin{pmatrix} 1 \\ 2 \end{pmatrix}$.

Form the matrices $P$ and $D$:
$$P = \begin{pmatrix} 1 & 1 \\ 1 & 2 \end{pmatrix}, \quad D = \begin{pmatrix} 2 & 0 \\ 0 & -1 \end{pmatrix}$$
Calculate $P^{-1}$ ($\det(P) = 2-1 = 1$):
$$P^{-1} = \begin{pmatrix} 2 & -1 \\ -1 & 1 \end{pmatrix}$$

**Compute $f(D)$**
Using Part (i), $f(D) = \text{diag}(f(2), f(-1))$.
* $f(2) = 2^{2025} + 2$
* $f(-1) = (-1)^{2025} + (-1) = -1 - 1 = -2$ (since 2025 is odd).
$$f(D) = \begin{pmatrix} 2^{2025} + 2 & 0 \\ 0 & -2 \end{pmatrix}$$
Let $\alpha = 2^{2025} + 2$ and $\beta = -2$.

**Compute $f(A)$**
$$f(A) = P f(D) P^{-1} = \begin{pmatrix} 1 & 1 \\ 1 & 2 \end{pmatrix} \begin{pmatrix} \alpha & 0 \\ 0 & \beta \end{pmatrix} \begin{pmatrix} 2 & -1 \\ -1 & 1 \end{pmatrix}$$
$$= \begin{pmatrix} \alpha & \beta \\ \alpha & 2\beta \end{pmatrix} \begin{pmatrix} 2 & -1 \\ -1 & 1 \end{pmatrix} = \begin{pmatrix} 2\alpha - \beta & -\alpha + \beta \\ 2\alpha - 2\beta & -\alpha + 2\beta \end{pmatrix}$$

Substituting $\alpha = 2^{2025} + 2$ and $\beta = -2$:
* Entry 1,1: $2(2^{2025} + 2) - (-2) = 2^{2026} + 4 + 2 = 2^{2026} + 6$
* Entry 1,2: $-(2^{2025} + 2) + (-2) = -2^{2025} - 4$
* Entry 2,1: $2(2^{2025} + 2) - 2(-2) = 2^{2026} + 4 + 4 = 2^{2026} + 8$
* Entry 2,2: $-(2^{2025} + 2) + 2(-2) = -2^{2025} - 6$

$$f(A) = \begin{pmatrix} 2^{2026} + 6 & -2^{2025} - 4 \\ 2^{2026} + 8 & -2^{2025} - 6 \end{pmatrix}$$

**Compute Trace and Determinant**
Note that trace and determinant are similarity invariants, so $\text{tr}(f(A)) = \text{tr}(f(D))$ and $\det(f(A)) = \det(f(D))$.

* **Trace:**
    $$\text{tr}(f(A)) = f(2) + f(-1) = (2^{2025} + 2) + (-2) = 2^{2025}$$

* **Determinant:**
    $$\det(f(A)) = f(2) \cdot f(-1) = (2^{2025} + 2)(-2) = -2^{2026} - 4$$

## Problem 4

Let $\lambda_1, ..., \lambda_m \in \mathbb{R}$ be distinct real numbers. Let $V = \text{span}(e^{\lambda_1 x}, ..., e^{\lambda_m x})$ be a subspace of $C(\mathbb{R})$. Let $D: V \rightarrow C(\mathbb{R})$ be the linear transformation $D(f) = f'$.

### Part (i)

Show that $\text{im } D \subset V$.

**Proof:**
1.  Let $f$ be any arbitrary element in $V$. By definition of the span, $f$ can be written as a linear combination of the spanning vectors:
    $$f(x) = c_1 e^{\lambda_1 x} + c_2 e^{\lambda_2 x} + \dots + c_m e^{\lambda_m x}$$
    where $c_1, \dots, c_m \in \mathbb{R}$.
2.  Apply the transformation $D$ to $f$:
    $$D(f) = \frac{d}{dx}\left( \sum_{i=1}^{m} c_i e^{\lambda_i x} \right)$$
3.  By the linearity of the derivative operator:
    $$D(f) = \sum_{i=1}^{m} c_i \frac{d}{dx}(e^{\lambda_i x})$$
4.  $\frac{d}{dx}(e^{\lambda_i x}) = \lambda_i e^{\lambda_i}$
    $$D(f) = \sum_{i=1}^{m} c_i \lambda_i e^{\lambda_i x}$$
    $$D(f) = (c_1 \lambda_1) e^{\lambda_1 x} + \dots + (c_m \lambda_m) e^{\lambda_m x}$$
5.  Let $k_i = c_i \lambda_i$. Then $D(f) = \sum_{i=1}^{m} k_i e^{\lambda_i x}$.
6.  This result is a linear combination of the basis vectors $\{e^{\lambda_1 x}, \dots, e^{\lambda_m x}\}$, which means $D(f) \in \text{span}(e^{\lambda_1 x}, ..., e^{\lambda_m x})$.
7.  Since $D(f) \in V$ for all $f \in V$, we conclude that $\text{im } D \subset V$.

### Part (ii)

Show that $D(e^{\lambda_i x}) = \lambda_i e^{\lambda_i x}$ for $i=1, ..., m$.

**Proof:**
1.  Let $v_i = e^{\lambda_i x}$.
2.  Apply $D$ to $v_i$:
    $$D(v_i) = \frac{d}{dx}(e^{\lambda_i x})$$
3.  By the chain rule, the derivative is $\lambda_i e^{\lambda_i x}$.
4.  Therefore, $D(e^{\lambda_i x}) = \lambda_i e^{\lambda_i x}$.
5.  This calculation confirms that each function $e^{\lambda_i x}$ is an eigenvector of the linear operator $D$ with the corresponding eigenvalue $\lambda_i$.

### Part (iii)

Show that $e^{\lambda_1 x}, ..., e^{\lambda_m x}$ are linearly independent.

**Proof:**
1.  From Part (ii), we established that the functions $v_1 = e^{\lambda_1 x}, \dots, v_m = e^{\lambda_m x}$ are eigenvectors of the linear operator $D: V \rightarrow V$.
2.  The corresponding eigenvalues are $\lambda_1, \dots, \lambda_m$.
3.  The problem states that $\lambda_1, \dots, \lambda_m$ are distinct real numbers.
4.  We apply **Theorem 10.9** from Lecture 10, which states: "Let $\lambda_1, ..., \lambda_m$ be distinct eigenvalues of \[a linear operator]... Let $v_i$ be an eigenvector corresponding to $\lambda_i$... Then $v_1, ..., v_m$ are linearly independent".
5.  Since our vectors $e^{\lambda_i x}$ are eigenvectors corresponding to distinct eigenvalues $\lambda_i$, they must be linearly independent.