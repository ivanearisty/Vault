For a linear map $T:V\to W$:
* **Kernel** $\ker T=\{v\in V:T(v)=0\}$: a subspace of $V$.
* **Image** $\operatorname{im} T=\{T(v):v\in V\}$: a subspace of $W$.

We want bases for each. (And often use Rank–Nullity: $\dim V=\dim\ker T+\dim\operatorname{im}T$.)

---

## A) Matrix Workflow (When $T(x)=Ax$)

Use this whenever vectors are in coordinates.

### Kernel Basis (Null Space)

1.  **Form** the homogeneous system $Ax=0$.
2.  **Row-reduce** $A$ to **RREF**.
3.  **Parameterize** the solution in terms of free variables.
4.  **Read off a basis**: one basis vector per free variable.

### Image Basis (Column Space)

1.  **Row-reduce** $A$ to **RREF**.
2.  **Identify pivot columns** (by index) in RREF.
3.  **Take the corresponding original columns of $A$**—they form a basis of $\operatorname{im}A$.

### Quick Checks

>[!tip] Dimension Checks
* $\dim\ker A = \#\text{free vars}$.
* $\dim\operatorname{im}A = \#\text{pivots}$.
* **Rank–Nullity**: $n=\operatorname{rank}+\operatorname{nullity}$ for $A\in\mathbb F^{m\times n}$.

---

## B) Abstract Workflow (When You Know $T$ on a Basis of $V$)

Use this if the problem says "Let $v_1,\dots,v_n$ be a basis of $V$, and $T(v_j)=\cdots$."

### Image Basis

1.  **Compute** $T(v_1),\dots,T(v_n)$.
2.  **Extract a basis** of their span (drop linear dependencies) by inspection or by putting their coordinate vectors into a matrix and using the matrix workflow (Branch A).

### Kernel Basis

1.  **Solve** $\sum_{j=1}^n c_j\,T(v_j)=0$ for scalars $c_j$. This gives a linear system for the $c_j$'s.
2.  Each independent solution $(c_1,\dots,c_n)$ gives a kernel vector $v_{\ker} = \sum_j c_j v_j$.
3.  Those vectors $v_{\ker}$ form a basis of $\ker T$.

>[!info] Matrix Shortcut
If you already formed the matrix $[T]^{B}_{Y}$ (columns $= [T(v_j)]_Y$), then you are back to the matrix workflow (Branch A) on that matrix.

---

## C) Worked Examples

### Example 1 (Matrix)

$$
A=\begin{bmatrix}1&2&3\\0&1&1\\1&1&2\end{bmatrix}
$$

**1. RREF:**
$$
A \xrightarrow{\text{RREF}} \begin{bmatrix}1&0&1\\0&1&1\\0&0&0\end{bmatrix}
$$
* **Pivot columns** are 1 and 2.
* Column 3 is a **free variable** column.

**2. Image Basis:**
Take the original pivot columns (1 and 2) from $A$:
$$
\text{Basis}(\operatorname{im}A) = \left\{\begin{bmatrix}1\\0\\1\end{bmatrix}, \begin{bmatrix}2\\1\\1\end{bmatrix}\right\}
$$
* $\dim(\operatorname{im}A) = 2$.

**3. Kernel Basis (from RREF system $Ax=0$):**
$$
\begin{aligned} x_1 + x_3 &= 0 \\ x_2 + x_3 &= 0 \end{aligned}
$$
Let $x_3 = t$ (free variable).
$$
\begin{aligned} x_1 &= -t \\ x_2 &= -t \\ x_3 &= t \end{aligned}
$$
Solution vector:
$$
\begin{bmatrix}x_1\\x_2\\x_3\end{bmatrix} = t \begin{bmatrix}-1\\-1\\1\end{bmatrix}
$$
$$
\text{Basis}(\ker A) = \left\{\begin{bmatrix}-1\\-1\\1\end{bmatrix}\right\}
$$
* $\dim(\ker A) = 1$.

**4. Check (Rank-Nullity):**
$n = \dim V = 3$.
$\operatorname{rank} + \operatorname{nullity} = 2 + 1 = 3$. This confirms the result.