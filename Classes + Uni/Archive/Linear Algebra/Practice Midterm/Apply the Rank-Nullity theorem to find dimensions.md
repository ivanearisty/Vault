# Apply the Rank–Nullity Theorem to Find Dimensions

## Rank–Nullity: The Tool

For a linear map $T:V\to W$ (finite-dimensional), the theorem is:

$$
\boxed{\dim V \;=\; \operatorname{rank}T \;+\; \operatorname{null}T}
$$

Where:
* $\operatorname{rank}T=\dim(\operatorname{im}T)$ (how many independent directions survive).
* $\operatorname{null}T=\dim(\ker T)$ (how many independent directions collapse to $0$).

---

## When to Use It (What It Answers Fast)

>[!tip] Quick Dimension Checks
* **Dimension of kernel** if you know (or can get) **rank**.
* **Dimension of image** if you know **nullity**.
* **Injective/surjective tests** when $\dim V$ and $\dim W$ are known.
* **Quick sanity check** on your RREF work: $n = \#\text{pivots} + \#\text{free vars}$ for an $m\times n$ matrix.

---

## Playbook (Pick the Branch that Fits Your Problem)

### A) Matrix Given ($A\in\mathbb{F}^{m\times n}$) (i.e., $T(x)=Ax$)

1.  Row-reduce to RREF.
2.  $\operatorname{rank} = \text{number of pivots}$.
3.  $\operatorname{nullity} = n - \operatorname{rank}$ (number of free variables).

**Conclude:**
* $\dim(\ker A) = \operatorname{nullity}$
* $\dim(\operatorname{im}A) = \operatorname{rank}$

**Quick Injectivity/Surjectivity (Finite-Dim):**
* If $n>m$, can’t be injective ($\operatorname{nullity} \ge n-m>0$).
* If $n<m$, can’t be surjective ($\operatorname{rank} \le n < m$).

### B) Abstract Map Given on a Basis ($\{v_j\}$)

1.  Form the matrix $[T]^{B}_{Y}$ whose $j$-th column is $[T(v_j)]_Y$.
2.  Apply branch **A** to that matrix.

### C) Only Properties Given (Injective/Surjective; Equal Dims)

* If $\dim V=\dim W=n$ and $T$ is injective $\Rightarrow \operatorname{rank} (=n)$, $\operatorname{nullity} (=0) \Rightarrow$ isomorphism.
* If $\dim V=\dim W=n$ and $T$ is surjective $\Rightarrow$ same conclusion.
* If $\dim V<\dim W$ and $T$ surjective is claimed $\Rightarrow$ impossible (contradiction by rank bound, $\operatorname{rank}T \le \dim V < \dim W$).

---

## Mini Examples

### 1) Rectangular Matrix (Quick)
Let
$$
A=\begin{bmatrix}
1&2&0\\
0&1&1
\end{bmatrix}\in\mathbb R^{2\times 3}
$$
RREF has 2 pivots $\Rightarrow \operatorname{rank}(=2)$.
$\operatorname{Nullity} (= 3-2=1)$.
So $\dim\ker A=1$, $\dim\operatorname{im}A=2$.

### 2) Abstract $\to$ Matrix
Let $V=\mathbb R^3$ and $W=\mathbb R^2$. Suppose
$$
T(v_1)=(1,0),\quad T(v_2)=(1,1),\quad T(v_3)=(2,1)
$$
Matrix (columns are images):
$$
A=\begin{bmatrix}1&1&2\\0&1&1\end{bmatrix}
$$
Row-reduce: 2 pivots $\Rightarrow \operatorname{rank} (=2)$, $\operatorname{nullity} (=3-2=1)$.
Thus $\dim\operatorname{im}T=2,\ \dim\ker T=1$.

### 3) Surjective Map Dimensions
$T:\mathbb R^5\to\mathbb R^3$ is surjective $\Rightarrow \operatorname{rank} (=3)$.
By rank–nullity: $5 = 3 + \operatorname{nullity} \Rightarrow \operatorname{nullity}=2$.

### 4) Injective Map Dimensions
$T:\mathbb R^4\to\mathbb R^6$ is injective $\Rightarrow \operatorname{nullity} (=0)$.
Hence $\operatorname{rank} (= \dim V = 4)$. (Not surjective since $\operatorname{rank} (4<\dim W=6)$.)

### 5) Function-Space Flavor (Dimension Reasoning)
$T:\mathcal P_4\to\mathcal P_3,\ T(p)=p'$.
$\dim \mathcal P_4=5$, $\dim \mathcal P_3=4$.
Kernel: $p'=0 \Rightarrow p$ constant $\Rightarrow \dim\ker T=1$.
Rank–nullity: $5= \operatorname{rank}T + 1 \Rightarrow \operatorname{rank}T=4$ (onto).

---

## Common Traps (and Fixes)

>[!warning] Common Mistakes
* **Using RREF columns as image basis**: Don’t. Use RREF only to find pivot indices, then take the **original columns** for an image basis.
* **Forgetting the domain dimension in rank–nullity**: The $n$ is $\dim V$ (or number of columns), not $\dim W$.
* **Assuming injective $\Rightarrow$ surjective without equal dimensions**: Only true when $\dim V=\dim W$.

---

## One-Check Question (Your Turn—One Line)

Let $A\in\mathbb R^{4\times 6}$ have $\operatorname{rank} (=3)$.
What are $\dim(\ker A)$ and $\dim(\operatorname{im}A)$?

$$\dim(\operatorname{im}A) = \operatorname{rank} = 3$$
$$\dim(\ker A) = \dim V - \operatorname{rank} = 6 - 3 = 3$$