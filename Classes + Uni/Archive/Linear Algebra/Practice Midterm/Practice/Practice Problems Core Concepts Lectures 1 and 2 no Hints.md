#### Category 1: Vector Spaces and Subspaces

These problems test your understanding of the fundamental axioms of a vector space and the three conditions for a set to be a subspace (contains the zero vector, closed under addition, closed under scalar multiplication).

1.  Testing the Axioms (Adapted from Problem Set 1, Problem 1)
    Let `V` be the set of all 2x2 matrices with a trace of zero. The trace of a matrix is the sum of the elements on its main diagonal (tr(A) = $a_{11}$ + $a_{22}$).
    * (a) Show that `V` is a subspace of $M_{2 \times 2}(\mathbb{R})$.
    * (b) Find a basis for `V`.
    * (c) What is the dimension of `V`?

2.  Identifying Non-Subspaces (From Problem Set 1, Problem 2)
    Let `S` be the set of all polynomials of *exactly* degree 2. Is `S` a subspace of $P_2(\mathbb{R})$, the vector space of all polynomials of degree at most 2? Justify your answer.

---

#### Category 2: Span and Linear Independence

These problems focus on the building blocks of vector spaces: how vectors combine (span) and whether a set of vectors is efficient or redundant (linear independence).

3.  Checking Span (Computational)
    Does the vector `b = (2, -7, 3)` lie in the span of the vectors $v_1 = (1, -2, 2)$ and $v_2 = (0, 5, 5)$ in $\mathbb{R}^3$?

4.  Determining Linear Independence (Computational)
    Determine if the following set of vectors in $\mathbb{R}^3$ is linearly independent:
    `{ (1, 2, 3), (4, 5, 6), (2, 1, 0) }`

5.  Linear Independence of Functions (From Problem Set 2, Problem 2)
    Show that the functions `cos(x)` and `sin(x)` are linearly independent in the vector space C($\mathbb{R}$) of continuous functions.

6.  A Proof on Linear Dependence (From Problem Set 2, Problem 1.iv)
    Let $\{v_1, ..., v_n\}$ be a linearly independent set of vectors in `V`. Suppose `w` is a vector in `V` such that the set $\{v_1 + w, v_2 + w, ..., v_n + w\}$ is linearly dependent. Prove that `w` must be in the span of $\{v_1, ..., v_n\}$.

---

#### Category 3: Basis and Dimension

These problems combine the concepts of span and linear independence to test your ability to find the "skeleton" of a vector space.

7.  Finding a Basis and Dimension (From Lecture 2, Example 2.19)
    Let `V` be the subspace of $\mathbb{R}^3$ defined by `V = { (a₁, a₂, a₃) | a₁ + a₂ + a₃ = 0 }`. Find a basis for `V` and state its dimension.

8.  Extending a Basis (Conceptual)
    The vectors $v_1 = (1, 1, 0, 0)$ and $v_2 = (0, 1, 1, 0)$ are linearly independent in $\mathbb{R}^4$. Find two more vectors $v_3$ and $v_4$ such that $\{v_1, v_2, v_3, v_4\}$ forms a basis for $\mathbb{R}^4$.

#### Category 4: Synthesis and Abstract Proof

This problem is at the level of the practice midterm and requires you to synthesize all the Week 1 concepts to write a formal proof.

9.  Abstract Proof (From Practice Midterm, Problem 1)
    Suppose `W` is a subspace of `V` and $\{v_1 + W, ..., v_m + W\}$ is a basis of the quotient space `V/W`. Let $\{w_1, ..., w_n\}$ be a basis of `W`. Show that the combined set $\{v_1, ..., v_m, w_1, ..., w_n\}$ is a basis of `V`.