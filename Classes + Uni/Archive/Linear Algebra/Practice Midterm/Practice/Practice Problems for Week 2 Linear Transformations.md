### **Revised Problem List for Week 2 (Lectures 3 & 4)**

Here is a new list of problems that is appropriate for your knowledge after Lecture 4. I have replaced the problematic question with one that tests the same concepts but is solvable with the tools you have.

1.  **Verifying Linearity (Adapted from Problem Set 3, Problem 4)**
    Let `T: P₂(ℝ) → P₂(ℝ)` be the transformation defined by `T(p(x)) = p(x-1)`. Is `T` a linear transformation? Prove your answer.
    *(This problem remains the same, as it's purely definition-based.)*

2.  **Identifying Non-Linearity**
    Consider the map `T: C¹(ℝ) → C(ℝ)` defined by `T(f(x)) = x * f'(x) + 1`. Is `T` a linear transformation? If not, provide a specific counterexample.
    *(This problem also remains the same.)*

3.  **Finding Kernel and Image (Revised Problem)**
    Consider the linear transformation `T: ℝ³ → ℝ²` given by `T(x) = Ax`, where
    `A = [[1, -2, 1], [2, -4, 2]]`
    *   **(a)** Find a basis for the kernel of `T` by setting up the system `Ax=0` and solving it using basic substitution.
    *   **(b)** Find a basis for the image of `T`. (Hint: What is the relationship between the columns of A?)
    *   **(c)** Verify that the Rank-Nullity Theorem holds.

    *   **Why this is better**: This matrix is simple enough that you can see the relationship between the columns by inspection (`col₂ = -2*col₁`, `col₃ = col₁`) and solve the system `x₁ - 2x₂ + x₃ = 0` without needing the full machinery of RREF. It tests the concepts of kernel and image without requiring a tool you haven't learned.

4.  **Applying the Rank-Nullity Theorem (Conceptual)**
    *   **(a)** Let `T: ℝ⁵ → ℝ³` be a linear transformation. Can `T` be injective? Explain using the Rank-Nullity Theorem.
    *   **(b)** Suppose `T` is surjective. What must the dimension of its kernel be?
    *(This problem remains the same, as it's purely theoretical.)*

5.  **Finding the Matrix [T] (From Practice Midterm, Problem 4)**
    Let `V = span{e³ˣcos(x), e³ˣsin(x)}` and `β = (e³ˣcos(x), e³ˣsin(x))`. Let `L: V → V` be `L(f) = f' - f`. Find the matrix `[L]β`.
    *(This problem remains, as it's about applying definitions, not solving systems.)*

6.  **Matrix Representation with Polynomials (From Problem Set 4, Problem 2)**
    Let `T: P₂(ℝ) → P₂(ℝ)` be `T(f) = f' - f`. Let `β = {1, x, x²}`. Find `[T]β`.
    *(This problem also remains.)*

7.  **A Proof on Injectivity and Independence (From Problem Set 3, Problem 3.iv)**
    Let `T: V → W` be a linear transformation. Suppose `{T(v₁), ..., T(vₙ)}` is a linearly independent set in `W`. Prove that `{v₁, ..., vₙ}` is a linearly independent set in `V`.
    *(This proof remains, as it relies only on definitions.)*

Thank you for your sharp observation. This revised plan and problem set are much better aligned with the logical progression of the course.