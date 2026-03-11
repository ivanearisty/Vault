## Problem 2: IoU and Generalized IoU (10 pts)

### (a) Non-differentiability Analysis (5 pts)

Let box $A = (x_1^A,\, y_1^A,\, x_2^A,\, y_2^A)$ and box $B = (x_1^B,\, y_1^B,\, x_2^B,\, y_2^B)$ where $(x_1, y_1)$ is the top-left corner and $(x_2, y_2)$ is the bottom-right corner.

#### Explicit IoU formula

**Intersection coordinates:**

$$
x_1^I = \max(x_1^A,\, x_1^B), \quad y_1^I = \max(y_1^A,\, y_1^B)
$$

$$
x_2^I = \min(x_2^A,\, x_2^B), \quad y_2^I = \min(y_2^A,\, y_2^B)
$$

**Intersection dimensions** (clamped to non-negative):

$$
w_I = \max(0,\; x_2^I - x_1^I), \qquad h_I = \max(0,\; y_2^I - y_1^I)
$$

**Areas:**

$$
|A \cap B| = w_I \cdot h_I
$$

$$
|A| = (x_2^A - x_1^A)(y_2^A - y_1^A), \qquad |B| = (x_2^B - x_1^B)(y_2^B - y_1^B)
$$

$$
|A \cup B| = |A| + |B| - |A \cap B|
$$

$$
\boxed{\text{IoU}(A, B) = \frac{|A \cap B|}{|A \cup B|} = \frac{\max(0,\, x_2^I - x_1^I) \cdot \max(0,\, y_2^I - y_1^I)}{|A| + |B| - |A \cap B|}}
$$

#### Non-differentiable operations

Two types of operations in this formula break differentiability:

1. **$\max$ and $\min$** used to compute $x_1^I, y_1^I, x_2^I, y_2^I$. The function $\max(a, b)$ has a kink at $a = b$ — the left and right derivatives disagree, so the gradient is undefined at that point.

2. **$\max(0, \cdot)$** used to clamp $w_I$ and $h_I$. This is the ReLU function, which has a non-differentiable kink at 0.

#### Geometric configurations where gradient is undefined

- **Aligned edges:** When $x_1^A = x_1^B$, or $y_1^A = y_1^B$, or $x_2^A = x_2^B$, or $y_2^A = y_2^B$. At these points the $\max$/$\min$ operations switch which argument they "pass through," creating a kink in the IoU surface.

- **Boxes just touching:** When $x_2^I = x_1^I$ or $y_2^I = y_1^I$ (one intersection dimension is exactly zero). The $\max(0, \cdot)$ clamp sits at its kink.

- **Non-overlapping boxes:** When $w_I = 0$ and $h_I = 0$, the intersection area is zero and $\text{IoU} = 0$ regardless of how far apart the boxes are. The gradient is zero everywhere in this regime — **there is no learning signal to push the boxes closer together**. This is IoU's most critical limitation for training.

---

### (b) Generalized IoU (GIoU) (5 pts)

$$
\text{GIoU} = \text{IoU} - \frac{|C \setminus (A \cup B)|}{|C|}
$$

where $C$ is the smallest axis-aligned box enclosing both $A$ and $B$:

$$
x_1^C = \min(x_1^A, x_1^B),\quad y_1^C = \min(y_1^A, y_1^B),\quad x_2^C = \max(x_2^A, x_2^B),\quad y_2^C = \max(y_2^A, y_2^B)
$$

$$
|C| = (x_2^C - x_1^C)(y_2^C - y_1^C)
$$

The penalty term $|C \setminus (A \cup B)| / |C|$ measures the fraction of the enclosing box that is empty space (not covered by either $A$ or $B$).

#### Why GIoU provides useful gradients when boxes don't overlap

When $\text{IoU} = 0$ (no overlap), the IoU gradient is zero — moving the predicted box slightly closer to the ground truth doesn't change IoU at all, so the optimizer has no signal.

GIoU fixes this through the penalty term. Even when $\text{IoU} = 0$, moving the predicted box toward the target **shrinks the enclosing box** $C$. A smaller $|C|$ means the penalty fraction $|C \setminus (A \cup B)| / |C|$ changes, producing a non-zero gradient. The optimizer can follow this gradient to bring the boxes together until they overlap, at which point the IoU term also becomes active.

In short: **IoU only "sees" overlap; GIoU also "sees" proximity.**

#### Range of GIoU values

$$
\boxed{\text{GIoU} \in [-1,\; 1]}
$$

- **GIoU $= 1$:** Perfect overlap ($A = B$). IoU $= 1$ and penalty $= 0$.
- **GIoU $= -1$:** Limit as boxes are infinitely far apart. IoU $\to 0$ and the enclosing box $C$ becomes almost entirely empty space, so penalty $\to 1$.

#### What negative values signify

A negative GIoU means the penalty term exceeds the IoU — the enclosing box contains more empty space than overlap. This happens when the boxes are far apart or have very little overlap relative to the gap between them. **The more negative the GIoU, the worse the localization** — the predicted box is far from the target. This gives a smooth, interpretable measure of "how bad" a non-overlapping prediction is, unlike IoU which just says 0 for all of them.
