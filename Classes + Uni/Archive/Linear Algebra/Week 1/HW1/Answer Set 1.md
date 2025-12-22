### Problem 1
$$
\begin{gather}
\text{Let } V = {f:R \to (0,\infty)} \text{ with: } \\
(f+g)(x) = f(x)g(x) \text{ and } (cf)(x) = f(x)^{c} \\ \\
\text{Additive identity:} \\
\text{We need a function } z(x) \in V \text{ such that for any function } f(x) \in V: \\
(f+z)(x) = f(x) \\ \\
\text{We have:} \\
(f+z)(x) = f(x)z(x) \\ \\
\text{Since } f(x) \text{ is a positive function, we can devide by it, giving us:} \\
z(x) = 1 \\
\text{which is a continous function that maps } \mathbb{R} \text{ to } (0, \infty), \text{so it is in } V. \\ \\
\text{Therefore the additive identiy is a constant function: } z(x) = 1. \\ \\
\text{Additive Inverse:} \\
\text{The additive inverse of a function } f(x) \in V, \text{ denoted as } -f, \\
\text{is a function such that: } (f+(-f))(x) = z(x), \text{where } z(x) \text{ is the additive identity.} \\ \\
\text{Using the definitions of addition and the additive idenity, we have: } \\(f+(−f))(x)=f(x)(−f)(x)=1 \\ \\
\end{gather}
$$

### Problem 2

$$
\begin{gather}
\text{Let S be the set of polynomials of degree exactly }  n \text{ i.e. } \\
S = \{ a_{n}t^{n} + \dots + a_{0} \in \mathcal{P}_{n}(F) : a_{n} \neq 0\} \\ 
\text{Is } S \text{ a subspace of } \mathcal{P}_{n}(F) \\ \\
(i) 0 ∈ S \\ \\
\text{Since we cannot have } a_{n} = 0 \text{ the 0 vector is not included.} \\
\text{Hence, it's not on a subspace}
\\ \\
\end{gather}
$$
### Problem 3


![[Screenshot 2025-10-02 at 3.34.04 PM.png]]

#### 1
$$
\begin{gather}
\text{The element } \mathbf{0} \in V \text{ is unique.}\\
\text{Assume, for the sake of contradiction, that } \mathbf{0} \text{ is not unique.} \\
\text{Then there must be a vector } \mathbf{0}' \in V \text{ such that: } \\
\forall x \neq 0 \in V : x + \mathbf{0}' = x. \\
\text{However we have that } x + \mathbf{0} = x. \\
\text{Then } x + \mathbf{0}' = x + \mathbf{0} \\ 
\text{Removing x we see that: } \mathbf{0}' = \mathbf{0} \\
\text{So } \mathbf{0} \text{ must be unique.}
\end{gather}
$$

#### 2
$$
\begin{gather}
\text{For any } x ∈ V , 0x = \mathbf{0} \\
(a+b)x = ax + bx \\
(0+0)x = ax + bx \\
0x = 0x + 0x \\
\mathbf{0} = 0x 
\end{gather}
$$
#### 3
$$
\begin{gather}
\text{For any } a \in F, a \mathbf{0} = \mathbf{0} \\
a(x+y) = ax+ax \\
a(\mathbf{0} + \mathbf{0}) = a\mathbf{0}+a\mathbf{0} \\
a\mathbf{0} = a\mathbf{0}+a\mathbf{0} \\
\mathbf{0} = a\mathbf{0}
\end{gather}
$$
#### 4
![[Screenshot 2025-10-04 at 11.32.14 PM.png]]
$$
\begin{gather}
\text{If } a = 0 \\
\text{Then } a\mathbf{0} = \mathbf{0} \text{ which we proved above} \\
\text{If } a \neq 0 \\
a^{-1}(ax) = a^{-1}\mathbf{0}\\
(a^{-1}a)x = a^{-1}\mathbf{0}\\
1x = a^{-1}\mathbf{0}\\
1x = \mathbf{0} \\
x = \mathbf{0}
\end{gather}
$$
#### 5 and 6
![[Screenshot 2025-12-20 at 2.01.14 PM.png]]


evaluate $$x + (-1)x$$
$$x + (-1)x = 1x + (-1)x$$
$$1x + (-1)x = (1 + (-1))x$$
$$(1 + (-1))x = 0x$$
$$0x = 0$$
We have now shown that:

$$x + (-1)x = 0$$
Since $x + (-1)x = 0$, the term $(-1)x$ must be that unique additive inverse.

## P4

### 1
Let $z_1 = a + bi$.
Let $z_2 = \frac{a}{a^2+b^2} + \left(\frac{-b}{a^2+b^2}\right)i$.

Real part: $a\left(\frac{a}{a^2+b^2}\right) - b\left(\frac{-b}{a^2+b^2}\right) = \frac{a^2}{a^2+b^2} + \frac{b^2}{a^2+b^2} = \frac{a^2+b^2}{a^2+b^2} = 1$.
Imaginary part $a\left(\frac{-b}{a^2+b^2}\right) + b\left(\frac{a}{a^2+b^2}\right) = \frac{-ab}{a^2+b^2} + \frac{ab}{a^2+b^2} = 0$.
Combining:
$$z_1 z_2 = 1 + 0i = 1$$
### 2

#### $3i + (5+2i)$
$$0 + 3i + 5 + 2i = (0+5) + (3+2)i = \mathbf{5 + 5i}$$
#### $(4+i) - 3$
$$(4-3) + (1-0)i = \mathbf{1 + i}$$
#### $(2+i)(1-i)$
$$((2)(1) - (1)(-1)) + ((2)(-1) + (1)(1))i$$
$$= (2 - (-1)) + (-2 + 1)i$$
$$= (2 + 1) - 1i$$
$$= \mathbf{3 - i}$$
#### $\frac{1-i}{1+i}$

From part (i):
$$\frac{1-i}{1+i} \cdot \frac{1-i}{1-i} = \frac{(1-i)(1-i)}{(1+i)(1-i)}$$

den: $1^2 + 1^2 = 2$
numerator: $(1-i)(1-i) = 1(1) + 1(-i) - i(1) - i(-i) = 1 - i - i + i^2 = 1 - 2i - 1 = -2i$
So
$$\frac{-2i}{2} = \mathbf{-i}$$
### 3
We prove closure by:
Addition: The sum $(a+bi) + (c+di) = (a+c) + (b+d)i$ is in $\mathbb{C}$ because the sum of real numbers ($a+c$ and $b+d$) is real.
Scalar Multiplication: The product $\alpha(a+bi) = \alpha a + \alpha bi$ for any scalar $\alpha \in \mathbb{R}$ is in $\mathbb{C}$ because the product of real numbers is real.

Zero Vector: The element $0 + 0i$ acts as the identity because $(a+bi) + (0+0i) = a+bi$.
Additive Inverse: For every $z = a+bi$, there exists $-z = (-a) + (-b)i$ such that $z + (-z) = 0 + 0i$.

The remaining axioms hold true for $\mathbb{C}$ because they hold true for $\mathbb{R}$. Since complex addition and scalar multiplication are performed component-wise on real numbers, they are  inherited from $\mathbb{R}$

### 4

Let $c_1, c_2 \in \mathbb{R}$ be scalars.
We can form a linear combination and set it to the zero vector of the space:
$$c_1(1) + c_2(i) = 0$$
Then:

$$c_1 + c_2i = 0$$

We are told that "a complex number $a + bi$ is $0$ if and only if $a=b=0$"
Our real part $a$ is $c_1$ and our imaginary part $b$ is $c_2$.
So c1 = 0 and c2 = 0 and it's linearly independent.