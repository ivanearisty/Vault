## Problem 1
### i

$$
\begin{gather}
\text{(i) Let } u, v \in V \text{ be distinct vectors in V. Then } \\ u,v \text{ is linearly dependent if and only if } u \text{ or } v \text{ is a multiple of each other.}
\end{gather}
$$

- $v_1,\dots,v_n$ are **linearly dependent** if some non-all-zero coefficients exist with $a_1v_1+\cdots+a_nv_n=0$.
- They’re **linearly independent** if the only solution to $a_1v_1+\cdots+a_nv_n=0$ is $a_1=\cdots=a_n=0$.

$$
\begin{gather}
\Rightarrow 
\text{If } \{ u,v \} \text{ is linearly dependent, then one vector is a multiple of another.} \\ \\
\text{From the definition of linear dependence: } \\
\{ u,v \} \text{ is LD if }  ∃a,b∈F \text{ such that } (¬(a=0∧b=0))∧(au+bv= \mathbf{0}).
\\
\\
\text{Since not both a and b are zero, we have two cases: } \\
\text{Case 1: } b \neq 0 \\
au+bv = 0  \\ 
bv = -au \\
v = \left( -\frac{a}{b} \right) u\\ \\
\text{Case 2: } a \neq 0 \\
au + bv = 0 \\
au = -bv \\
u = \left( -\frac{b}{a} \right)v \\

\text{since } a \in \mathbb{R} \land b \in \mathbb{R} \text{ in all possibilities, one vector is a multiple of another.} \\
\\ 
\Leftarrow 
\text{If one vector is a multiple of another, then } \{ u,v \} \text{ is LD:} \\
\text{Let } c \text{ be a scalar such that } c \in \mathbb{R}.  \\
\text{This means that there is a scalar such that:} \\
u = cv \\
u + (-cv) = \mathbf{0} \\
(1)u + (-c)v = \mathbf{0} \\
\text{This is a linear combination of u and v that equals the zero vector.} \\
\text{The coefficients are a₁ = 1 and a₂ = -c.} \\
\text{Since a₁ is not zero, the coefficients are not both zero.} \\
\text{Therefore, by definition, the set } \{ u,v \} \text{is linearly dependent}
\end{gather}
$$

### ii

Any subset of linearly independent vectors is linearly independent.

$$
\begin{gather}
\text{Let } S = \{ v_{1},v_{2},\dots,v_{n} \} \text{ be an arbitrary linearly independent set of vectors.} \\
\text{Let } T \subseteq S \text{ such that } \{  v_{1},v_{2},\dots,v_{k} \} \text{ where } k \leq n. \\ \\
\text{For the sake of contradiction, let's assume that } T \text{ is linearly dependent.} \\ 
\text{This means that } \exists \{ a_{1},a_{2},\dots, a_{k} \} \in F \text{ such that: } \\ 
(a_{1}v_{1} + a_{2},v_{2} + \dots + a_{k}v_{k} = 0) \land (\exists a_{g} \in \{ a_{1},a_{2},\dots, a_{k} \} \rightarrow a_{g} \neq 0) \\ \\
\text{We could then construct S as these vectors  } \{ a_{1}v_{1},a_{2}v_{2},\dots, a_{k}v_{k} \} : k \leq n \\
\text{and for the missing vectors } \text{, since }  \exists a_{g} \in \{ a_{1},a_{2},\dots, a_{k} \} \rightarrow a_{g} \neq 0 \\
\{ a_{k+1}v_{k+1},a_{k+2}v_{k+2},\dots, a_{n }v_{kn} \} \text{ we can set the scalars to 0.}  \\ \\
\text{Hence, we have now constructed } S \text{ as a dependent set of vectors.} \\
\text{However, this contradicts our initial assumption that } S \text{ was lineraly independent}
\end{gather} 
$$

### iii 

Any set containing a set of linearly dependent vectors is linearly dependent. In particular, if a set of vectors contain a zero vector then it is linearly dependent. 

$$
\begin{gather}
\text{Let } T = \{ v_{1},v_{2},\dots ,v_{k} \} \text{ be a linearly dependent set.} \\
\text{Let } S \text{ be any set containing } T. \\
\text{For example: } S = \{ v_{1},v_{2},\dots ,v_{k}, v_{k+1},\dots,v_{n} \} \\
\text{Since } T \text{ is linearly dependent, we know there exists scalars } \{c_{1},c_{2},\dots,c_{k} \} \\
\text{such that: } \\
c_{1}v_{1}+c_{2}v_{2}+\dots + c_{k}v_{k} = \mathbf{0} \\
\text{We then extend this to a linear combination of all vectors in } S \\
c_{1}v_{1}+c_{2}v_{2}+\dots + c_{k}v_{k} + \mathbf{0}v_{k+1} + \dots + \mathbf{0}v_{n}= \mathbf{0} \\
\text{by giving the remaining vectors a coefficient of 0.} \\
\text{Since we know that at least one of the scalars from } c_{1} \text{ to } c_{k} \text{ is not zero.} \\
\text{The definition of a linearly dependent set holds for } S. \\
\\
\text{Let } S \text{ be a set containing the zero vector.} \\
\text{Consider: } (1)\mathbf{0} + 0v_{2}+0v_{3}+\dots+v_{n} \\
\text{this sum is equal to the zero vector and not all coefficients are 0.} \\
\text{By definition it's linearly dependent.}
\end{gather}
$$

### iv

Let $v_{1},v_{2},\dots,v_{n}$ be linearly independent.
Suppose $v_{1}+w, v_{2}+\mathbf{w},\dots,v_{n}+w$ are linearly dependent.
Then, $w\in span(v_{1},\dots,v_{n})$


$$
\begin{gather}
\text{Let } S \text{ be the set of vectors } v_{1},v_{2},\dots,v_{n} \\
\text{A vector } w \text{ is in } span(S) \text{ if a combination of the vectors in } S \\
\text{can construct it with some coefficients } \{ a_{1},a_{2},\dots,a_{n} \} \in \mathbb{R} \\ \\
\text{If } v_{1}+w, v_{2}+\mathbf{w},\dots,v_{n}+w\text{ is linearly dependent, then:} \\
a_{1}(v_{1}+w) + a_{2}(v_{2}+w)+\dots+a_{n}(v_{n}+w) = 0 \\
\text{Distributing we have: } \\
a_{1}v_{1}+a_{1}w + a_{2}v_{2}+a_{2}w+\dots+a_{n}v_{n}+a_{n}w = 0 \\
\text{which in turn:} \\
a_{1}v_{1}+ a_{2}v_{2}+\dots+a_{n}v_{n} = -(a_{1}w + a_{2}w+\dots++a_{n}w)\\
a_{1}v_{1}+ a_{2}v_{2}+\dots+a_{n}v_{n}  = w(-(a_{1}+a_{2}+\dots+a_{n})) \\
\text{Call } \sum(a_{1},a_{2},\dots,a_{n}) \text{ some constant } C \\
\text{Since this constant is non-zero, then: } \\
\left( -\left( \frac{a_{1}}{c} \right) \right)v_{1}+ \left( -\left( \frac{a_{2}}{c} \right) \right)v_{2}+\dots+\left( -\left( \frac{a_{n}}{c} \right) \right)v_{n} = w \\ 
\end{gather}
$$
#Note-for-prof   Figure out why this ^ is not 

The final equation shows that w can be written as a linear combination of the vectors​. By definition, this means that $w\in span(S)$

### v

![[Screenshot 2025-09-21 at 9.29.20 PM.png]]

$$
\begin{gather}
\text{Let u be an arbitrary vector in } span(v_{1}​,v_{2},v_{3}​,v_{4}​) \\
\text{By definition, we can write u as a linear combination:} \\
\text{If } span(v_{1},v_{2},v_{3},v_{4}) \subseteq span(v_{1}-v_{2},v_{2}-v_{3},v_{3}-v_{4},v_{4})
\end{gather}
$$

## Problem 2

![[Screenshot 2025-12-14 at 7.30.47 PM.png]]

### i

Suppose there exist scalars $c_1$ and $c_2$ such that for all $x$:
$$c_1 \cos x + c_2 \sin x = 0$$

* **Let $x = 0$:**
    $$c_1 \cos(0) + c_2 \sin(0) = 0$$
    $$c_1(1) + c_2(0) = 0$$
    $$\Rightarrow c_1 = 0$$
    $$c_2 \sin x = 0$$

* **Let $x = \frac{\pi}{2}$:**
    $$c_2 \sin\left(\frac{\pi}{2}\right) = 0$$
    $$c_2(1) = 0$$
    $$\Rightarrow c_2 = 0$$

Since the only solution is $c_1 = 0$ and $c_2 = 0$, the set $\{\cos x, \sin x\}$ is **linearly independent**.
### ii

Suppose there exist scalars $c_1$ and $c_2$ such that for all $x$:
$$c_1 e^x + c_2 e^{2x} = 0$$
* **Let $x = 0$:**
    $$c_1 e^0 + c_2 e^{2(0)} = 0$$
    $$c_1(1) + c_2(1) = 0$$
    $$\Rightarrow c_1 + c_2 = 0 \quad$$

* **Let $x = 1$:**
    $$c_1 e^1 + c_2 e^2 = 0$$
    $$e(c_1 + c_2 e) = 0$$
    Since $e \neq 0$, we can divide by it:
    $$c_1 + c_2 e = 0 \quad$$

From Equation 1, we know that $c_1 = -c_2$. 

$$(-c_2) + c_2 e = 0$$
$$c_2(e - 1) = 0$$

Since $e \approx 2.718$, we know that $(e - 1) \neq 0$. Therefore, we can divide by $(e-1)$, which forces:
$$c_2 = 0$$

Substituting $c_2 = 0$ back into Equation 1 ($c_1 = -c_2$):
$$c_1 = 0$$

Since the only solution is $c_1 = 0$ and $c_2 = 0$, the set $\{e^x, e^{2x}\}$ is **linearly independent**.

## Problem 3
![[Screenshot 2025-12-14 at 7.34.18 PM.png]]

### i

1.  **Zero Vector:** The zero polynomial is defined as $z(x) = 0$ for all $x$. Evaluating at $x=1$, we get $z(1) = 0$. Thus, $z \in \mathcal{Q}_n(1)$.
2.  **Addition:** Let $f, g \in \mathcal{Q}_n(1)$. By definition, this means $f(1) = 0$ and $g(1) = 0$.
    Evaluate the sum $(f+g)$ at $x=1$:
    $$(f+g)(1) = f(1) + g(1) = 0 + 0 = 0$$
    Therefore, $f+g \in \mathcal{Q}_n(1)$.
3.  **Scalar Multiplication:** Let $f \in \mathcal{Q}_n(1)$ and $c \in \mathbb{R}$.
    Evaluate the product $(cf)$ at $x=1$:
    $$(cf)(1) = c \cdot f(1) = c \cdot 0 = 0$$
    Therefore, $cf \in \mathcal{Q}_n(1)$.

### ii

Consider the **evaluation map** $T: \mathcal{P}_n(\mathbb{R}) \to \mathbb{R}$ defined by $T(f) = f(1)$.
* This map is linear.
* The subspace $\mathcal{Q}_n(1)$ is exactly the **kernel** (or null space) of $T$.
* The image of $T$ is all of $\mathbb{R}$ (since we can easily find a polynomial, like $f(x)=1$, such that $f(1) \neq 0$). Thus, $\dim(\text{Im}(T)) = 1$.

By the **Rank-Nullity Theorem**:
$$\dim(\mathcal{P}_n(\mathbb{R})) = \dim(\text{Ker}(T)) + \dim(\text{Im}(T))$$
$$n+1 = \dim(\mathcal{Q}_n(1)) + 1$$
$$\dim(\mathcal{Q}_n(1)) = n$$

**Finding a Basis:**
We need $n$ linearly independent polynomials that satisfy $f(1)=0$.
A polynomial $f(x)$ vanishes at $x=1$ if and only if $(x-1)$ is a factor.
We can construct a basis by shifting the standard powers $x^k$ to evaluate to 0 at $x=1$. Consider the set:
$$\mathcal{B} = \{ x-1, x^2-1, x^3-1, \dots, x^n-1 \}$$

1.  **Cardinality:** There are $n$ vectors in this set.
2.  **In Subspace:** For any basis vector $b_k(x) = x^k - 1$, $b_k(1) = 1^k - 1 = 0$. So they are in $\mathcal{Q}_n(1)$.
3.  **Linear Independence:**
    Consider the linear combination:
    $$c_1(x-1) + c_2(x^2-1) + \dots + c_n(x^n-1) = 0$$
    The highest degree term is $c_n x^n$. For the polynomial to be zero, $c_n$ must be 0. We can proceed by induction downwards (the next highest term is $c_{n-1}x^{n-1}$, etc.) to show that all $c_i = 0$.
    
Thus, a valid basis is:
$$\mathcal{B} = \{ x^k - 1 \mid k = 1, \dots, n \}$$
### iii

To show that $\mathcal{P}_n(\mathbb{R})$ is the sum of these two spaces, we need to demonstrate that **any** polynomial $p(x) \in \mathcal{P}_n(\mathbb{R})$ can be written as a sum of a vector in $\mathcal{Q}_n(1)$ and a vector in $\text{span}(1)$.

Let $p(x)$ be any arbitrary polynomial in $\mathcal{P}_n(\mathbb{R})$.
Let $c = p(1)$ be the value of the polynomial at $x=1$. Note that $c$ is a constant, so the constant polynomial $r(x) = c$ is in $\text{span}(1)$.

Now, we can decompose $p(x)$ as:
$$p(x) = \underbrace{(p(x) - c)}_{\text{call this } q(x)} + \underbrace{c}_{\text{call this } r(x)}$$

1.  **Analyze $r(x)$:**
    $r(x) = c$, which is clearly in $\text{span}(1)$.

2.  **Analyze $q(x)$:**
    $q(x) = p(x) - p(1)$.
    Evaluate $q$ at $x=1$:
    $$q(1) = p(1) - p(1) = 0$$
    Since $q(1) = 0$ and $q$ is a polynomial of degree at most $n$ (subtracting a constant doesn't increase degree), **$q(x) \in \mathcal{Q}_n(1)$**.

Since any $p(x)$ can be written as $q(x) + r(x)$ where $q \in \mathcal{Q}_n(1)$ and $r \in \text{span}(1)$, we have shown that:
$$\mathcal{P}_n(\mathbb{R}) = \mathcal{Q}_n(1) + \text{span}(1)$$
## Problem 4

![[Screenshot 2025-12-14 at 7.34.28 PM.png]]


### i

Suppose that $v_1 + v_2 \in V_1 \cup V_2$.
This means that either $v_1 + v_2 \in V_1$ or $v_1 + v_2 \in V_2$. Let's examine both cases.

* **Case 1: Assume $v_1 + v_2 \in V_1$.**
    Since $V_1$ is a subspace, it is closed under subtraction. We know that $v_1 \in V_1$.
    Therefore, the difference $(v_1 + v_2) - v_1$ must also be in $V_1$.
    $$(v_1 + v_2) - v_1 = v_2$$
    This implies $v_2 \in V_1$.
    **Contradiction:** The problem states that $v_2 \notin V_1$.

* **Case 2: Assume $v_1 + v_2 \in V_2$.**
    Since $V_2$ is a subspace, it is closed under subtraction. We know that $v_2 \in V_2$.
    Therefore, the difference $(v_1 + v_2) - v_2$ must also be in $V_2$.
    $$(v_1 + v_2) - v_2 = v_1$$
    This implies $v_1 \in V_2$.
    **Contradiction:** The problem states that $v_1 \notin V_2$.


Since both cases lead to a contradiction, our initial assumption must be false. Therefore, **$v_1 + v_2 \notin V_1 \cup V_2$**.


### ii

$\Leftarrow$
Assume $V_1 \subset V_2$ or $V_2 \subset V_1$.
* If $V_1 \subset V_2$, then $V_1 \cup V_2 = V_2$. Since $V_2$ is a subspace, the union is a subspace.
* If $V_2 \subset V_1$, then $V_1 \cup V_2 = V_1$. Since $V_1$ is a subspace, the union is a subspace.
In either case, $V_1 \cup V_2$ is a subspace.

$\Rightarrow$
Assume $V_1 \cup V_2$ is a subspace. We must show that one subspace is contained in the other.
We will prove this by **contrapositive** (or contradiction, utilizing the result from Part (i)).

* Suppose that neither is contained in the other: assume $V_1 \not\subset V_2$ **and** $V_2 \not\subset V_1$.
* Because $V_1 \not\subset V_2$, there must exist some vector $v_1 \in V_1$ such that $v_1 \notin V_2$.
* Because $V_2 \not\subset V_1$, there must exist some vector $v_2 \in V_2$ such that $v_2 \notin V_1$.
* Now, consider the vector $w = v_1 + v_2$.
* Since we assumed $V_1 \cup V_2$ is a subspace, it must be closed under addition. Since $v_1 \in V_1 \cup V_2$ and $v_2 \in V_1 \cup V_2$, their sum $v_1 + v_2$ must be in $V_1 \cup V_2$.
* **However**, from Part (i), we proved that for such $v_1$ and $v_2$, the sum $v_1 + v_2 \notin V_1 \cup V_2$.
* This is a contradiction.

The assumption that neither subspace contains the other must be false. Therefore, it must be true that either **$V_1 \subset V_2$ or $V_2 \subset V_1$**.