## Problem 1

![[Screenshot 2025-11-09 at 6.25.29 PM.png]]

$$
\begin{gather}
tI = \begin{pmatrix}
t & 0 & 0 & \dots & 0  \\
0 & t & 0 & \dots & 0  \\
0 & 0 & t & \dots & 0  \\
0 & 0 & 0 & \dots & t 
\end{pmatrix}
\end{gather}
$$
$$
\begin{gather}
M = A + tI = \begin{pmatrix}
t & 0 & 0 & \dots & a_{0}  \\
-1 & t & 0 & \dots & a_{1}  \\
\vdots & \vdots & \vdots & \vdots &\vdots & \\
0 & -1 & t & \dots & a_{n-2}  \\
0 & 0 & -1 & \dots & t a_{n-1}
\end{pmatrix}
\end{gather}
$$
$$
\begin{gather}
\det(A + tI) = t \cdot -1^{1+1} \det M_{11} + -1^{1+n} \cdot a_{0} \det(M_{1n}) \\
\det(A + tI) = t \det M_{11} + -1^{1+n} \cdot a_{0} \det(M_{1n}) \\ 
\\ \text{Deleting the first row and column gives the same pattern as the original,} \\
\text{but with size } (n-1)\times (n-1) \\
\\
D_{n-1}(t) = \det(M_{11}) \\
\\
\text{Deleting the first row and last column gives us a minor that is upper triangular.} \\
\text{It's determinant is } -1^{n-1} \\
\text{Thererefore we have } -1^{1+n} \cdot a_{0} -1^{1-n} = -1^{(1+n)+(n-1)} \cdot a_{0} = -1^{2n} \cdot a_{0} \\
\text{Since 2n is always positive we just get } a_{0} \\ \\

D_{n}(t) = t \cdot D_{n-1}(t) + a_{0} \\
\\
\text{With the base case } D_{1}(t) = t + a_{0} \\
\text{this recurrence will unroll to } t^{n} + a_{n-1}t^{n-1} + a_{n-2}t^{n-2} + \dots a_{1}t + a_{0}
\end{gather} 
$$

## Problem 2
![[Screenshot 2025-11-09 at 7.07.08 PM.png]]
### (i)
$$
\begin{gather}
\text{If } (A^m=0) \text{ for some } (m>0), \text{ then} \\
0=\det(A^m)=\det(A)^m. \\
\text{Over a field, the only scalar whose m-th power is 0 is 0. Hence }  \\\det(A)=0.
\end{gather}
$$

### (ii)

 Scaling one row by c scales the determinant by c. Scaling **all n rows** multiplies by $c^{n}$.

### (iii)

$\det(A)=\det(A^t)=\det(-A)=(-1)^{2n+1}\det(A)=-\det(A).$
Thus $2\det(A)=0\Rightarrow \det(A)=0$
### (iv)
$1=\det(I)=\det(AA^t)=\det(A)\det(A^t)=\det(A)^2.$
### (v)
$$
\begin{gather}
\text{Two identities:} \\
1. \text{Since } A \text{ is orthogonal }, A^tA=I \\
   A^t-I=A^t-A^tA=A^t(I-A) \\
   \text{Taking determinants:} \\
   \det(A^t-I)=\det(A^t)\det(I-A)=\det(A)\det(I-A)=\det(I-A) \\
   \text{because } \det(A)=1 \text{ and } \det(A^t)=\det(A). \\
\\
2. \text{Also } (I-A)^t=I-A^t, \text{ hence} \\
   A^t-I=-(I-A^t)=-(I-A)^t \\
   \text{so, for} 3\times3 \text{ matrices}, \\
   \det(A^t-I)=\det!\big(- (I-A)^t\big)=(-1)^3\det!\big((I-A)^t\big)=-\det(I-A) \\
\\
\text{Combine the two expressions for } \det(A^t-I) \\
\det(I-A)=-\det(I-A) \Rightarrow 2\det(I-A)=0 \Rightarrow \boxed{\det(I-A)=0.}
\end{gather}
$$


## Problem 3

### $\Leftarrow$ If $\det(A)=\pm1$, then $A^{-1}$ has integer entries

Cofactor/adjugate formula:

$$A^{-1}=\frac{1}{\det(A)}\operatorname{adj}(A)$$

where $\operatorname{adj}(A)$ is the transpose of the cofactor matrix.

Each cofactor is $(\pm)\det(\text{integer minor})$, hence integer. So $\operatorname{adj}(A)\in M_{n\times n}(\mathbb{Z})$.

If $\det(A)=\pm1$, dividing by $\pm1$ doesn’t change integrality, thus $A^{-1}\in M_{n\times n}(\mathbb{Z})$.

### $\Rightarrow$ If $A^{-1}$ has integer entries, then $\det(A)=\pm1$

Take determinants:

$$\det(A^{-1})=\frac{1}{\det(A)}$$

Since $A$ has integer entries, $\det(A)\in\mathbb{Z}$. Since $A^{-1}$ has integer entries, $\det(A^{-1})\in\mathbb{Z}$ too.

Thus $1/\det(A)\in\mathbb{Z}$. The only integers whose reciprocals are integers are $\pm1$. Hence $\det(A)=\pm1$.
## Problem 4
![[Screenshot 2025-11-09 at 10.26.22 PM.png]]
### (i)
#### 1
$$
 \\ \mathbf{1}\mathbf{1}^t = \begin{pmatrix} 1 \\ 1 \\ \vdots \\ 1 \end{pmatrix} \begin{pmatrix} 1 & 1 & \cdots & 1 \end{pmatrix} = \begin{pmatrix} 1 & 1 & \cdots & 1 \\ 1 & 1 & \cdots & 1 \\ \vdots & \vdots & \ddots & \vdots \\ 1 & 1 & \cdots & 1 \end{pmatrix} = J\_n
$$
$$
 \\ I\_n + J\_n = \begin{pmatrix} 1 & 0 & \cdots & 0 \\ 0 & 1 & \cdots & 0 \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \cdots & 1 \end{pmatrix} + \begin{pmatrix} 1 & 1 & \cdots & 1 \\ 1 & 1 & \cdots & 1 \\ \vdots & \vdots & \ddots & \vdots \\ 1 & 1 & \cdots & 1 \end{pmatrix} = \begin{pmatrix} 2 & 1 & \cdots & 1 \\ 1 & 2 & \cdots & 1 \\ \vdots & \vdots & \ddots & \vdots \\ 1 & 1 & \cdots & 2 \end{pmatrix}
$$
#### 2 
$A^{-1} = I_n^{-1} = I_n$.
$v^t A^{-1} u = \mathbf{1}^t I_n \mathbf{1} = \mathbf{1}^t \mathbf{1}$.
This is the dot product of $\mathbf{1}$ with itself:
$$ \\ \mathbf{1}^t \mathbf{1} = \begin{pmatrix} 1 & 1 & \cdots & 1 \end{pmatrix} \begin{pmatrix} 1 \\ 1 \\ \vdots \\ 1 \end{pmatrix} = (1 \cdot 1) + (1 \cdot 1) + \cdots + (1 \cdot 1) \text{ (n times)} = \sum_{i=1}^n 1 = n$$

### (ii)

$$R_2 \leftarrow R_2 - u \cdot R_1$$
$$\begin{pmatrix} 1 & -v^t \\ 0 & A+uv^t \end{pmatrix}$$
This kind of row operation does not change the determinant. So, $\det(M)$ is the same as the determinant of this new, cleaner matrix.

Because it's block upper-triangular, the determinant is just the product of the determinants of the blocks on the diagonal:
$$\det(M) = \det(1) \cdot \det(A+uv^t) = \det(A+uv^t)$$

### (iii)

Let's call $C_1 = \begin{pmatrix} 1 \\ u \end{pmatrix}$ and the second column block $C_2 = \begin{pmatrix} -v^t \\ A \end{pmatrix}$.


$C_1 \leftarrow C_1 - C_{2} \cdot (A^{-1}u)$
$$\begin{pmatrix} 1+v^t A^{-1}u & -v^t \\ 0 & A \end{pmatrix}$$
Just like our row operation, this column operation does not change the determinant.

This matrix is *also* block upper-triangular. So, we calculate its determinant the same way:
$$\det(M) = \det(1+v^t A^{-1}u) \cdot \det(A)$$
Since $1+v^t A^{-1}u$ is just a single number (a $1 \times 1$ matrix), its determinant is just itself.
### (iv)

From the previous problem, we have this formula:
$$\det(A+uv^t) = \det(A) \big(1+v^t A^{-1}u\big)$$

How does $I_n + J$ fit this?
* Let $\mathbf{1}$ be the column vector of all ones: $\begin{pmatrix} 1 \\ \vdots \\ 1 \end{pmatrix}$.
* Then $\mathbf{1}\mathbf{1}^t$ (column times row) is $\begin{pmatrix} 1 \\ \vdots \\ 1 \end{pmatrix} \begin{pmatrix} 1 & \cdots & 1 \end{pmatrix}$, which is the all-ones matrix, $J$.

So, we're trying to find $\det(I_n + \mathbf{1}\mathbf{1}^t)$.
* $A = I_n$
* $u = \mathbf{1}$
* $v = \mathbf{1}$

$$\det(I_n + \mathbf{1}\mathbf{1}^t) = \det(I_n) \cdot \big(1 + \mathbf{1}^t (I_n)^{-1} \mathbf{1}\big)$$
$$= 1 \cdot \big(1 + \mathbf{1}^t I_n \mathbf{1}\big)$$
$$= 1 + \mathbf{1}^t \mathbf{1}$$

$$\begin{pmatrix} 1 & \cdots & 1 \end{pmatrix} \begin{pmatrix} 1 \\ \vdots \\ 1 \end{pmatrix} = (1\cdot1 + 1\cdot1 + \cdots + 1\cdot1)$$

Since there are $n$ ones, this is just $n$.

The whole thing equals $1 + n$.

### (v)
#### .1

$$(A+uv^t)^{-1} = A^{-1} - \frac{A^{-1}u v^t A^{-1}}{1+v^t A^{-1}u}$$

Let $\alpha = v^t A^{-1} u$
$B = A^{-1} - \frac{1}{1+\alpha} A^{-1}u v^t A^{-1}$.

$$(A+uv^t) \cdot \left( A^{-1} - \frac{1}{1+\alpha} A^{-1}u v^t A^{-1} \right)$$

Distribute this out:
1.  $A \cdot A^{-1} = \mathbf{I}$
2.  $A \cdot \left( - \frac{1}{1+\alpha} A^{-1}u v^t A^{-1} \right) = - \frac{1}{1+\alpha} (A A^{-1}) u v^t A^{-1} = \mathbf{- \frac{1}{1+\alpha} u v^t A^{-1}}$
3.  $uv^t \cdot A^{-1} = \mathbf{+ uv^t A^{-1}}$
4.  $uv^t \cdot \left( - \frac{1}{1+\alpha} A^{-1}u v^t A^{-1} \right) = - \frac{1}{1+\alpha} u (v^t A^{-1} u) v^t A^{-1}$
    * Notice that $v^t A^{-1} u$ is just our $\alpha$
    * So this term is $\mathbf{- \frac{\alpha}{1+\alpha} u v^t A^{-1}}$


All three messy terms have $uv^t A^{-1}$ in them. Let's factor it out:
$$\mathbf{I} \quad + \quad \left[ - \frac{1}{1+\alpha} + 1 - \frac{\alpha}{1+\alpha} \right] (u v^t A^{-1})$$
$$\left[ \frac{-1}{1+\alpha} + \frac{1+\alpha}{1+\alpha} - \frac{\alpha}{1+\alpha} \right]$$
$$\left[ \frac{-1 + (1+\alpha) - \alpha}{1+\alpha} \right]$$
$$\left[ \frac{-1 + 1 + \alpha - \alpha}{1+\alpha} \right] = \left[ \frac{0}{1+\alpha} \right] = \mathbf{0}$$
So, the entire middle part just cancels out to zero

All we're left with is $I + 0 \cdot (u v^t A^{-1}) = \mathbf{I}$.
Since $(A+uv^t) \cdot B = I$, we're done.

#### .2

We had two *different* ways to factor our original $M$ matrix:
- ***From (ii):** $M = \begin{pmatrix}1&0\\u&I\end{pmatrix} \begin{pmatrix}1&-v^t\\0&A+uv^t\end{pmatrix}$
- ***From (iii):** $M = \begin{pmatrix}1+\alpha&-v^t\\0&A\end{pmatrix} \begin{pmatrix}1&0\\A^{-1}u&I\end{pmatrix}$ (where $\alpha = v^tA^{-1}u$)

Since $M = M$, let's find $M^{-1}$ both ways. The answers *must* be the same. We just need to look at the bottom-right corner.

**Inverse, Way 1:** We invert the factorization from (ii).
- $M^{-1} = \begin{pmatrix}1&-v^t\\0&A+uv^t\end{pmatrix}^{-1} \begin{pmatrix}1&0\\u&I\end{pmatrix}^{-1}$
- The bottom-right corner of $M^{-1}$ just ends up being the inverse of the bottom-right block, which is **$(A+uv^t)^{-1}$**.

**Inverse, Way 2:** We invert the factorization from (iii).
- $M^{-1} = \begin{pmatrix}1&0\\A^{-1}u&I\end{pmatrix}^{-1} \begin{pmatrix}1+\alpha&-v^t\\0&A\end{pmatrix}^{-1}$
- The bottom-right corner of *this* $M^{-1}$ is **$A^{-1} - \frac{A^{-1}u v^t A^{-1}}{1+\alpha}$**.


The bottom-right corner of $M^{-1}$ must be equal to itself. Therefore:    $$(A+uv^t)^{-1} = A^{-1} - \frac{A^{-1}u v^t A^{-1}}{1+v^t A^{-1}u}$$
