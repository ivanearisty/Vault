## 1. The Goal: The Structure of Solutions

### The System and Definitions

A system of linear equations is written in matrix form as:
$$
Ax = b
$$
* **$A$** is the coefficient matrix.
* **$x$** is the vector of unknown variables.
* **$b$** is the vector of constants.

**Homogeneous vs. Inhomogeneous (Definition 7.1):**
* If $b = 0$, the system $Ax = 0$ is **homogeneous**. The solution set is the **kernel** of $A$, denoted $\text{Ker}(A)$.
* If $b \neq 0$, the system $Ax = b$ is **inhomogeneous**.

### The Structure of Solutions (Lemma 6.20)

The *entire* solution set to an inhomogeneous system $Ax = b$ has the form:
$$
x = x_p + x_h
$$
Where:
* **$x_p$** is **one particular solution** to $Ax = b$.
* **$x_h$** is the **general solution** to the homogeneous equation $Ax = 0$ (i.e., $x_h$ is any vector in $\text{Ker}(A)$).

>[!tip] **Key Takeaway**
>Solving $Ax = b$ is a two-part task: find one particular solution ($x_p$) and then find the entire kernel of $A$ ($x_h$). Gaussian elimination accomplishes both simultaneously.

---

## 2. The Tool: Gaussian Elimination

Gaussian elimination is a systematic procedure to simplify a matrix to its **Row Reduced Echelon Form (RREF)**.

### Core Principle (Lemma 6.21)

The process is built on **elementary row operations** that **do not change the solution set** of the system:
1.  Swapping two rows.
2.  Multiplying a row by a non-zero scalar.
3.  Adding a multiple of one row to another row.

### The Augmented Matrix (Definition 7.5)

The main workspace is the **Augmented Matrix** $\left[ A \ | \ b \right]$. This convenient notation combines $A$ and $b$ so that row operations manipulate both sides of the equations correctly.

---

## 3. The Language: Row Reduced Echelon Form (RREF)

A matrix is in RREF if it satisfies a strict set of conditions.

### The Vocabulary (Definition 7.2)

* **Pivot:** The first non-zero entry in any non-zero row.
* **Pivot Column:** A column that contains a pivot.
* **Free Variables:** The variables corresponding to the **non-pivot columns**. These variables can be chosen freely.

### The Rules for RREF (Definition 7.2)

1.  All zero rows are at the bottom.
2.  Each pivot is to the right of the pivots in the rows above it.
3.  All pivots are equal to $1$.
4.  Each pivot is the **only non-zero entry** in its column.

>[!info] **Connection to Rank**
>The number of pivots in the RREF of $A$ is equal to the **rank** of the matrix.

---

## 4. The Algorithm: Gauss-Jordan Elimination

The full algorithm to reach RREF consists of a forward pass and a backward pass.

### 1. Forward Pass (Left to Right, Top to Bottom)

1.  **Pivot Selection:** Find the leftmost column with a non-zero entry. Swap rows to bring a non-zero entry (the pivot) to the top of the working submatrix.
2.  **Normalize Pivot:** Multiply the pivot's row to make the pivot equal to $1$.
3.  **Eliminate Below Pivot:** Use row addition operations to create zeros in all positions *below* the pivot in that column.
4.  **Move Down/Right:** Ignore the pivot row and columns to its left, and repeat steps 1-3 on the remaining submatrix.

### 2. Backward Pass (Right to Left, Bottom to Top)

5.  **Eliminate Above Pivots:** Starting with the rightmost pivot, use row addition operations to create zeros in all positions *above* each pivot.

---

## 5. The Result: Interpreting the RREF of $\left[ A \ | \ b \right]$

This is the most critical step for the exam.

### Consistency (Theorem 7.6): Does a Solution Exist?

A system is **inconsistent** (no solution) if and only if the RREF of $\left[ A \ | \ b \right]$ contains a row of the form:
$$
\left[ \begin{array}{cccc|c} 0 & 0 & \dots & 0 & 1 \end{array} \right]
$$
This corresponds to the impossible equation $0 = 1$. If this row does not appear, a solution exists (the system is **consistent**).

### Uniqueness: One Solution or Infinitely Many?

Assuming the system is consistent:

* **Exactly one unique solution:** **NO free variables** exist (every column in $A$ is a pivot column).
* **Infinitely many solutions:** **YES, free variables** exist (there is at least one non-pivot column in $A$). The free variables can be chosen arbitrarily.

>[!example] **Exam Expectation**
>You must be able to perform Gaussian elimination on $\left[ A \ | \ b \right]$ and use the RREF to state definitively:
>1.  Whether a solution exists.
>2.  If it exists, whether it is unique or infinite.
>3.  If solutions exist, write the solution set in **parametric vector form** (the $x = x_p + x_h$ structure).