# Problem Set 2
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

