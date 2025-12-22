# Lecture Notes

## Vector Spaces

A set V is called a vector space (linear space) over F if it is equipped with addition V × V → V, (x, y) 7 → x + y and scalar multiplication F × V → V, (a, x) 7 → ax

satisfying the following axioms: for any a, b ∈ F , x, y, z ∈ V :
### Axioms

(I) $x + y = y + x$, commutativity
(II) $(x + y) + z = x + (y + z)$, associativity
(III) There exists $0 \in V$ such that $x + 0 = x$, additive identity
(IV) For any $x \in V$, there exists $-x \in V$ such that $x + (-x) = 0$, additive inverse
(V) $a(bx) = (ab)x$, multiplicative associativity
(VI) $1x = x$, multiplicative identity
(VII) $a(x + y) = ax + ay$, distributive property 1
(VIII) $(a + b)x = ax + bx$, distributive property 2

An element $x \in V$ is called a *vector* and an element $a \in F$ is called a *scalar*.

### (i) The element $0 \in V$ is unique.
#Note-for-prof: How can I be sure that I actually proved something without question. For example:

I would've said:
$$
\begin{gather}
\text{Let both } 0 \in V \text{ and } 0' \in V \\
\text{Then } x + 0 = x \text{ and } x + 0' = x \\
\text{Then } x + 0 = x + 0',\\
\text{ if we subtract x from both sides we get:} \\
0 = 0'
\end{gather}
$$

Would this be valid? What rigor do I need to prove? Do I need to call axioms specifically? 

Compare to chatgpt proof:
![[Screenshot 2025-09-20 at 9.23.33 PM.png | 400]]

---

### (ii) For any $x \in V$, $0x = 0$.

**Proof:**
$$
\begin{gather}
0x & \text{start} \\
0x = 0x + \mathbf{0} & \text{additive identity} \\
0x + \mathbf{0} = 0x + (0x + (-0x)) & \text{additive inverse} \\
0x + (0x + (-0x)) = (0x + 0x) + (-0x) & \text{associativity} \\
(0x + 0x) + (-0x) = (0+ 0)x + (-0x) & \text{distributive property 2} \\
(0+ 0)x + (-0x) = 0x+ (-0x) & \text{properties of scalars in } \mathbb{R} \land \mathbb{C} \\
0x+ (-0x) = \mathbf{0} & \text{additive inverse}
\end{gather}
$$

---

### (iii) For any $a \in F$, $a0 = 0$.

**Proof:**
$$
\begin{gather}
a\mathbf{0} & \text{start} \\
a\mathbf{0} = a\mathbf{0} + \mathbf{0} & \text{additive identity} \\
a\mathbf{0} + \mathbf{0} 
\end{gather}
$$

#Note-for-prof isnt 2 a0+0=a0+(a0+(−a0)) exactly what we're trying to prove? how come it's not circular?

![[Screenshot 2025-09-20 at 10.17.22 PM.png ]]

---

### (iv) If $a \in F$, $x \in V$ such that $ax = 0$, then either $a=0$ (in $F$) or $x=0$ (in $V$).

**Proof:**

$$
\begin{gather}
\text{Case 1: } \\
\text{If } v = \mathbf{0}, \text{we know that for any } a \in F, a\mathbf{0} = \mathbf{0} \text{ (from ) iii} \\
\\
\text{Case 2:} \\
\text{If } v \neq \mathbf{0} \text{ we must show that } a \text{ must be } 0. \\
av = \mathbf{0} \\
av = v + (-v)
\end{gather}
$$

---

### (v) For any $x \in V$, the element $-x$ is unique.

**Proof:**
$$
\begin{gather}
\text{Let } (-x) \text{ and } (-x)'\\
x + (-x) = \mathbf{0} \\
x + (-x)' = \mathbf{0} \\
(-x) = \mathbf{0} - x \\
(-x)' = \mathbf{0} - x\\
\therefore (-x) = (-x)'
\end{gather}
$$

The axioms we have only define addition and scalar multiplication. Subtraction is an operation that we typically _define_ later as a−b=a+(−b). Since this definition itself relies on the existence of a unique inverse (−b), using it in the proof would be circular reasoning.

A more rigorous proof will stick only to the axioms we've been given.

![[Screenshot 2025-09-20 at 11.02.14 PM.png]]

---

### (vi) For any $x \in V$, $(-1)x = -x$.

**Proof:**
How does this not just follow?

---

### (vii) If $x+z = y+z$, then $x=y$.

**Proof:**
$$
\begin{gather}
x + z = y + z \\
x + z + (-z) = y + z + (-z) \\
z + (-z) = \mathbf{0} \\
x + \mathbf{0} = x \\
y + \mathbf{0} = y \\ \\
x = y
\end{gather}
$$
## 1.6

The main idea is that sets of well-behaved functions (in this case, continuous and differentiable ones) follow the exact same 8 rules of a vector space that lists of numbers ($F^n$) and matrices do.

This notation describes the specific set of functions we're dealing with:

* **$C$** stands for **Continuous**.
* **$k$** is a number that tells you how many times you can **differentiate** the function and still have the result be a continuous function.
* **$(\mathbb{R})$** means the functions map real numbers to real numbers ($f: \mathbb{R} \to \mathbb{R}$).

#### Examples:
* **$C^0(\mathbb{R})$**: This is the set of all functions that are simply **continuous**. The absolute value function, $f(x)=|x|$, is in this set. .
* **$C^1(\mathbb{R})$**: This is the set of functions that you can differentiate once, and the resulting derivative is *also* a continuous function. For example, $f(x) = x^3$ is in $C^1(\mathbb{R})$ because its derivative, $f'(x) = 3x^2$, is a smooth, continuous parabola.
* The function $f(x)=|x|$ is in $C^0(\mathbb{R})$ but it is **not** in $C^1(\mathbb{R})$ because its derivative has a "jump" at $x=0$ and is not continuous there.

To treat this set of functions as a vector space, we define addition and scalar multiplication in a "pointwise" manner:

* **Function Addition:** The sum of two functions, $f$ and $g$, is a new function `(f+g)` whose value at any point `x` is just the sum of the individual function values at that point: `(f+g)(x) = f(x) + g(x)`. Visually, you're just adding the heights of the two graphs at every point. .
* **Scalar Multiplication:** To multiply a function `f` by a scalar `c`, you create a new function `(cf)` whose value at `x` is `c` times the value of `f(x)`: `(cf)(x) = c * f(x)`. Visually, this stretches or shrinks the graph of the function vertically.

In this function vector space:
* The **"zero vector"** is the zero function, $f(x) = 0$.
* The **additive inverse** of a function $f(x)$ is the function $-f(x)$.

This concept is huge in physics and engineering, as it allows the powerful tools of linear algebra to be used to solve problems involving functions, like differential equations.


## Subspaces
![[Screenshot 2025-09-21 at 1.40.32 AM.png]]

# Introduction

The familiar “vector rules” (add tip-to-tail, scale to stretch/shrink, parallelogram picture, etc.) work the same in 3-D space as in the plane. Using those rules, we can write compact vector equations for lines and planes.
## A line through two points $P$ and $Q$

* Let $\mathbf{u}=\overrightarrow{OP}$ and $\mathbf{v}=\overrightarrow{OQ}$ be the **position vectors** of $P$ and $Q$.
* The direction from $P$ to $Q$ is $\mathbf{w}=\overrightarrow{PQ}=\mathbf{v}-\mathbf{u}$ (difference of coordinates).
* Any point $X$ on the line through $P$ and $Q$ is

  $$
  \mathbf{x}=\mathbf{u}+t\,\mathbf{w}
  \;=\; \mathbf{u}+t(\mathbf{v}-\mathbf{u})
  \;=\; P+t(Q-P),\qquad t\in\mathbb{R}.
  $$

  This is the **vector/parametric equation** of the line.
  (If you prefer coordinates: if $P=(x_1,y_1,z_1)$ and $Q=(x_2,y_2,z_2)$, then the direction is $(x_2-x_1,\; y_2-y_1,\; z_2-z_1)$.)

**Example (from the text).**
$P=(-2,0,1),\; Q=(4,5,3)$.
Direction $Q-P=(6,5,2)$.
Line: $\displaystyle \mathbf{x}=(-2,0,1)+t(6,5,2)$.
Equivalently (since all three direction components are nonzero):

$$
\frac{x+2}{6}=\frac{y}{5}=\frac{z-1}{2}.
$$

## A plane through three non-collinear points $P,Q,R$

* Build two independent directions inside the plane:

  $$
  \mathbf{u}=\overrightarrow{PQ}=Q-P,\qquad
  \mathbf{v}=\overrightarrow{PR}=R-P.
  $$
* Any point $X$ in that plane can be reached from $P$ by some combination of those two directions:

  $$
  \mathbf{x}=P+s\,\mathbf{u}+t\,\mathbf{v},\qquad s,t\in\mathbb{R}.
  $$

  That’s the **parametric equation** of the plane. (Requires $P,Q,R$ not collinear so that $\mathbf{u}$ and $\mathbf{v}$ aren’t parallel.)

**Example (from the text).**
$P=(1,0,2),\; Q=(-3,-2,4),\; R=(1,8,-5)$.
$\mathbf{u}=Q-P=(-4,-2,2),\quad \mathbf{v}=R-P=(0,8,-7)$.
Plane: $\displaystyle \mathbf{x}=(1,0,2)+s(-4,-2,2)+t(0,8,-7)$.

It’s often handy to also give the **scalar (implicit) form** using a normal vector.
Take $\mathbf{n}=\mathbf{u}\times\mathbf{v}=(1,14,16)$ (any nonzero multiple works).
Then

$$
\mathbf{n}\cdot(\mathbf{x}-P)=0
\;\;\Longleftrightarrow\;\;
x+14y+16z=33.
$$
  
## What is a vector/parametric equation of a line?  
  
* Pick an **anchor point** $P$ that lies on the line.  
* Pick a **direction vector** $\mathbf{d}$ that points along the line.  
* Then every point on the line is reached by “start at $P$, walk $t$ steps along $\mathbf{d}$”:  
  
  $$  
  \mathbf{r}(t)=P+t\,\mathbf{d},\qquad t\in\mathbb{R}.  
  $$  
  
  Here $t$ is the **parameter**. Changing $t$ slides you along the line.  
  
Why it matters:  
  
* Works perfectly in 3D (and higher), where “$y=mx+b$” doesn’t exist.  
* Makes intersection problems easy (solve two param equations together).  
* Lets you restrict to a **segment** by bounding $t$ (e.g., $t\in[0,1]$ gives the segment from $P$ to $Q$).  
  
## About the “eight properties”

The passage alludes to the standard vector space axioms (closure, associativity/commutativity of $+$, additive identity/inverses, distributivity and associativity for scalar multiplication, and the scalar identity). Any structure satisfying them is a **vector space**—$\mathbb{R}^2$, $\mathbb{R}^3$, and many others.

## Small gotchas

* For a line, $P\neq Q$.
* For a plane, $P,Q,R$ must be non-collinear (otherwise $\mathbf{u}$ and $\mathbf{v}$ are parallel and you don’t get a plane).

## What “parallel” / “collinear” means

* Two **vectors** $\mathbf{a}, \mathbf{b}\neq \mathbf{0}$ are **parallel** iff one is a scalar multiple of the other: $\mathbf{b}=k\mathbf{a}$ for some real $k$.
* Three **points** $O,P,Q$ (with $O$ the origin) are **collinear** iff the vectors $\overrightarrow{OP}$ and $\overrightarrow{OQ}$ are parallel.
  Equivalently, $Q$ lies on the line through $O$ and $P$, i.e. $Q=tP$ for some $t$.

![[Pasted image 20250910185603.png | 700]]

### Proof by “scalar multiple” test (quickest)

Example: $P=(3,1,2),\quad Q=(6,4,2)$

Assume $Q=kP$. Then compare coordinates:

$$
6=3k\Rightarrow k=2,\qquad 4=1k\Rightarrow k=4,\qquad 2=2k\Rightarrow k=1.
$$

You can’t have $k=2,4,$ and $1$ simultaneously. Contradiction.
So $Q$ is **not** a scalar multiple of $P$ ⇒ $\overrightarrow{OP}$ and $\overrightarrow{OQ}$ are **not parallel**, and $O,P,Q$ are **not collinear**.

### Proof by cross product (also standard in $\mathbb{R}^3$)

Compute $P\times Q$:

$$
P\times Q
=\begin{vmatrix}
\mathbf{i}&\mathbf{j}&\mathbf{k}\\[2pt]
3&1&2\\
6&4&2
\end{vmatrix}
=(-6,\,6,\,6)\neq \mathbf{0}.
$$

Nonzero cross product ⇒ not parallel.

### Using your $Q-P$ computation (nice intuition check)

You found $Q-P=(3,3,0)$.
If $O,P,Q$ were collinear, then $Q-P$ would be a scalar multiple of $P$ (indeed, $Q=tP\Rightarrow Q-P=(t-1)P$).
Test that:

$$
(3,3,0)=k(3,1,2)\ \Rightarrow\ 3k=3\Rightarrow k=1,\ \text{but then }1\cdot 1=1\neq 3.
$$



# Vector & Geometry Practice

## 1) Parallel vectors from the origin

Determine whether the vectors **from the origin** to each pair of points are parallel.

**(a)** $(3,\,1,\,2)$ and $(6,\,4,\,2)$

$$
\begin{gather}
\text{Assume } (3,\,1,\,2) \text{ and } (6,\,4,\,2) \text{ are parallel}. \\ \\
\text{Then, there is some k, such that } (3,\,1,\,2) = k(6,\,4,\,2). \\ \\
6=3k\Rightarrow k=2,\qquad 4=1k\Rightarrow k=4,\qquad 2=2k\Rightarrow k=1. \\ \\
\text{You can’t have $k=2,4,$ and $1$ simultaneously. Contradiction.}
\end{gather}
$$

**(b)** $(-3,\,1,\,7)$ and $(9,\,-3,\,-21)$

$$
\begin{gather}
\text{Assume } (-3,\,1,\,7) \text{ and } (9,\,-3,\,-21) \text{ are parallel}. \\ \\
\text{Then, there is some k, such that } (-3,\,1,\,7) = k(9,\,-3,\,-21). \\ \\
-\frac{9}{3} , -\frac{3}{1}, -\frac{21}{7} = \frac{-3}{1} \Rightarrow \text{parallel.}
\end{gather}
$$

> Since the same kkk works for all coordinates, the vectors are **parallel** (specifically, antiparallel—they point in opposite directions).

**(c)** $(5,\,-6,\,7)$ and $(-5,\,6,\,-7)$

$-\frac{5}{5}, -\frac{6}{6}, -\frac{7}{7} = -1 \Rightarrow \parallel$

**(d)** $(2,\,0,\,-5)$ and $(5,\,0,\,-2)$

$\frac{5}{2},0, \frac{2}{5}$ not parallel

*Answer idea:* Check if one vector is a scalar multiple of the other.

---

## 2) Lines through two points in space

Find a vector/parametric equation of the line through each pair of points.

**(a)** $(3,\,-2,\,4)$ and $(-5,\,7,\,1)$

Let $P=(3,-2,4)$ and $Q=(-5,7,1)$.  
Direction $\mathbf{d}=Q-P=(-8,\,9,\,-3)$.  
  
**Vector/parametric form**  
  
$$  
\boxed{\;\mathbf{r}(t)= (3,-2,4)+t\,(-8,\,9,\,-3),\quad t\in\mathbb{R}\;}  
$$  
  
**Component form**  
  
$$  
\boxed{\;x=3-8t,\quad y=-2+9t,\quad z=4-3t\;}  
$$  
  
**(Optional) Symmetric form** (since all direction components are nonzero)  
  
$$  
\boxed{\;\frac{x-3}{-8}=\frac{y+2}{9}=\frac{z-4}{-3}\;}  
$$  
  Visualization: ![[line_PQ.html]]
https://www.desmos.com/3d/xpz7oapwbj

**(b)** $(2,\,4,\,0)$ and $(-3,\,-6,\,0)$

$$
\begin{gather}
P = (2,\,4,\,0) \space,  Q = (-3,\,-6,\,0) \\
\text{Direction } d = Q - P = (-5,\, -10,\, 0) \\
r(t) = (2,\,4,\,0) + t (-5,\, -10,\, 0),\, t \in \mathbb{R}
\end{gather}
$$

**(c)** $(3,\,7,\,2)$ and $(3,\,7,\,-8)$

$$
\begin{gather}
P = (3,\,7,\,2) \text{ and } Q = (3,\,7,\,-8) \\
\text{Direction } d = Q - P = (0,\,0,\,-10) \\
r(t) = (3,\,7,\,2) + t(0,\,0,\,-10)
\end{gather}
$$

**(d)** $(-2,\,-1,\,5)$ and $(3,\,9,\,7)$

$$
\begin{gather}
P = (-2,\,-1,\,5) \text{ and } Q = (3,\,9,\,7) \\
\end{gather}
$$

*Answer idea:* Use $\mathbf{x} = P + t(Q-P)$.

---

## 3) Planes through three points in space

Find a parametric equation (and optionally a scalar/normal form) of the plane containing each triple of points.

**(a)** $(2,\,-5,\,-1),\ (0,\,4,\,6),\ (-3,\,7,\,1)$

$$
\begin{gather}
P = (2,-5,-1), \space Q = (0,4,6), \space R = (-3,7,1) \\ \\
u = Q - P = (0-2, 4- (-5), 6 - (-1)) = (-2, 9, 7) \\
v = (-3 - 2, 7 - (-5), 1 - (-1)) = (-5, 12, 2) \\
u = (-2, 9, 7) , v = (-5, 12, 2) \\
\\
\text{Parametric form}: \\
r(s,t) = P + su + tv = (2,-5,-1) + s(-2, 9, 7) + t(-5, 12, 2) \\ \\
\text{Normal vector and scalar form} \\

\end{gather}
$$
![[Screenshot 2025-09-11 at 11.23.07 AM.png]]
**(b)** $(3,\,-6,\,7),\ (-2,\,0,\,-4),\ (5,\,-9,\,-2)$
**(c)** $(-8,\,2,\,0),\ (1,\,3,\,0),\ (6,\,-5,\,0)$
**(d)** $(1,\,1,\,1),\ (5,\,5,\,5),\ (-6,\,4,\,2)$

*Answer idea:* Let $\mathbf{u}=Q-P$ and $\mathbf{v}=R-P$, then $\mathbf{x}=P + s\mathbf{u} + t\mathbf{v}$.
(Optional) Find a normal $\mathbf{n}=\mathbf{u}\times \mathbf{v}$ and write $\mathbf{n}\cdot(\mathbf{x}-P)=0$.

---

## 4) Concept & proof questions

**(a) Zero vector in the Euclidean plane**
What are the coordinates of the vector $\mathbf{0}$ in $\mathbb{R}^2$ that satisfies *condition 3 on page 3* (the additive identity property: $\mathbf{x} + \mathbf{0} = \mathbf{x}$)?

$$
\begin{gather}
\text{There exists a vector denoted 0 such that x + 0 = x for each vector x.} \\ \\
\text{For any } x = (a_{1}, a_{2}) \in \mathbb{R}^{2}, \text{ vector addition in coordinates is component-wise, so:} \\
x+0 = (a_{1},a_{2}) + (0,0) = (a_{1}+0, a_{2}+0) = (a_{1},a_{2}) = 0
\end{gather}
$$


**(b) Scaling endpoints**
Let $\mathbf{x}$ be the vector from the origin to $(a_1, a_2)$ in $\mathbb{R}^2$. Prove that for any real $t$, the vector $t\mathbf{x}$ (from the origin) terminates at $(ta_1, ta_2)$.

$$
\begin{gather}
x = (a_{1}, a_{2}) \\
xt = t(a_{1},a_{2})
\end{gather}
$$

**(c) Parallelogram diagonals**
Prove that the diagonals of a parallelogram bisect each other.


