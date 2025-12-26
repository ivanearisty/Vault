## Sesquilinearity

One of the defining axioms of a complex inner product space is conjugate symmetry.
$$
\langle x,y \rangle = \overline{\langle y,x \rangle }
$$
This means that if you swap the order of the vectors, you must take the complex conjugate of the resulting number.

In complex inner product spaces, we cannot have linearity in both arguments at the same time if we want to preserve the concept of **length**.

If the inner product were linear in both arguments (bilinear), then the "length" of a complex vector would not always be a real, positive number.

If the inner product were linear in both slots, consider a vector $v$ and a complex scalar $i$:

- **Linear in 1st slot:** $\langle iv, v \rangle = i \langle v, v \rangle$
- **Linear in 2nd slot:** $\langle v, iv \rangle = i \langle v, v \rangle$
$$\langle iv, iv \rangle = i \cdot i \cdot \langle v, v \rangle = i^2 \langle v, v \rangle = -1 \langle v, v \rangle$$

The "length squared" of $iv$ would be a **negative number**. To ensure that $\langle v, v \rangle$ is always a positive real number (so we can take its square root to find distance), we _must_ have one slot be **conjugate-linear**.

To fix this, inner product is **sesquilinear** (meaning "one-and-a-half linear").

- **1st slot:** Standard linear.
- **2nd slot:** Conjugate linear.

With conjugate linearity in the second slot, the calculation for $iv$ becomes:

$$\langle iv, iv \rangle = i \cdot \bar{i} \cdot \langle v, v \rangle = i(-i) \langle v, v \rangle = 1 \langle v, v \rangle$$

Because $i \cdot \bar{i} = 1$, the length stays positive and real.

### 3. Math vs. Physics Conventions

It is important to note that the "first slot" vs. "second slot" choice is purely a **convention**.

- **Mathematicians** usually pick the **first** slot to be linear (as seen in your textbook).
    
- **Physicists** (especially in Quantum Mechanics using Bra-Ket notation $\langle \phi | \psi \rangle$) usually pick the **second** slot to be linear.
    

|**Property**|**Math Convention**|**Physics Convention**|
|---|---|---|
|**Linear Slot**|First: $\langle cx, y \rangle = c \langle x, y \rangle$|Second: $\langle x, cy \rangle = c \langle x, y \rangle$|
|**Conjugate Slot**|Second: $\langle x, cy \rangle = \bar{c} \langle x, y \rangle$|First: $\langle cx, y \rangle = \bar{c} \langle x, y \rangle$|

Regardless of the convention, the core logic is the same: one side must "undo" the imaginary part of the other to keep the geometry consistent.

Would you like to see how this rule affects the **Cauchy-Schwarz Inequality** in complex spaces?
## Theorem 6.1

### A
$$
\begin{gather}
\langle x, y+z \rangle = \langle x,y \rangle + \langle x,z \rangle \\
\text{Proof:} \\
\langle x, y + z \rangle  = \overline{\langle y + z, x \rangle} = \overline{\langle y,x \rangle} + \overline{\langle z,x \rangle} = \langle x,y \rangle  + \langle x,z \rangle 
\end{gather}
$$
### B
