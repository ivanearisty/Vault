You’re given a map ($T: V \to W$) between vector spaces over the same field ($\mathbb F$) (usually ($\mathbb R$) or ($\mathbb C$)). Your job: decide if ($T$) is linear.

---

> [!abstract] The Definition
> 
> For all ($x,y \in V$) and all scalars ($a,b \in \mathbb F$), the map must satisfy:
> 
> $$T(ax+by) = aT(x) + bT(y)$$
> 
> Equivalently, and often faster to check separately:
> 
> - **Additivity**: $T(x+y) = T(x)+T(y)$
>     
> - **Homogeneity**: $T(cx) = cT(x)$ for all scalars ($c$).
>     
> 
> An immediate corollary is the **zero test**: a linear map must send the zero vector to the zero vector.
> 
> $$T(0) = 0$$
> 
> (If ($T(0) \neq 0$), stop—it's not linear.)

---

> [!tip] A Fast 5-Step Checklist
> 
> Use this in order:
> 
> 1. **Domain/Codomain Sanity**: Are ($V$) and ($W$) vector spaces over the same field? (e.g., Polynomials, function spaces, matrices, $\mathbb R^n$, etc.)
>     
> 2. **Zero Test**: Compute ($T(0)$). If ($T(0) \neq 0$), it’s not linear.
>     
> 3. **Structure Test**: Inspect the formula.
>     
> 
> - Sums of components, fixed matrix multiplication, fixed linear combos → **Likely linear**.
>     
> - Products of variables, absolute values, max/min, norms, division by a component, nonlinear activations → **Not linear**.
>     
> - Adding a fixed nonzero vector ($b$) (i.e., ($T(x)=S(x)+b$)) → **Not linear** unless ($b=0$).
>     
> 
> 4. **Axioms on Symbols**: If still unsure, verify additivity & homogeneity symbolically.
>     
> 
> - Compute ($T(x+y)$) and compare with ($T(x)+T(y)$).
>     
> - Compute ($T(cx)$) and compare with ($cT(x)$).
>     
> 
> 5. **Matrix Lens**: If you can write ($T(x)=Ax$) for a fixed matrix ($A$), then ($T$) is linear.
>     

---

> [!info] Common Patterns
> 
> - **Always Linear**:
>     
> - ($T(x)=Ax$) with a fixed matrix ($A$).
>     
> - On function spaces: Derivative ($T(f)=f'$), Integral ($T(f)=\int f$), multiplication by a fixed function ($T(f)=p \cdot f$).
>     
> - On polynomials: ($T(\sum a_i x^i)=\sum c_i a_i x^i$) with fixed scalars ($c_i$).
>     
> - **Not Linear (Typical Traps)**:
>     
> - **Translations/Affine maps**: ($T(x)=S(x)+b$) with ($b \neq 0$). (Fails zero test.)
>     
> - **Component products**: ($T(x_1,\dots,x_n)=x_1x_2$) or ($(x_1^2,\dots)$).
>     
> - **Nonlinear ops**: ($|\cdot|$), ($\max$), ($\mathrm{ReLU}$), norms ($\|x\|$), normalization ($x/\|x\|$).
>     
> - **Variable scalings**: ($T(x)=\alpha(x)x$) where ($\alpha$) depends on ($x$).
>     
> - **Division by a coordinate**: ($T(x_1,x_2)=(x_1/x_2, \cdot)$).
>     

---

> [!check] Minimal Proofs & Arguments
> 
> - **Zero test derivation**: ($T(0)=T(0 \cdot x)=0 \cdot T(x)=0$).
>     
> - **Additivity from full axiom**: put ($a=b=1$).
>     
> - **Homogeneity from full axiom**: put ($b=0$).
>     
> - **Matrix form ⇒ linear**: ($T(ax+by)=A(ax+by)=aAx+bAx=aT(x)+bT(y)$).
>     

---

> [!example] Worked Micro-Examples
> 
> 1. ($T:\mathbb R^3\to\mathbb R^2$), ($T(x,y,z)=(2x-y, x+z)$)
>     
> 
> - **Zero test**: ($T(0,0,0)=(0,0)$). ✅
>     
> - **Structure**: Each output is a fixed linear combo of inputs.
>     
> - **Conclusion**: **Linear**. (Matrix form is ($T(v)=Av$) with ($A = \begin{bmatrix} 2 & -1 & 0 \\ 1 & 0 & 1 \end{bmatrix}$)).
>     
> 
> 2. ($T:\mathbb R^2\to\mathbb R^2$), ($T(x,y)=(x+1, y)$)
>     
> 
> - **Zero test**: ($T(0,0)=(1,0) \neq 0$). ❌
>     
> - **Conclusion**: **Not linear** (it's a translation).
>     
> 
> 3. ($T:\mathbb R^2\to\mathbb R$), ($T(x,y)=xy$)
>     
> 
> - **Zero test**: ($T(0,0)=0$). ✅
>     
> - **Additivity**: ($T((1,0)+(0,1)) = T(1,1) = 1$), but ($T(1,0)+T(0,1) = 0+0 = 0$). Fails. ❌
>     
> - **Conclusion**: **Not linear** (product of components).
>     
> 
> 4. ($T:\mathcal P_3\to\mathcal P_2$), ($T(p)=p'+p$)
>     
> 
> - **Zero test**: ($T(0)=0$). ✅
>     
> - **Operator sum**: The derivative and identity operators are both linear, so their sum is linear.
>     
> - **Conclusion**: **Linear**.
>     
> 
> 5. ($T:C^1[0,1]\to C[0,1]$), ($(T f)(x)=xf(x)$)
>     
> 
> - **Zero test**: ($T(0)=0$). ✅
>     
> - Homogeneity and additivity follow from pointwise rules.
>     
> - **Conclusion**: **Linear** (multiplication by a fixed function ($x$)).
>     

---

> [!tip] Quick Tactics for Exams
> 
> - **To prove linear**: Start with the full axiom ($T(ax+by)=\dots$). Work symbolically.
>     
> - **To disprove linear**: Provide **one** counterexample to additivity or homogeneity, or just show ($T(0) \neq 0$). One is enough.
>     

---

> [!warning] Pitfalls & Fixes
> 
> - **Piecewise definitions**: Check the axioms across the boundary; linearity can fail at the seam.
>     
> - **Hidden dependence on input**: If a coefficient depends on ($x$) (e.g., ($T(x)=\|x\|x$)), it’s not linear.
>     
> - **Field mismatch**: Over ($\mathbb C$), be careful: conjugation is **not** complex-linear (though it is real-linear). Check homogeneity with complex scalars.
>     

---

> [!question] Your Turn
> 
> Is ($T:\mathbb R^2\to\mathbb R^2$), ($T(x,y)=(x, |y|)$) linear? If not, name the first axiom it violates and give a 1-line counterexample.