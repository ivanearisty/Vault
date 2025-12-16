
1. straight up diagonalizable
2. repeated eigenvalues jordan cannonical form
3. complex eigenvalues is the real cannoncial form 12.10-18.8

if reals you do 12.8, 
if complex 12.10.

you have a complex eigenvector corresponding to some eingevalue, you need to take the real part and the imaginary part 


problem 4 is just quadratic formula

### **Recipe 1: Large Determinants (Practice Final Prob 1)**
**The Goal:** Compute the determinant of a large ($4 \times 4$ or $5 \times 5$) matrix.
**The Trick:** It will likely be a **Block Matrix** (lots of zeros in a corner).

**Step-by-Step Algorithm:**
1.  **Scan for Zeros:** Look for a row or column with mostly zeros.
2.  **Laplace Expand:** Use the formula $\det(A) = \sum (-1)^{i+j} a_{ij} \det(\text{Minor})$.
    * *Tip:* If you pick a row with only one non-zero number, you only calculate **one** smaller determinant.
3.  **Repeat:** Keep doing this until you hit a $2 \times 2$ or $3 \times 3$ you can calculate directly.

**Variation Handling ($5 \times 5$ vs $4 \times 4$):**
* **The Practice Problem:** A $5 \times 5$ matrix.
    * Step 1: Expand Col 1 (only one '1'). $\to$ reduced to $4 \times 4$.
    * Step 2: Expand Row 1 (only one '2'). $\to$ reduced to $3 \times 3$.
    * [cite_start]Step 3: Calculate the $3 \times 3$ directly or expand again [cite: 2122-2126].
* **Deviation:** If the zeros aren't perfect, use **Row Operations** first.
    * *Remember:* Adding a multiple of a row to another does **not** change the determinant. [cite_start]Use this to create zeros, *then* expand[cite: 1195].

---
### **Recipe 2: Real Canonical Form (Practice Final Prob 2)**
**The Goal:** Analyze a matrix (likely $2 \times 2$ or $3 \times 3$) that has complex eigenvalues. Find the basis $P$ that makes it "almost diagonal" (Real Canonical Form).

**Step-by-Step Algorithm:**
1.  **Find Eigenvalues:** Solve $\det(xI - A) = 0$.
    * [cite_start]You will get complex roots: $\lambda = a \pm bi$[cite: 2134].
2.  **Find ONE Eigenvector:** Solve $(A - (a+bi)I)v = 0$ for the eigenvalue with the **plus** sign.
    * You will get a vector with complex numbers like $v = \begin{pmatrix} 1 \\ 2+i \end{pmatrix}$.
3.  **Split Real and Imaginary Parts:**
    * Write $v = \text{Re}(v) + i \text{Im}(v)$.
    * Example: If $v = \begin{pmatrix} 1 \\ 2+i \end{pmatrix} = \begin{pmatrix} 1 \\ 2 \end{pmatrix} + i \begin{pmatrix} 0 \\ 1 \end{pmatrix}$.
4.  **Construct $P$:** Set $P = (\text{Re}(v) \quad \text{Im}(v))$.
    * **Crucial:** The order matters. [cite_start]Real part first column, Imaginary part second column[cite: 2139].
5.  [cite_start]**Write the Form:** The answer is always $P^{-1}AP = \begin{pmatrix} a & b \\ -b & a \end{pmatrix}$[cite: 1751].

**Variation Handling ($3 \times 3$ Matrix):**
* If it is $3 \times 3$, you will likely have one **real** eigenvalue ($\lambda_1$) and two **complex** ($\lambda_{2,3} = a \pm bi$).
* Find the real eigenvector $v_1$ for $\lambda_1$.
* Find the complex parts $\text{Re}(v_2), \text{Im}(v_2)$ for $\lambda_2$.
* Your matrix $P$ will be $3 \times 3$: $P = (v_1 \quad \text{Re}(v_2) \quad \text{Im}(v_2))$.
* The form will be block diagonal: $\begin{pmatrix} \lambda_1 & 0 & 0 \\ 0 & a & b \\ 0 & -b & a \end{pmatrix}$.

---

### **Recipe 3: Jordan Form & Functions (Practice Final Prob 3)**
**The Goal:** Handle a non-diagonalizable matrix (repeated eigenvalue) and apply a function like $\log(A)$ or $e^A$ or $\sin(A)$.

**Step-by-Step Algorithm:**
1.  **Check Eigenvalues:** Solve $\det(xI - A) = 0$.
    * [cite_start]You will get a repeated root $\lambda$ (e.g., $(x - 1/2)^2 = 0$)[cite: 2148].
2.  **Check Geometric Multiplicity:**
    * Find the eigenspace $\dim(\ker(A - \lambda I))$.
    * [cite_start]If the dimension is **less** than the algebra power (e.g., dim is 1, but power is 2), it's a **Jordan Block**[cite: 2149].
3.  **Find the Basis (The "Chain"):**
    * Find a vector $w$ that is **NOT** an eigenvector, but $(A - \lambda I)w \neq 0$.
    * Calculate $v = (A - \lambda I)w$. (This $v$ will be an eigenvector).
    * **Construct $P$:** $P = (v \quad w)$. Order matters! [cite_start]Eigenvector first, generalized vector second [cite: 2151-2152].
    * The form is $J = \begin{pmatrix} \lambda & 1 \\ 0 & \lambda \end{pmatrix}$.
4.  **Compute the Function $f(A)$:**
    * Formula: $f(A) = P f(J) P^{-1}$.
    * **The Magic Formula for $f(J)$:**
        [cite_start]For a $2 \times 2$ Jordan block, $f \begin{pmatrix} \lambda & 1 \\ 0 & \lambda \end{pmatrix} = \begin{pmatrix} f(\lambda) & f'(\lambda) \\ 0 & f(\lambda) \end{pmatrix}$[cite: 1827].
        * *Example:* If $f(x) = \log(1-x)$, then $f'(x) = \frac{-1}{1-x}$. Plug $\lambda$ into both.

**Variation Handling (Different Function):**
* **If $f(x) = e^x$:** $f'(x) = e^x$. The block is $\begin{pmatrix} e^\lambda & e^\lambda \\ 0 & e^\lambda \end{pmatrix}$.
* **If $f(x) = x^{100}$:** $f'(x) = 100x^{99}$. The block is $\begin{pmatrix} \lambda^{100} & 100\lambda^{99} \\ 0 & \lambda^{100} \end{pmatrix}$.

| **Step**                         | **General Recipe**                                                                                                                                  | **Practice Final (Problem 3)**                                                                                                                                                                           | **Exercise 2 (Just Solved)**                                                                                                                                                     |
| -------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Check Bucket**              | Solve char. polynomial. Look for **repeated eigenvalue** $\lambda$. If matrix $\neq \lambda I$, it's Jordan.                                        | **Polynomial:** $(\lambda - 1/2)^2 = 0$.<br><br>  <br><br>**Result:** Repeated $\lambda = 1/2$.<br><br>  <br><br>**Check:** Matrix is not diagonal.                                                      | **Polynomial:** $(\lambda - 3)^2 = 0$.<br><br>  <br><br>**Result:** Repeated $\lambda = 3$.<br><br>  <br><br>**Check:** Matrix is not diagonal.                                  |
| **2. The "Magic" Matrix $f(J)$** | Use the shortcut formula:<br><br>  <br><br>$f(J) = \begin{pmatrix} f(\lambda) & f'(\lambda) \\ 0 & f(\lambda) \end{pmatrix}$                        | **Function:** Likely a series (e.g., $(I-A)^{-1}$ or geometric).<br><br>  <br><br>**Formula:** Used derivation to prove the top-right corner is the derivative.                                          | **Function:** $f(x) = e^x$.<br><br>  <br><br>**Calculation:** $f(3) = e^3, f'(3) = e^3$.<br><br>  <br><br>**Matrix:** $e^3 \begin{pmatrix} 1 & 1 \\ 0 & 1 \end{pmatrix}$         |
| **3. Basis Chain**               | 1. Pick generator $w$ (simple, not eigenvector).<br><br>  <br><br>2. Calculate $v = (A - \lambda I)w$.<br><br>  <br><br>3. Build $P = (v \quad w)$. | **Generator:** Pick vector $w$.<br><br>  <br><br>**Calc:** Multiply to get eigenvector $v$.<br><br>  <br><br>**Matrix:** $P = \begin{pmatrix} -1 & 1 \\ 0.5 & 0 \end{pmatrix}$ (using $w=\binom{1}{0}$). | **Generator:** Picked $w = \binom{1}{0}$.<br><br>  <br><br>**Calc:** $(B - 3I)w = \binom{1}{1}$.<br><br>  <br><br>**Matrix:** $P = \begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix}$ |
| **4. Final Math**                | Calculate $f(A) = P \cdot f(J) \cdot P^{-1}$.                                                                                                       | **Inverse:** Found $P^{-1}$.<br><br>  <br><br>**Multiply:** Sandwich the magic matrix between $P$ and $P^{-1}$.                                                                                          | **Inverse:** Found $P^{-1} = \begin{pmatrix} 0 & 1 \\ 1 & -1 \end{pmatrix}$.<br><br>  <br><br>**Multiply:** $e^3 P \begin{pmatrix} 1 & 1 \\ 0 & 1 \end{pmatrix} P^{-1}$          |

---

### **Recipe 4: Proofs about $AB$ vs $BA$ (Practice Final Prob 4)**
**The Goal:** Prove a relationship between eigenvectors/eigenvalues of $AB$ and $BA$.

**The Logic:**
* Start with the assumption: $v$ is an eigenvector of $AB$.
    * Equation: $ABv = \lambda v$.
* Multiply both sides by $B$ on the **left**.
    * $B(ABv) = B(\lambda v)$.
    * $(BA)(Bv) = \lambda (Bv)$.
* [cite_start]**Conclusion:** The vector $(Bv)$ is an eigenvector of $BA$ with the same eigenvalue $\lambda$[cite: 2162].
* *Sanity Check:* You must state that $Bv \neq 0$ (because if the vector is zero, it can't be an eigenvector). [cite_start]Since $\lambda \neq 0$ and $v \neq 0$, $ABv \neq 0$, so $Bv$ can't be zero[cite: 2163].

**Variation Handling:**
* [cite_start]**Prove $\text{tr}(AB) = \text{tr}(BA)$:** Use the summation definition $\sum_i \sum_k a_{ik} b_{ki} = \sum_k \sum_i b_{ki} a_{ik}$[cite: 1482].
* **Prove $\det(I - AB) = \det(I - BA)$:** (Problem 15 in Set 2). This is a harder identity but relies on similar "multiply by B" logic.

---

### **Recipe 5: Special Matrices ($A^2 = -I$, etc.) (Practice Final Prob 6)**
**The Goal:** Analyze a matrix that satisfies a specific polynomial equation (like $A^2 + I = 0$).

**Step-by-Step Algorithm:**
1.  **Minimal Polynomial:** If $A^2 = -I$, then $A$ is a root of $x^2 + 1 = 0$.
    * This means the eigenvalues must be roots of $x^2 + 1$. [cite_start]So $\lambda = i$ or $\lambda = -i$ [cite: 2186-2187].
2.  **Trace and Determinant:**
    * Pairs: Since $A$ is real, complex eigenvalues come in conjugate pairs ($k$ of $i$, and $k$ of $-i$).
    * [cite_start]Total dimension $n = 2k$ (must be even)[cite: 2184].
    * [cite_start]$\text{Trace} = k(i) + k(-i) = 0$[cite: 2193].
    * [cite_start]$\text{Determinant} = (i)^k (-i)^k = (-i^2)^k = 1^k = 1$[cite: 2196].

**Variation Handling ($A^2 = I$ or $A^2 = A$):**
* **If $A^2 = I$:** Roots of $x^2 - 1 = 0$ are $\lambda = 1, -1$. $A$ is diagonalizable.
* **If $A^2 = A$:** Roots of $x^2 - x = 0$ are $\lambda = 1, 0$. (This is a projection matrix).
* **Strategy:** Always form the polynomial equation (e.g., $x^2 - 1 = 0$), find the roots, and those are your only possible eigenvalues.

---

### **Summary of "Must-Memorize" for the Exam:**
1.  **Block Determinant:** Expand along rows/cols with zeros.
2.  **Complex Eigenbasis:** $P = (\text{Re } v \quad \text{Im } v)$.
3.  **Jordan Basis:** $P = (v \quad w)$ where $(A-\lambda I)w = v$.
4.  **Jordan Function:** $f \begin{pmatrix} \lambda & 1 \\ 0 & \lambda \end{pmatrix} = \begin{pmatrix} f(\lambda) & f'(\lambda) \\ 0 & f(\lambda) \end{pmatrix}$.