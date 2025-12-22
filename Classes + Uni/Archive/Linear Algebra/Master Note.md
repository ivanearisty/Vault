# Lecture 1

**1) Vector spaces (axioms, basic consequences)**

* Uniqueness of Zero and Additive Inverses
* Cancellation Law (x + z = y + z ⇒ x = y)
* Scalar-0 and 0-Vector Identities (0·v = 0, a·0 = 0)
* Negation/Distributive Identities (−1·v = −v)

**2) Subspaces (definition + examples)**

* Subspace Test (aka Subspace Criterion)
* Intersection of Subspaces is a Subspace
* Span is a Subspace (and is the smallest subspace containing a set)

**3) Span & linear combinations**

* Characterization of Span (as all finite linear combinations)
* “Smallest Subspace Containing S” Property of Span
* Redundancy Lemma (removing a vector that’s a combo of others preserves span)

**4) Linear dependence vs. independence**

* Linear Dependence Criterion (a set is dependent ⇔ one vector is a linear combo of the others)
* Nonzero Singleton is Independent
* Subset of an Independent Set is Independent
* Exchange Lemma (basic form for two vectors/short sets)

# Lecture 2

**1) Span (revisited, with examples)**

* Trimming a Spanning Set to a Basis (remove redundant vectors)
* Extending Independent Sets to Bases

**2) Linear dependence & independence (characterizations)**

* Steinitz Exchange Lemma (Replacement Theorem)
* Size Bounds: Independent Sets ≤ any Basis (in finite dimension)

**3) Basis (definition, trimming; replacement)**

* All Bases Have the Same Cardinality (Dimension is Well-Defined)
* Basis Extension (any independent set can be extended to a basis)
* Basis Reduction (any spanning set can be reduced to a basis)

**4) Intersection, sum, and direct sum of subspaces (and dimension formula)**

* Direct Sum Criterion: V = U ⊕ W ⇔ V = U + W and U ∩ W = {0}
* Grassmann (Dimension) Formula: dim(U + W) = dim U + dim W − dim(U ∩ W)
* Dimension Bounds for Subspaces (dim W ≤ dim V)

# Lecture 3

**1) Sum, direct sum, and the dimension formula for sums**

* Grassmann Formula (applications & corollaries)
* Direct Sum Dimension Additivity: dim(U ⊕ W) = dim U + dim W

**2) Quotient space (cosets, well-defined ops, dim(V/W))**

* Quotient Space Well-Defined (vector space structure on V/W)
* Dimension of a Quotient: dim(V/W) = dim V − dim W (finite-dimensional V)

**3) Linear transformations (definition + core examples)**

* Kernel and Image are Subspaces
* Rank–Nullity Theorem: dim V = rank(T) + nullity(T)
* First Isomorphism Theorem (Vector Spaces): V/ker T ≅ im T

**4) Operations on linear transformations (addition, scalar mult., L(V,W))**

* L(V, W) is a Vector Space (closure under + and scalar mult.)
* Linearity of Composition; Associativity of Composition
* “Determined by a Basis” Theorem (a linear map is fixed by its values on a basis)