You are absolutely right. For a final exam, you need **computational tools** and **theorems that justify your steps**, not the theoretical proofs like bubble sort permutations.

Here is the "Must-Know" list for Lectures 8–13, filtered for exam relevance.

### **Lecture 8: Determinants**
* **Definition 7.12 & Lemma 7.13 (Basic Properties):** Know that the determinant is multilinear and alternating. [cite_start]This is the logic you use when doing row operations to compute a determinant[cite: 1363, 1371].
* **Corollary 8.6 (Row Operations):** This is the most practical tool. [cite_start]Know how swapping rows, scaling rows, and adding rows affects the determinant[cite: 1449].
* **Proposition 8.14 (Block Matrices):** Extremely useful shortcut. [cite_start]If you see a block upper triangular matrix, the determinant is just the product of the determinants of the diagonal blocks[cite: 1487].
* **Theorem 8.19 (Laplace Expansion):** You must know this formula to expand along a specific row or column. [cite_start]This is your backup if row reduction gets messy[cite: 1523].
* **Corollary 8.22 (Inverse Formula):** Know that $A^{-1} = \frac{1}{\det(A)} (\text{Cof}(A))^t$. [cite_start]You might need this for symbolic inverses[cite: 1538].
* [cite_start]**Skip:** The Leibniz formula (Theorem 8.1) and the bubble sort proof (Lemma 8.4)[cite: 1390, 1407].

### **Lecture 9: Eigenvalues & Eigenvectors**
* **Definition 9.18 (Eigenvalues/Vectors):** The core definition $Av = \lambda v$. [cite_start]Everything builds on this[cite: 1623].
* [cite_start]**Proposition 9.20 & Definition 9.21 (Characteristic Polynomial):** Know $p_A(x) = \det(xI - A)$ and how to find roots to get eigenvalues[cite: 1630, 1634].
* **Definition 9.21 (Multiplicities):** Crucial distinction.
    * [cite_start]**Geometric Multiplicity:** Dimension of the eigenspace ($\dim \ker(A - \lambda I)$)[cite: 1637].
    * [cite_start]**Algebraic Multiplicity:** How many times the root appears in the polynomial[cite: 1638].
* [cite_start]**Examples 9.22, 9.23, 9.24:** Read these to see the standard workflow for finding eigenvalues in real vs. complex fields[cite: 1640, 1653, 1657].

### **Lecture 10: Diagonalizability**
* **Theorem 10.9 (Independence of Eigenvectors):** Eigenvectors from *distinct* eigenvalues are linearly independent. [cite_start]This justifies why distinct eigenvalues imply diagonalizability[cite: 1744].
* **Theorem 10.11 (Diagonalizability Test):** **Memorize this.** A matrix is diagonalizable if and only if:
    1.  The characteristic polynomial splits (factors completely).
    2.  [cite_start]Geometric Multiplicity = Algebraic Multiplicity for *every* eigenvalue[cite: 1779].
* [cite_start]**Corollary 10.12 (Shortcut):** If $A$ has $n$ distinct eigenvalues, it is automatically diagonalizable[cite: 1798].
* **Proposition 10.6 (Trace):** Know that $\text{tr}(AB) = \text{tr}(BA)$. [cite_start]This is a common trick for checking calculations[cite: 1729, 1731].

### **Lecture 11: Minimal Polynomial & Cayley-Hamilton**
* **Theorem 11.5 (Cayley-Hamilton):** $p_A(A) = 0$. [cite_start]Useful for simplifying powers of matrices (e.g., $A^n$)[cite: 1839].
* **Theorem 11.10 (Minimal Polynomial):** Know definition of $m_A(x)$. [cite_start]It is the smallest degree polynomial where $m_A(A)=0$[cite: 1889].
* [cite_start]**Proposition 11.13 (Factors):** $m_A(x)$ and $p_A(x)$ have the exact same roots (eigenvalues), just potentially different powers[cite: 1914].
* [cite_start]**Theorem 11.14 (The "Best" Diag Test):** $A$ is diagonalizable **if and only if** $m_A(x)$ has **no repeated factors** (i.e., $(x-1)(x-2)$ is okay, but $(x-1)^2$ is not)[cite: 1926].
* [cite_start]**Example 11.12:** Read this to understand why the Jordan block $(x-\lambda)^n$ is the "worst case" scenario (not diagonalizable)[cite: 1906].

### **Lecture 12: Jordan Canonical Form (JCF)**
* [cite_start]**Theorem 12.4 (JCF Existence):** Know that every matrix is similar to a Jordan matrix (block diagonal)[cite: 1974].
* **Theorem 12.8 (2x2 JCF):** **Must Read.** This tells you exactly how to handle the $2\times2$ case where eigenvalues are repeated but it's not diagonalizable. [cite_start]You need to find a generalized eigenvector[cite: 1991].
* **Theorem 12.10 (Real Canonical Form):** Know the form $\begin{pmatrix} a & b \\ -b & a \end{pmatrix}$ for complex eigenvalues $a \pm bi$. [cite_start]This appears often in ODE problems[cite: 2009].
* [cite_start]**Corollary 12.18 (Matrix Exp):** Definition of $e^A$[cite: 2101].
* **Example 12.19 & Proposition 12.17(ii):** Know the formula for the exponential of a Jordan block. [cite_start]It results in a matrix with terms like $\frac{t^k}{k!}e^{\lambda t}$ in the upper triangle[cite: 2046, 2113].

### **Lecture 13: Matrix Exponentials & ODEs**
* [cite_start]**Theorem 13.1 (Properties):** Specifically property (ii) $\det(e^A) = e^{\text{tr}(A)}$ and (iv) the derivative rule[cite: 2124].
* **Corollary 13.4 (ODE Solution):** The solution to $x' = Ax$ is $x(t) = e^{tA}x(0)$. [cite_start]This is the main application problem you will likely solve[cite: 2153].
* **Example 13.5:** Read this carefully. It walks through solving a second-order ODE by converting it to a matrix system. [cite_start]It covers the three cases: distinct real roots, repeated roots (Jordan case), and complex roots[cite: 2157].

# **Linear Algebra Final Exam Master Note (Lectures 8–13)**

## **Lecture 8: Determinants**
**Core Concept:** The determinant is a scalar value that determines if a matrix is invertible.
* **Properties:**
    * [cite_start]**Normalization:** $\det(I) = 1$[cite: 1010].
    * [cite_start]**Alternating:** Swapping two columns (or rows) flips the sign ($\times -1$)[cite: 1015].
    * [cite_start]**Multilinear:** Linear in each column individually[cite: 1008].
    * [cite_start]**Invertibility:** Matrix $A$ is invertible if and only if $\det(A) \neq 0$[cite: 1115].
    * [cite_start]**Multiplicativity:** $\det(AB) = \det(A)\det(B)$[cite: 1107].
    * [cite_start]**Transpose:** $\det(A^t) = \det(A)$[cite: 1123].

**Computing Determinants:**
1.  **Row Reduction (Efficient for large matrices):**
    * Reduce $A$ to an upper triangular matrix $R$.
    * [cite_start]$\det(A) = (-1)^k (\text{product of diagonal entries of } R)$, where $k$ is the number of row swaps performed[cite: 1133, 1093].
    * [cite_start]*Note:* Adding a multiple of one row to another does **not** change the determinant[cite: 1095]. [cite_start]Scaling a row by $c$ multiplies the determinant by $c$[cite: 1094].
2.  **Laplace Expansion (Recursive):**
    * [cite_start]Formula: $\det(A) = \sum_{i=1}^n (-1)^{i+j} a_{ij} \det(A_{ij})$ (expansion along $j$-th column)[cite: 1167].
    * [cite_start]$A_{ij}$ is the $(i, j)$-minor (submatrix deleting row $i$, column $j$)[cite: 1163].

---

## **Lecture 9: Eigenvalues & Eigenvectors**
**Definitions:**
* [cite_start]**Eigenvalue/Eigenvector:** $Av = \lambda v$ where $v \neq 0$[cite: 1266, 1269].
* [cite_start]**Characteristic Polynomial:** $p_A(x) = \det(xI - A)$[cite: 1277].
    * [cite_start]Roots of $p_A(x)$ are the eigenvalues[cite: 1277].
* [cite_start]**Eigenspace:** $E(\lambda, A) = \ker(\lambda I - A)$[cite: 1279].

**Multiplicities:**
1.  [cite_start]**Algebraic Multiplicity:** The power $k$ of the factor $(x - \lambda)^k$ in $p_A(x)$[cite: 1281].
2.  [cite_start]**Geometric Multiplicity:** The dimension of the eigenspace, $\dim E(\lambda, A)$[cite: 1280].
    * [cite_start]*Theorem:* $1 \le \text{Geometric Multiplicity} \le \text{Algebraic Multiplicity}$[cite: 1412].

**Cramer's Rule:**
* [cite_start]If $A$ is invertible, the solution to $Ax=b$ is $x_i = \frac{\det(A_i(b))}{\det(A)}$, where $A_i(b)$ is matrix $A$ with column $i$ replaced by $b$[cite: 1190].

---

## **Lecture 10: Diagonalizability**
[cite_start]**Definition:** A matrix $A$ is diagonalizable if it is similar to a diagonal matrix ($P^{-1}AP = D$)[cite: 1270].
* [cite_start]This is equivalent to finding a basis of $F^n$ consisting entirely of eigenvectors[cite: 1267].

**Diagonalizability Test:**
$A$ is diagonalizable **if and only if**:
1.  [cite_start]$p_A(x)$ splits (factors completely into linear terms)[cite: 1426], **AND**
2.  [cite_start]For every eigenvalue $\lambda$, **Geometric Multiplicity = Algebraic Multiplicity**[cite: 1426].
    * [cite_start]*Shortcut:* If $A$ has $n$ **distinct** eigenvalues, it is automatically diagonalizable[cite: 1441].

**Trace:**
* $\text{tr}(A) = \sum a_{ii}$ (sum of diagonal entries).
* [cite_start]$\text{tr}(A) = \sum \text{eigenvalues}$[cite: 1352].
* [cite_start]$\text{tr}(AB) = \text{tr}(BA)$[cite: 1374].

---

## **Lecture 11: Cayley-Hamilton & Minimal Polynomial**
**Cayley-Hamilton Theorem:**
* [cite_start]Every matrix satisfies its own characteristic equation: $p_A(A) = 0$[cite: 1482].

**Minimal Polynomial ($m_A(x)$):**
* [cite_start]The unique monic polynomial of **least degree** such that $m_A(A) = 0$[cite: 1537, 1534].
* **Properties:**
    1.  [cite_start]If $f(A) = 0$, then $m_A(x)$ divides $f(x)$[cite: 1536].
    2.  [cite_start]$m_A(x)$ and $p_A(x)$ share the **same roots** (eigenvalues)[cite: 1560].
    3.  [cite_start]$m_A(x)$ divides $p_A(x)$[cite: 1538].

**Diagonalizability Test (via Minimal Polynomial):**
* [cite_start]$A$ is diagonalizable **if and only if** $m_A(x)$ splits into **distinct linear factors** (no repeated roots like $(x-\lambda)^2$)[cite: 1569].

---

## **Lecture 12: Jordan Canonical Form (JCF)**
**Concept:** If $A$ is not diagonalizable, the "simplest" form it can reach is JCF.
* [cite_start]**Theorem:** Every complex matrix is similar to a block diagonal matrix of Jordan Blocks[cite: 1618].
    * $J = \begin{pmatrix} J_{k_1}(\lambda_1) & & 0 \\ & \ddots & \\ 0 & & J_{k_m}(\lambda_m) \end{pmatrix}$.

**Jordan Block $J_k(\lambda)$:**
* [cite_start]A $k \times k$ matrix with $\lambda$ on the diagonal and $1$s on the super-diagonal (immediately above diagonal)[cite: 1550].
    $$J_k(\lambda) = \begin{pmatrix} \lambda & 1 & 0 \\ 0 & \lambda & 1 \\ 0 & 0 & \lambda \end{pmatrix}$$

**Real Canonical Form ($2 \times 2$):**
[cite_start]If a real matrix $A$ has complex eigenvalues $a \pm bi$, it is similar to $\begin{pmatrix} a & b \\ -b & a \end{pmatrix}$[cite: 1653].

**Matrix Functions:**
* [cite_start]To compute $f(A)$ (like $e^A$ or $\sin A$), use the JCF: $f(A) = P f(J) P^{-1}$[cite: 1728].
* For a Jordan block, derivatives of $f$ appear in the upper triangle:
    [cite_start]$f(J_n(\lambda)) = \begin{pmatrix} f(\lambda) & f'(\lambda) & \dots \\ 0 & f(\lambda) & \dots \\ \vdots & \vdots & \ddots \end{pmatrix}$[cite: 1690].

---

## **Lecture 13: Matrix Exponentials & ODEs**
**Matrix Exponential ($e^A$):**
* [cite_start]Definition: $e^A = \sum_{k=0}^{\infty} \frac{A^k}{k!}$[cite: 1744].
* **Key Properties:**
    * [cite_start]$e^0 = I$[cite: 1767].
    * [cite_start]$\det(e^A) = e^{\text{tr}(A)}$[cite: 1769].
    * [cite_start]If $AB = BA$, then $e^{A+B} = e^A e^B$[cite: 1770].
    * [cite_start]$\frac{d}{dt} e^{tA} = A e^{tA}$[cite: 1771].

**Solving Systems of ODEs:**
* [cite_start]For the system $x'(t) = Ax(t)$, the solution is **$x(t) = e^{tA}x(0)$**[cite: 1797].

**Calculation Cases for $e^{tA}$:**
1.  [cite_start]**Diagonalizable:** If $A = PDP^{-1}$, then $e^{tA} = P e^{tD} P^{-1}$ (where $e^{tD}$ just exponentiates the diagonal entries)[cite: 1807].
2.  [cite_start]**Jordan Block:** Use the formula from Lecture 12. For a $2 \times 2$ block $\begin{pmatrix} \lambda & 1 \\ 0 & \lambda \end{pmatrix}$, $e^{tJ} = e^{\lambda t} \begin{pmatrix} 1 & t \\ 0 & 1 \end{pmatrix}$[cite: 1812].
3.  **Complex Eigenvalues ($2 \times 2$):** If $A$ has roots $a \pm bi$, use the real form:
    [cite_start]$e^{tA} = e^{at} \begin{pmatrix} \cos(bt) & \sin(bt) \\ -\sin(bt) & \cos(bt) \end{pmatrix}$ (conjugated by P)[cite: 1817].