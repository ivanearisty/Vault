## Problem 1

The vectors are

$$
\begin{gather}
p_{1} = \{ 1,2,0 \} \\
p_{2} = \{ 1,1,1 \} \\
p_{2} = \{ 1,0,5 \}
\end{gather}
$$

Match coefficients of powers of (x):
$$
\begin{gather}
(x^3: a + c = 0) \\
(x^2: b = 0) \\
(x^1: 2a + b = 0) \\
\text{constant}: (b + 5c = 0)
\end{gather}
$$
From $(b=0)$, the $(x^1)$ equation gives $(2a=0\Rightarrow a=0)$. Then $(a+c=0\Rightarrow c=0)$, and the constant equation is satisfied automatically.

Thus the only solution is (a=b=c=0). Therefore it is **linearly independent**.

## Problem 2

$$
\begin{gather}
\textbf{Compute } \det P. \\
\quad \text{Expand along the 3rd column (two zeros):} \\
\det P
= 1\cdot \\
\det\begin{pmatrix}1 & k \\ k & 1\end{pmatrix}
= (1)(1) - (k)(k) \\
= 1 - k^2.
\end{gather}
$$
$$
\begin{gather}
1 - k^2 = 0 
\;\Longleftrightarrow\;
k^2 = 1
\;\Longleftrightarrow\;
k = \pm 1.
\end{gather}
$$

$$
\begin{gather}
P =
\begin{pmatrix}
1 & k & 0 \\
k & 1 & 0 \\
0 & 1 & 1
\end{pmatrix}, \\[8pt]
P^{-1} =
\begin{pmatrix}
-\dfrac{1}{k^2 - 1} & \dfrac{k}{k^2 - 1} & 0 \\[6pt]
\dfrac{k}{k^2 - 1} & -\dfrac{1}{k^2 - 1} & 0 \\[6pt]
-\dfrac{k}{k^2 - 1} & \dfrac{1}{k^2 - 1} & 1
\end{pmatrix}, \\[12pt]
P^{-1} P =
\begin{pmatrix}
-\dfrac{1}{k^2 - 1} & \dfrac{k}{k^2 - 1} & 0 \\[6pt]
\dfrac{k}{k^2 - 1} & -\dfrac{1}{k^2 - 1} & 0 \\[6pt]
-\dfrac{k}{k^2 - 1} & \dfrac{1}{k^2 - 1} & 1
\end{pmatrix}
\begin{pmatrix}
1 & k & 0 \\
k & 1 & 0 \\
0 & 1 & 1
\end{pmatrix}
=
\begin{pmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1
\end{pmatrix}
= I_3.
\end{gather}
$$

## Problem 3

### 1

For the zero argument:
$$
\omega(v_1,\dots,0,\dots,v_m)
= 0\cdot\omega(v_1,\dots,1,\dots,v_m)
= 0.
$$

For the linear combination:
$$
\omega(v_1,\dots,v_j+\sum_{i\ne j}\lambda_i v_i,\dots,v_m)
= \omega(v_1,\dots,v_j,\dots,v_m)
+ \sum_{i\ne j}\lambda_i\,\omega(v_1,\dots,v_i,\dots,v_m).
$$
Each term in the sum has a repeated vector (since $v_i$ appears twice),  
so by the alternating property they are $0$.  
Hence only the first term remains.

### 2

If the vectors are linearly dependent, one can be written as  
$$
v_j = \sum_{i\ne j}\lambda_i v_i.
$$
Then
$$
\omega(v_1,\dots,v_j,\dots,v_m)
= \sum_{i\ne j}\lambda_i\,\omega(v_1,\dots,v_i,\dots,v_m).
$$
Each sum has two identical vectors $\Rightarrow 0$.  
Therefore $\omega(v_1,\dots,v_m)=0$.

### 3

Any $m$ vectors in $V$ with $m>n$ are linearly dependent.  
By part (ii), $\omega(v_1,\dots,v_m)=0$.  
Hence $\bigwedge^m V^* = \{0\}$.

## Problem 4

### 1

$$
B=
\begin{pmatrix}
1 & 0 & 1\\
1 & 1 & 2\\
0 & 1 & 1
\end{pmatrix}
\ \xrightarrow{\,R_2\leftarrow R_2-R_1\,}\
\begin{pmatrix}
1 & 0 & 1\\
0 & 1 & 1\\
0 & 1 & 1
\end{pmatrix}
\ \xrightarrow{\,R_3\leftarrow R_3-R_2\,}\
\begin{pmatrix}
1 & 0 & 1\\
0 & 1 & 1\\
0 & 0 & 0
\end{pmatrix}
= \mathrm{RREF}(B).
$$

$$
\text{Let } x=(x_1,x_2,x_3)^\top.\quad
\begin{cases}
x_1 + x_3 = 0\\
x_2 + x_3 = 0
\end{cases}
\Rightarrow
x_1=-x_3,\; x_2=-x_3,\; \text{free: } x_3=t.
$$

$$
\text{All solutions: } x=t\,(-1,-1,1),\quad t\in\mathbb{R}.
\quad\text{Thus } \mathrm{Null}(B)=\operatorname{span}\{(-1,-1,1)\}.
$$


### 2

A quick consistency test uses a left-null vector of $A$ (i.e., $w\neq 0$ with $w^\top A=0$).
Since $w\in\mathrm{Null}(A^\top)=\mathrm{Null}(B)$, take $w=(-1,-1,1)$.  
Then a necessary condition for solvability is $w^\top b=0$:
$$
w^\top b = (-1,-1,1)\cdot (1,2,5) = -1-2+5 = 2\neq 0.
$$
Therefore
$$
\text{the system } Ax=\begin{pmatrix}1\\2\\5\end{pmatrix}\ \text{is inconsistent (no solution).}
$$

### 3

$C = [A | B] = \left( \begin{array}{ccc|ccc} 1 & 1 & 0 & 1 & 0 & 1 \\ 0 & 1 & 1 & 1 & 1 & 2 \\ 1 & 2 & 1 & 0 & 1 & 1 \end{array} \right)$

We solve $Cv = 0$:
$$\left( \begin{array}{cccccc|c} 1 & 1 & 0 & 1 & 0 & 1 & 0 \\ 0 & 1 & 1 & 1 & 1 & 2 & 0 \\ 1 & 2 & 1 & 0 & 1 & 1 & 0 \end{array} \right)$$
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

The variables are $v = (x_1, x_2, x_3, -y_1, -y_2, -y_3)^t$.
* **Pivot columns:** 1, 2, 4 (corresponding to $x_1, x_2, -y_1$).
* **Free columns:** 3, 5, 6 (corresponding to $x_3, -y_2, -y_3$).

Let $x_3 = s$, $-y_2 = t$, and $-y_3 = u$, where $s, t, u \in \mathbb{R}$.
The equations are:
* $x_1 - x_3 - (-y_2) - (-y_3) = 0 \implies x_1 - s - t - u = 0 \implies x_1 = s + t + u$
* $x_2 + x_3 + (-y_2) + (-y_3) = 0 \implies x_2 + s + t + u = 0 \implies x_2 = -s - t - u$
* $(-y_1) + (-y_3) = 0 \implies -y_1 + u = 0 \implies -y_1 = -u$

From the variables, we have:
* $x_1 = s + t + u$
* $x_2 = -s - t - u$
* $x_3 = s$

And for $y$:
* $-y_1 = -u \implies y_1 = u$
* $-y_2 = t \implies y_2 = -t$
* $-y_3 = u \implies y_3 = -u$

The solutions are all pairs of vectors $(x, y)$ of the form:
$$x = \begin{pmatrix} s + t + u \\ -s - t - u \\ s \end{pmatrix} \quad \text{and} \quad y = \begin{pmatrix} u \\ -t \\ -u \end{pmatrix}$$
for any scalars $s, t, u \in \mathbb{R}$.

We can also express this as a linear combination:
$x = s\begin{pmatrix} 1 \\ -1 \\ 1 \end{pmatrix} + t\begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix} + u\begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix}$
$y = s\begin{pmatrix} 0 \\ 0 \\ 0 \end{pmatrix} + t\begin{pmatrix} 0 \\ -1 \\ 0 \end{pmatrix} + u\begin{pmatrix} 1 \\ 0 \\ -1 \end{pmatrix}$
### 4

$x = \begin{pmatrix} s + t + u \\ -s - t - u \\ s \end{pmatrix}$
$$z = Ax = \begin{pmatrix} 1 & 1 & 0 \\ 0 & 1 & 1 \\ 1 & 2 & 1 \end{pmatrix} \begin{pmatrix} s + t + u \\ -s - t - u \\ s \end{pmatrix}$$
$$z = \begin{pmatrix} 1(s + t + u) + 1(-s - t - u) + 0(s) \\ 0(s + t + u) + 1(-s - t - u) + 1(s) \\ 1(s + t + u) + 2(-s - t - u) + 1(s) \end{pmatrix}$$
$$z = \begin{pmatrix} (s+t+u) - (s+t+u) \\ (-s-t-u) + s \\ (s+t+u) - (2s+2t+2u) + s \end{pmatrix}$$
$$z = \begin{pmatrix} 0 \\ -t - u \\ (s-2s+s) + (t-2t) + (u-2u) \end{pmatrix}$$
$$z = \begin{pmatrix} 0 \\ -t - u \\ 0 - t - u \end{pmatrix} = \begin{pmatrix} 0 \\ -t - u \\ -t - u \end{pmatrix}$$

We can factor out the scalar part:
$$z = (-t - u) \begin{pmatrix} 0 \\ 1 \\ 1 \end{pmatrix}$$
Since $s, t, u$ can be any real numbers, their combination $k = -t-u$ can also be any real number.
So, the intersection $\text{im } A \cap \text{im } B$ is the set of all vectors $z = k \begin{pmatrix} 0 \\ 1 \\ 1 \end{pmatrix}$ for $k \in \mathbb{R}$.

This is the span of the vector $\begin{pmatrix} 0 \\ 1 \\ 1 \end{pmatrix}$.

A basis for $\text{im } A \cap \text{im } B$ is $\left\{ \begin{pmatrix} 0 \\ 1 \\ 1 \end{pmatrix} \right\}$.

### 5

Recall:
$$x = \begin{pmatrix} s + t + u \\ -s - t - u \\ s \end{pmatrix}$$
where $s, t, u \in \mathbb{R}$.

We separate the vector $x$ based on the free parameters $s, t,$ and $u$:
$$x = \begin{pmatrix} s \\ -s \\ s \end{pmatrix} + \begin{pmatrix} t \\ -t \\ 0 \end{pmatrix} + \begin{pmatrix} u \\ -u \\ 0 \end{pmatrix}$$
$$x = s \begin{pmatrix} 1 \\ -1 \\ 1 \end{pmatrix} + t \begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix} + u \begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix}$$

The set $L_{A}^{-1}(\text{im } B)$ is the subspace spanned by the vectors in this combination:
$$L_{A}^{-1}(\text{im } B) = \text{span} \left\{ \begin{pmatrix} 1 \\ -1 \\ 1 \end{pmatrix}, \begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix}, \begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix} \right\}$$

A basis is a *linearly independent* spanning set. The vector $\begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix}$ is repeated, so the set is linearly dependent. We can remove the redundant vector. The remaining two vectors:
$v_1 = \begin{pmatrix} 1 \\ -1 \\ 1 \end{pmatrix}$ and $v_2 = \begin{pmatrix} 1 \\ -1 \\ 0 \end{pmatrix}$
are linearly independent (they are clearly not scalar multiples of each other).

Therefore, these two vectors form a basis for the subspace.
