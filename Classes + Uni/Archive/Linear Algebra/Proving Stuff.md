# How to choose a proof strategy

1. Read the logical form

* “For all …, show …” → Direct proof. Start with an arbitrary object and use definitions/axioms to reach the claim.
* “If and only if” → Prove two directions separately (“⇒” and “⇐”).
* “There exists …” → Construct it explicitly (give a formula or coordinates) and then verify it works.
* “There exists a unique …” → Do both: (i) existence by construction; (ii) uniqueness by assuming two candidates and proving they’re equal.
* “Not …” or “no such …” → Often contradiction is natural: assume one exists and derive an axiom-violating consequence.
* Set equality (e.g., two line descriptions are the same set) → Prove mutual inclusion: show A ⊆ B and B ⊆ A.

2. Decide: coordinate or axiomatic?

* In $\mathbb{R}^n$: You may use component-wise definitions (add by components, scale by components). This is fast and concrete.
* In abstract vector spaces (next sections): You must use only the vector-space axioms (associativity, commutativity, identities, inverses, distributivity, etc.). No coordinates unless given a basis.

# Go-to proof patterns (with tiny templates)

Direct proof (universal statements)

* Goal: $\forall x,\ P(x)\Rightarrow Q(x)$.
* Template:
  “Let $x$ be arbitrary with $P(x)$. By definition … (unpack operations). Using axioms/known lemmas, … hence $Q(x)$. Since $x$ was arbitrary, we’re done.”

Existence (construct-and-check)

* Goal: “There exists $v$ with property $P(v)$.”
* Template:
  “Define $v := (\text{your formula})$. Then compute/argue to show $P(v)$ holds.”

Uniqueness

* Goal: “There exists a unique $v$ with $P(v)$.”
* Template:
  Existence: construct $v$ and verify $P(v)$.
  Uniqueness: suppose $v$ and $w$ both satisfy $P$. Show $v=w$ (often by subtracting or comparing components).

Biconditional (“iff”)

* Prove both directions with the most natural tools for each.
  Example: “Vectors $u,v$ are parallel iff $v=tu$ for some scalar $t$.”
  (⇒) If parallel, argue geometrically or with direction vectors to get a scalar multiple.
  (⇐) If $v=tu$, then $u$ and $v$ point along the same line.

Contrapositive / Contradiction

* Contrapositive: prove “not $Q$ ⇒ not $P$” instead of “$P$ ⇒ $Q$” when it’s cleaner.
* Contradiction: assume the statement is false, use axioms to create an impossibility (e.g., produce two different “zero vectors”).

Set equality (lines/planes)

* To show two parametric descriptions define the same line/plane, take an arbitrary point from one description and algebraically solve for parameters to express it in the other; repeat in reverse.

# What to actually write (typical early tasks)

Zero vector (additive identity) in $\mathbb{R}^2$

* Existence (construct): $\mathbf{0}=(0,0)$.
  Check: $(a_1,a_2)+(0,0)=(a_1,a_2)$.
* Uniqueness: If $\mathbf{z}$ also satisfies $x+\mathbf{z}=x$ for all $x$, plug $x=\mathbf{0}$: $\mathbf{0}+\mathbf{z}=\mathbf{0}\Rightarrow \mathbf{z}=\mathbf{0}$.

Parallel vectors in $\mathbb{R}^3$

* “$u$ and $v$ are parallel iff $v=tu$.” Use an “iff” proof. In coordinates, check proportional components (careful with zeros).

Collinearity of points $P,Q,R$

* Vectors $Q-P$ and $R-P$ are parallel. Use the parallel “iff” pattern.

Line through $P,Q$

* Existence: write $x=P+t(Q-P)$; check it passes through both and is closed under affine combinations.
* Uniqueness of description: if you get a different base point and direction, show each point on one form can be reparametrized to the other (mutual inclusion).

Plane through $P,Q,R$ (non-collinear)

* Construct $x=P+s(Q-P)+t(R-P)$.
* To match another description, solve for $(s,t)$ or show both spans are the same (again by mutual inclusion).

# Micro-checklist before you start

* What is given? (point(s), vector(s), scalar(s), equations) What must be shown (equation, inclusion, uniqueness)?
* Which definitions/axioms unlock this? (component-wise add/scale, axioms like identities/inverses)
* Is a construction obvious? (then do existence) Is there “unique”? (add a short uniqueness step)
* Is it an “iff”? (split directions)
* In $\mathbb{R}^n$, can I finish quickly by components?

# Common moves you’ll reuse a lot

* Equality of vectors is componentwise equality.
* To prove two vectors are equal, show their difference is the zero vector.
* For uniqueness: start from “assume two objects satisfy the defining property,” then subtract/compare until they coincide.
* When stuck on set equality, switch to “⊆ both ways.”
