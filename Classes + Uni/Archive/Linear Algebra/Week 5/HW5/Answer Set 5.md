### Problem 1

The standard basis for $M_{2\times2}(F)$ is $\beta = \{v_1, v_2, v_3, v_4\}$, where:
$v_1 = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}, v_2 = \begin{pmatrix} 0 & 1 \\ 0 & 0 \end{pmatrix}, v_3 = \begin{pmatrix} 0 & 0 \\ 1 & 0 \end{pmatrix}, v_4 = \begin{pmatrix} 0 & 0 \\ 0 & 1 \end{pmatrix}$

* $T(v_1) = A v_1 = \begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix} \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 3 & 0 \end{pmatrix}$
* $T(v_2) = A v_2 = \begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix} \begin{pmatrix} 0 & 1 \\ 0 & 0 \end{pmatrix} = \begin{pmatrix} 0 & 1 \\ 0 & 3 \end{pmatrix}$
* $T(v_3) = A v_3 = \begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix} \begin{pmatrix} 0 & 0 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} 2 & 0 \\ 4 & 0 \end{pmatrix}$
* $T(v_4) = A v_4 = \begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix} \begin{pmatrix} 0 & 0 \\ 0 & 1 \end{pmatrix} = \begin{pmatrix} 0 & 2 \\ 0 & 4 \end{pmatrix}$
* $T(v_1) = 1v_1 + 0v_2 + 3v_3 + 0v_4 \implies [T(v_1)]_{\beta} = \begin{pmatrix} 1 \\ 0 \\ 3 \\ 0 \end{pmatrix}$
* $T(v_2) = 0v_1 + 1v_2 + 0v_3 + 3v_4 \implies [T(v_2)]_{\beta} = \begin{pmatrix} 0 \\ 1 \\ 0 \\ 3 \end{pmatrix}$
* $T(v_3) = 2v_1 + 0v_2 + 4v_3 + 0v_4 \implies [T(v_3)]_{\beta} = \begin{pmatrix} 2 \\ 0 \\ 4 \\ 0 \end{pmatrix}$
* $T(v_4) = 0v_1 + 2v_2 + 0v_3 + 4v_4 \implies [T(v_4)]_{\beta} = \begin{pmatrix} 0 \\ 2 \\ 0 \\ 4 \end{pmatrix}$
$[T]_{\beta}^{\beta} = \begin{pmatrix} 1 & 0 & 2 & 0 \\ 0 & 1 & 0 & 2 \\ 3 & 0 & 4 & 0 \\ 0 & 3 & 0 & 4 \end{pmatrix}$

### Problem 2

- P: We express the vectors of $\beta = (v_1, v_2, v_3)$ in terms of $\gamma = (w_1, w_2, w_3)$.
    * $v_1 = 1 = w_1$. So, $[v_1]_{\gamma} = \begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix}$.
    * $v_2 = x = (x+1) - 1 = w_2 - w_1$. So, $[v_2]_{\gamma} = \begin{pmatrix} -1 \\ 1 \\ 0 \end{pmatrix}$.
    * $v_3 = x^2 = ((x+1)-1)^2 = (x+1)^2 - 2(x+1) + 1 = w_3 - 2w_2 + w_1$. So, $[v_3]_{\gamma} = \begin{pmatrix} 1 \\ -2 \\ 1 \end{pmatrix}$.

    The change of basis matrix from $\gamma$ to $\beta$ is:
    $$
    P = [id]_{\beta}^{\gamma} = \begin{pmatrix} 1 & -1 & 1 \\ 0 & 1 & -2 \\ 0 & 0 & 1 \end{pmatrix}
    $$

* $P^{-1}$: The change of basis matrix from $\beta$ to $\gamma$ is $P^{-1} = [id]_{\gamma}^{\beta}$. We express the vectors of $\gamma$ in terms of $\beta$.
    * $w_1 = 1 = 1 \cdot v_1 + 0 \cdot v_2 + 0 \cdot v_3$. So, $[w_1]_{\beta} = \begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix}$.
    * $w_2 = x+1 = 1 \cdot v_1 + 1 \cdot v_2 + 0 \cdot v_3$. So, $[w_2]_{\beta} = \begin{pmatrix} 1 \\ 1 \\ 0 \end{pmatrix}$.
    * $w_3 = (x+1)^2 = x^2 + 2x + 1 = 1 \cdot v_1 + 2 \cdot v_2 + 1 \cdot v_3$. So, $[w_3]_{\beta} = \begin{pmatrix} 1 \\ 2 \\ 1 \end{pmatrix}$.

    The change of basis matrix from $\beta$ to $\gamma$ is:
    $$
    P^{-1} = [id]_{\gamma}^{\beta} = \begin{pmatrix} 1 & 1 & 1 \\ 0 & 1 & 2 \\ 0 & 0 & 1 \end{pmatrix}
    $$
#### (ii)

* $[T]_{\beta}^{\beta}$: We apply T to the vectors in $\beta=(1,x,x^2)$.
    * $T(1) = (3+x)(0) + 1 = 1 = 1 \cdot 1 + 0 \cdot x + 0 \cdot x^2$. So, $[T(1)]_{\beta} = \begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix}$.
    * $T(x) = (3+x)(1) + x = 3+2x = 3 \cdot 1 + 2 \cdot x + 0 \cdot x^2$. So, $[T(x)]_{\beta} = \begin{pmatrix} 3 \\ 2 \\ 0 \end{pmatrix}$.
    * $T(x^2) = (3+x)(2x) + x^2 = 6x + 3x^2 = 0 \cdot 1 + 6 \cdot x + 3 \cdot x^2$. So, $[T(x^2)]_{\beta} = \begin{pmatrix} 0 \\ 6 \\ 3 \end{pmatrix}$.
    $$
    [T]_{\beta}^{\beta} = \begin{pmatrix} 1 & 3 & 0 \\ 0 & 2 & 6 \\ 0 & 0 & 3 \end{pmatrix}
    $$

* $[T]_{\gamma}^{\gamma}$: We apply T to the vectors in $\gamma=(1, x+1, (x+1)^2)$.
    * $T(1) = 1 = 1 \cdot 1 + 0 \cdot (x+1) + 0 \cdot (x+1)^2$. So, $[T(1)]_{\gamma} = \begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix}$.
    * $T(x+1) = (3+x)(1) + (x+1) = 4+2x = 2(x+1) + 2 = 2 \cdot 1 + 2 \cdot (x+1) + 0 \cdot (x+1)^2$. So, $[T(x+1)]_{\gamma} = \begin{pmatrix} 2 \\ 2 \\ 0 \end{pmatrix}$.
    - $$
\begin{gather}
T((x+1)^2) = (3+x)(2(x+1)) + (x+1)^2 = (2+(x+1))(2(x+1)) \\
(x+1)^2 = 4(x+1) + 3(x+1)^2 = 0 \cdot 1 + 4 \cdot (x+1) + 3 \cdot (x+1)^2 \\
    So, [T((x+1)^2)]_{\gamma} = \begin{pmatrix} 0 \\ 4 \\ 3 \end{pmatrix}
\end{gather}
$$
$$ [T]_{\gamma}^{\gamma} = \begin{pmatrix} 1 & 2 & 0 \\ 0 & 2 & 4 \\ 0 & 0 & 3 \end{pmatrix} $$

#### (iii)

$$
P[T]_{\beta}^{\beta}P^{-1} = \begin{pmatrix} 1 & -1 & 1 \\ 0 & 1 & -2 \\ 0 & 0 & 1 \end{pmatrix} \begin{pmatrix} 1 & 3 & 0 \\ 0 & 2 & 6 \\ 0 & 0 & 3 \end{pmatrix} \begin{pmatrix} 1 & 1 & 1 \\ 0 & 1 & 2 \\ 0 & 0 & 1 \end{pmatrix}
$$
First, multiply $P[T]_{\beta}^{\beta}$:
$$
\begin{pmatrix} 1 & -1 & 1 \\ 0 & 1 & -2 \\ 0 & 0 & 1 \end{pmatrix} \begin{pmatrix} 1 & 3 & 0 \\ 0 & 2 & 6 \\ 0 & 0 & 3 \end{pmatrix} = \begin{pmatrix} 1 & 1 & -3 \\ 0 & 2 & 0 \\ 0 & 0 & 3 \end{pmatrix}
$$
Then, multiply the result by $P^{-1}$:
$$
\begin{pmatrix} 1 & 1 & -3 \\ 0 & 2 & 0 \\ 0 & 0 & 3 \end{pmatrix} \begin{pmatrix} 1 & 1 & 1 \\ 0 & 1 & 2 \\ 0 & 0 & 1 \end{pmatrix} = \begin{pmatrix} 1 & 2 & 0 \\ 0 & 2 & 4 \\ 0 & 0 & 3 \end{pmatrix}
$$
Which is equal to $[T]_{\gamma}^{\gamma}$ as calculated in part (ii).

### Problem 3

#### (i)

Let $\beta = \{v_0, v_1, ..., v_n\}$ where $v_j(x)=(x-a)^j$. 
The set $\beta^* = \{\varphi_0, \varphi_1, ..., \varphi_n\}$ is the dual basis to $\beta$ if $\varphi_i(v_j) = \delta_{ij}$ for all $i,j \in \{0, ..., n\}$

We compute $\varphi_i(v_j)$:
$$
\varphi_i(v_j) = \frac{1}{i!} v_j^{(i)}(a) = \frac{1}{i!} \frac{d^i}{dx^i}((x-a)^j)
$$
The $i$-th derivative of $(x-a)^j$ is:
* If $i < j$: $\frac{j!}{(j-i)!}(x-a)^{j-i}$. At $x=a$, this is 0.
* If $i > j$: The derivative is 0.
* If $i = j$: The derivative is $i!$. At $x=a$, this is $i!$.

Therefore,
$$
\varphi_i(v_j) = \frac{1}{i!} \times \begin{cases} 0 & \text{if } i \neq j \\ i! & \text{if } i = j \end{cases} = \delta_{ij}
$$
Since $\varphi_i(v_j) = \delta_{ij}$, the set $\{\varphi_{0},\varphi_{1},...,\varphi_{n}\}$ is the dual basis.

#### (ii)

Any polynomial $f(x) \in \mathcal{P}_n(\mathbb{R})$ can be written as a linear combination of the basis vectors:
$$
f(x) = \sum_{i=0}^{n} c_i v_i(x) = \sum_{i=0}^{n} c_i (x-a)^i
$$
The coefficients $c_i$ form the coordinate vector of $f(x)$. 

As shown in \[[Lecture 5#Remark 5.28]], the coordinates of a vector $v$ are given by applying the dual basis functionals to $v$. Thus, the $i$-th coordinate $c_i$ is given by $\varphi_i(f)$.

Substituting this back into the expansion for $f(x)$:
$$
f(x) = \sum_{i=0}^{n} \varphi_i(f) (x-a)^i
$$

#### (iii)

We want to find scalars $c_i$ such that $l_b = \sum_{i=0}^{n} c_i \varphi_i$.

As shown in the proof of \[[Lecture 5#Proposition 5.27]], for any linear functional $\varphi$, its coordinates with respect to the dual basis $\{\varphi_i\}$ are given by applying $\varphi$ to the primal basis vectors $\{v_i\}$.

So, the coefficient $c_i$ is given by $l_b(v_i)$.
$$
c_i = l_b(v_i) = v_i(b) = (b-a)^i
$$
Therefore, the linear functional $l_b$ can be expressed as:
$$
l_b = \sum_{i=0}^{n} (b-a)^i \varphi_i
$$

### Problem 4

1.  Find Rank and Nullity:
    * The image of T is $\text{im } T = \text{span}\{T(1), T(x), T(x^2), T(x^3)\} = \text{span}\{0, 1, 2x, 3x^2\} = \mathcal{P}_2(\mathbb{R})$.
    * The rank of T is $r = \dim(\text{im } T) = 3$.
    * The kernel of T is $\ker T = \{f \in \mathcal{P}_3(\mathbb{R}) | f' = 0\}$, which is the space of constant polynomials $\mathcal{P}_0(\mathbb{R})$.
    * The nullity of T is $\dim(\ker T) = 1$.

2.  Construct Bases $\beta$ and $\gamma$:
    * Choose a basis for $\gamma$: Start with a basis for $\text{im } T$, such as $\{1, x, x^2\}$, and extend it to a basis for the codomain $\mathcal{P}_3(\mathbb{R})$. We can add $x^3$.
        Let $\gamma = (1, x, x^2, x^3)$.
    * Choose a basis for $\beta$: We need to find a basis $\beta=(v_1, v_2, v_3, v_4)$ such that the matrix representation has the desired form. This means:
        * $T(v_1) = 1 \cdot \gamma_1 = 1$
        * $T(v_2) = 1 \cdot \gamma_2 = x$
        * $T(v_3) = 1 \cdot \gamma_3 = x^2$
        * $T(v_4) = 0$ (so $v_4 \in \ker T$)
    * Find the vectors for $\beta$:
        * For $T(v_1) = 1$, we can choose $v_1 = x$.
        * For $T(v_2) = x$, we can choose $v_2 = \frac{1}{2}x^2$.
        * For $T(v_3) = x^2$, we can choose $v_3 = \frac{1}{3}x^3$.
        * For $T(v_4) = 0$, we need a vector in the kernel of T. We can choose $v_4 = 1$.
    * Verify $\beta$ is a basis: The set $\beta = (x, \frac{1}{2}x^2, \frac{1}{3}x^3, 1)$ consists of four polynomials of distinct degrees (1, 2, 3, 0). They are linearly independent and thus form a basis for $\mathcal{P}_3(\mathbb{R})$.

The desired bases are:
$$
\beta = (x, \frac{1}{2}x^2, \frac{1}{3}x^3, 1)
$$
$$
\gamma = (1, x, x^2, x^3)
$$