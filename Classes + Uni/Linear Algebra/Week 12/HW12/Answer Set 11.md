## Problem 1

Notes: 

|**Feature**|**Characteristic Polynomial pA​(x)**|**Minimal Polynomial mA​(x)**|
|---|---|---|
|**How to find it**|Calculate determinant $\det(xI - A)$.|Test factors of $p_A(x)$.|
|**Roots**|The roots are all eigenvalues.|The roots are **exactly the same** eigenvalues.|
|**Multiplicity**|Roots appear $k$ times (algebraic multiplicity).|Roots appear $l$ times, where $1 \le l \le k$.|
|**Theorem**|**Cayley-Hamilton Theorem:** Every matrix satisfies its characteristic polynomial ($p_A(A)=0$).|It divides the characteristic polynomial ($m_A(x)$ divides $p_A(x)$).

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

**$m_C(x) = (x-2)^2$

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

