## Problem 1
### 1 
$$
\begin{gather}
\text{If T is invertible then:} \\
T \circ T^{-1} = ID_{w} \\
\land \\
T^{-1} \circ T = ID_{v} \\
\\
\text{If we apply:} \\
T \circ T^{-1} = ID_{w} \\
(T \circ T^{-1})^{t} = (ID_{w})^{t} \\
(T^{-1})^{t} \circ T^{t} = ID_{w*} \\ \\
T^{-1} \circ T = ID_{v} \\
(T^{-1} \circ T)^{t} = (ID_{v})^{t} \\
T^{t} \circ (T^{-1})^{t} = ID_{v*} \\ \\
\text{Which proves that } T^{t} \text{ is invertible,} \\
\text{and it's inverse is } (T^{-1})^{t}
\end{gather}
$$

### 2

![[Screenshot 2025-10-31 at 2.13.10 PM.png]]$$
\begin{gather}
\text{From (i) we have that } (A^{-1} )^{t} = (A^{t})^{-1}. \\
\text{If } A^{t} = A, \text{then } (A^{-1} )^{t} = (A^{t})^{-1} = A^{-1}. \\ \\
\text{If } A^{t} = -A \text{ then } (A^{-1} )^{t} = (A^{t})^{-1} = (-A)^{-1} = -A^{-1}.
\end{gather}
$$

## Problem 2
### 1
$$
\begin{gather}
w = \sum_{i=1}^{m} a_{i}w_{i} \\
\bar{\varphi}(w) = \bar{\varphi}\left( \sum_{i=1}^{m} a_{i}w_{i} \right) =
\sum_{i=1}^{m} a_{i}\bar{\varphi}(w_{i}) = 
\sum_{i=1}^{m} a_{i}\varphi(w_{i}) = 
\varphi\left( \sum_{i=1}^{m} a_{i}w_{i} \right) =
\varphi(w)
\end{gather}
$$

### 2
For $a,b \in F$ and $\varphi_1, \varphi_2 \in W^*$,  
$$
i(a\varphi_1 + b\varphi_2) = \widetilde{\,a\varphi_1 + b\varphi_2\,}.
$$
Evaluate both sides on the basis of $V$:

- On $w_i$:  
  $$
  \widetilde{\,a\varphi_1 + b\varphi_2\,}(w_i)
  = a\varphi_1(w_i) + b\varphi_2(w_i)
  = (a\tilde{\varphi}_1 + b\tilde{\varphi}_2)(w_i).
  $$
- On $v_j$:  
  $$
  \widetilde{\,a\varphi_1 + b\varphi_2\,}(v_j)
  = 0 = (a\tilde{\varphi}_1 + b\tilde{\varphi}_2)(v_j).
  $$

Since the two functionals agree on a basis, they are equal:
$$
i(a\varphi_1 + b\varphi_2) = a\,i(\varphi_1) + b\,i(\varphi_2).
$$
Hence, $i$ is linear.


$$
\begin{gather}
\text{Injective: }\\
\text{If } i(\varphi) = \bar{\varphi} = 0 \in V^{*}, \text{then for } w \in W:
\\
\varphi(w) = \bar{\varphi}(w) = 0\\
\text{Which means that } \varphi = 0 \text{ in } W^{*}. \\
\text{Since } ker(i) = \mathbf{0}, \text{ i is injective}
\end{gather}
$$

### 3
### 4

## Problem 3

## Problem 4
