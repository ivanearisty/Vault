#### Category 1: Vector Spaces and Subspaces

These problems test your understanding of the fundamental axioms of a vector space and the three conditions for a set to be a subspace (contains the zero vector, closed under addition, closed under scalar multiplication).

1.  **Testing the Axioms (Adapted from Problem Set 1, Problem 1)**
    Let `V` be the set of all 2x2 matrices with a trace of zero. The trace of a matrix is the sum of the elements on its main diagonal (tr(A) = a₁₁ + a₂₂).
    *   **(a)** Show that `V` is a subspace of M₂ₓ₂(ℝ).
    *   **(b)** Find a basis for `V`.
    *   **(c)** What is the dimension of `V`?

    *   **What this tests**: Your ability to verify the three subspace properties (zero vector, closure under addition, closure under scalar multiplication) in a context other than ℝⁿ. It also connects the idea of a subspace to basis and dimension.

2.  **Identifying Non-Subspaces (From Problem Set 1, Problem 2)**
    Let `S` be the set of all polynomials of *exactly* degree 2. Is `S` a subspace of P₂(ℝ), the vector space of all polynomials of degree at most 2? Justify your answer.

    *   **What this tests**: Careful reading of definitions. This problem is a classic example that hinges on whether the set contains the zero vector and is closed under addition.

---

#### **Category 2: Span and Linear Independence**

These problems focus on the building blocks of vector spaces: how vectors combine (span) and whether a set of vectors is efficient or redundant (linear independence).

3.  **Checking Span (Computational)**
    Does the vector `b = (2, -7, 3)` lie in the span of the vectors `v₁ = (1, -2, 2)` and `v₂ = (0, 5, 5)` in ℝ³?

    *   **What this tests**: Your ability to set up and solve a system of linear equations to determine if one vector is a linear combination of others.

4.  **Determining Linear Independence (Computational)**
    Determine if the following set of vectors in ℝ³ is linearly independent:
    `{ (1, 2, 3), (4, 5, 6), (2, 1, 0) }`

    *   **What this tests**: Your ability to set up a homogeneous system of equations (`c₁v₁ + c₂v₂ + ... = 0`) and determine if it has a non-trivial solution, typically by using row reduction.

5.  **Linear Independence of Functions (From Problem Set 2, Problem 2)**
    Show that the functions `cos(x)` and `sin(x)` are linearly independent in the vector space C(ℝ) of continuous functions.

    *   **Hint**: Start with the equation `c₁cos(x) + c₂sin(x) = 0`. This equation must hold for *all* values of `x`. Try plugging in specific, convenient values for `x` (like `x=0` or `x=π/2`) to show that `c₁` and `c₂` must be zero.
    *   **What this tests**: Your understanding that linear independence is a general concept and requires different proof techniques in different vector spaces.

6.  **A Proof on Linear Dependence (From Problem Set 2, Problem 1.iv)**
    Let `{v₁, ..., vₙ}` be a linearly independent set of vectors in `V`. Suppose `w` is a vector in `V` such that the set `{v₁ + w, v₂ + w, ..., vₙ + w}` is linearly dependent. Prove that `w` must be in the span of `{v₁, ..., vₙ}`.

    *   **What this tests**: Your ability to work with the definitions of linear independence and span to construct a formal proof.

---

#### **Category 3: Basis and Dimension**

These problems combine the concepts of span and linear independence to test your ability to find the "skeleton" of a vector space.

7.  **Finding a Basis and Dimension (From Lecture 2, Example 2.19)**
    Let `V` be the subspace of ℝ³ defined by `V = { (a₁, a₂, a₃) | a₁ + a₂ + a₃ = 0 }`. Find a basis for `V` and state its dimension.

    *   **What this tests**: A core skill. You must find a set of vectors that is linearly independent and spans the entire subspace.

8.  **Extending a Basis (Conceptual)**
    The vectors `v₁ = (1, 1, 0, 0)` and `v₂ = (0, 1, 1, 0)` are linearly independent in ℝ⁴. Find two more vectors `v₃` and `v₄` such that `{v₁, v₂, v₃, v₄}` forms a basis for ℝ⁴.

    *   **Hint**: A simple way is to test vectors from the standard basis (`e₁`, `e₂`, `e₃`, `e₄`) one by one to see if they are in the span of the vectors you already have. If a vector is not in the span, you can add it to your set.
    *   **What this tests**: Your understanding of Corollary 2.16, which states that any linearly independent set can be extended to a basis.

---

#### **Category 4: Synthesis and Abstract Proof**

This problem is at the level of the practice midterm and requires you to synthesize all the Week 1 concepts to write a formal proof.

9.  **Abstract Proof (From Practice Midterm, Problem 1)**
    Suppose `W` is a subspace of `V` and `{v₁ + W, ..., vₘ + W}` is a basis of the quotient space `V/W`. Let `{w₁, ..., wₙ}` be a basis of `W`. Show that the combined set `{v₁, ..., vₘ, w₁, ..., wₙ}` is a basis of `V`.

    *   **Hint**: You must prove two things separately:
        1.  The set spans `V`. (Take an arbitrary `v` in `V` and show it can be written as a linear combination of the vectors in the set).
        2.  The set is linearly independent. (Set a linear combination equal to zero and show all the scalar coefficients must be zero).
    *   **What this tests**: Your ability to work with the formal definitions of basis, span, and linear independence in a more abstract setting, which is a key skill for success on the exam.