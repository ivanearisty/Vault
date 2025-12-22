Excellent — let’s strip this theorem down to its core idea and then show exactly how you use it in problems.

---

> [!tip] Theorem 4.22 — What It Really Means (in plain language)
> 
> We have a linear map
> 
> $$T: V \to W$$
> 
> and we’ve chosen:
> 
> - a basis ($\beta = (v_1, v_2, \dots, v_n)$) for the domain ($V$),
>     
> - a basis ($\gamma = (w_1, w_2, \dots, w_m)$) for the codomain ($W$).
>     
> 
> This theorem says:
> 
> Once you choose those bases, every linear transformation ($T$) can be represented as a matrix ($A$) such that applying ($T$) to a vector is the same as multiplying by ($A$).
> 
> Symbolically:
> 
> $$[T(v)]_\gamma = A [v]_\beta$$
> 
> That’s all it’s saying.

---

> [!info] How to Build That Matrix (A)
> 
> Think of the columns of ($A$) as the “pictures” of what ($T$) does to the basis vectors of ($V$), expressed in the basis of ($W$).
> 
> Steps (you’ll literally do this in every problem):
> 
> 1. Take each domain basis vector ($v_j$).
>     
> 2. Apply ($T$) to it → get a vector ($T(v_j)$) in ($W$).
>     
> 3. Write ($T(v_j)$) as a combination of the basis vectors of ($W$):
>     
> 
> $$ T(v_j) = a_{1j}w_1 + a_{2j}w_2 + \dots + a_{mj}w_m$$
> 
> 4. The coefficients ($(a_{1j}, \dots, a_{mj})$) become the $j$-th column of the matrix ($A$).
>     
> 5. Do this for all ($j=1,\dots,n$).
>     

---

> [!example] Example
> 
> Let’s say
> 
> $$T:\mathbb R^2 \to \mathbb R^2, \quad T(x,y) = (2x + y, 3x - y)$$
> 
> and we use the standard basis for both domain and codomain:
> 
> $$\beta = \gamma = \{ e_1 = (1,0), e_2 = (0,1) \}$$
> 
> ### Step 1. Apply $T$ to each basis vector.
> 
> $$T(e_1) = T(1,0) = (2,3)$$
> 
> $$T(e_2) = T(0,1) = (1,-1)$$
> 
> ### Step 2. Write each result in the codomain basis.
> 
> They’re already expressed in ($\{e_1,e_2\}$):
> 
> $$T(e_1) = 2e_1 + 3e_2, \quad T(e_2) = 1e_1 - 1e_2$$
> 
> ### Step 3. Build the matrix — columns are the coefficients.
> 
> $$A = \begin{bmatrix} 2 & 1 \\ 3 & -1 \end{bmatrix}$$
> 
> ✅ So:
> 
> $$[T]_{\gamma\beta} = \begin{bmatrix} 2 & 1 \\ 3 & -1 \end{bmatrix}$$

---

> [!check] How You Use It on Exercises
> 
> In exercises, you’ll usually do one of these:
> 
> - Given ($T$), find its matrix.
>     
>     → Apply the 5 steps above.
>     
> - Given a matrix ($A$), find ($T(v)$).
>     
>     → Multiply: ($[T(v)]_\gamma = A [v]_\beta$).
>     
> - Change of basis problems.
>     
>     → You’ll compute a new matrix representation when the basis changes, using this same idea but with a change-of-basis matrix.
>     

---

> [!abstract] Mini Intuition
> 
> - Choosing a basis is like choosing a coordinate system.
>     
> - In that coordinate system, linear maps become matrices.
>     
> - Vectors become coordinate columns.
>     
> - The formula ($[T(v)]_\gamma = A [v]_\beta$) is the bridge that connects the abstract linear transformation to ordinary matrix multiplication.
>     

--- 

