## Problem 1: Convolution Filter Design and Analysis (20 pts)

### (a) Directional Edge Detection (6 pts)

**Filter 1 — Horizontal Edge Detector** (detects dark-to-light or light-to-dark transitions in the vertical direction):

$$
W_H = \begin{pmatrix} -1 & -1 & -1 \\ 0 & 0 & 0 \\ 1 & 1 & 1 \end{pmatrix}
$$

**How it works:** The top row sums pixels above the center and the bottom row sums pixels below. When there's a horizontal boundary (intensity changes going up/down), the difference between the bottom sum and top sum is large, producing a strong response. The middle row contributes nothing, so the filter compares "one row up" against "one row down." In flat regions both sums match and the output is zero.

**Filter 2 — Vertical Edge Detector** (detects transitions in the horizontal direction):

$$
W_V = \begin{pmatrix} -1 & 0 & 1 \\ -1 & 0 & 1 \\ -1 & 0 & 1 \end{pmatrix}
$$

**How it works:** Same idea rotated 90 degrees. The left column sums pixels to the left of center, the right column sums pixels to the right. A strong horizontal intensity change (vertical edge) produces a large difference. The center column contributes nothing.

#### Testing on the 5x5 image (valid padding $\Rightarrow$ 3x3 output)

$$
X = \begin{pmatrix} 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 0 & 0 \\ 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 & 1 \end{pmatrix}
$$

**$W_H \star X$ (horizontal edge filter):**

At position $(0,0)$, the $3\times 3$ patch covers rows 0–2, cols 0–2:

$$
(-1)(0)+(-1)(0)+(-1)(0) + (0)(0)+(0)(0)+(0)(0) + (1)(1)+(1)(1)+(1)(1) = 3
$$

Positions $(0,1)$ and $(0,2)$ use identical row structure $\Rightarrow$ same result. At row 1, the patch covers rows 1–3:

$$
(-1)(0){+}(-1)(0){+}(-1)(0) + (0)(1){+}(0)(1){+}(0)(1) + (1)(1){+}(1)(1){+}(1)(1) = 3
$$

At row 2, the patch covers rows 2–4 (all ones):

$$
(-1)(1){+}(-1)(1){+}(-1)(1) + 0 + (1)(1){+}(1)(1){+}(1)(1) = -3 + 3 = 0
$$

$$
\boxed{W_H \star X = \begin{pmatrix} 3 & 3 & 3 \\ 3 & 3 & 3 \\ 0 & 0 & 0 \end{pmatrix}}
$$

The top two rows show a strong response exactly where the horizontal edge is (the 0 $\to$ 1 transition between rows 2 and 3). The bottom row is zero because it falls inside the uniform region.

**$W_V \star X$ (vertical edge filter):**

Every row of $X$ is constant across columns (all 0s or all 1s), so for any patch the left-column sum always equals the right-column sum, giving zero everywhere.

$$
\boxed{W_V \star X = \begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix}}
$$

This confirms correct behavior: the image contains a horizontal edge but no vertical edges, and the filters respond accordingly.

---

### (b) Comparing Blur Filters (4 pts)

$$
W_A = \frac{1}{9}\begin{pmatrix} 1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{pmatrix} \qquad W_B = \frac{1}{16}\begin{pmatrix} 1 & 2 & 1 \\ 2 & 4 & 2 \\ 1 & 2 & 1 \end{pmatrix}
$$

**Why both blur:** Both filters replace each pixel with a weighted average of its $3\times 3$ neighborhood. Averaging neighboring pixels suppresses rapid intensity changes (high-frequency content), which is what makes the image look smoother. Any pixel that is brighter or darker than its surroundings gets pulled toward the local mean.

**Which better preserves edges:** $W_B$ (weighted blur). Two reasons:

1. **Center weighting:** $W_B$ gives the center pixel $4/16 = 25\%$ of the total weight, vs. $1/9 \approx 11\%$ for $W_A$. Near an edge, the center pixel's original value is preserved more.
2. **Distance falloff:** $W_B$'s weights decrease with distance from center (corners get $1/16$, direct neighbors get $2/16$, center gets $4/16$). This Gaussian-like profile means far-away pixels across an edge boundary contribute less, so the edge stays sharper. $W_A$ weights all 9 pixels equally, smearing the edge uniformly.

---

### (c) Sharpening Filter Design (6 pts)

The unsharp masking formula is:

$$
\text{Sharpened} = \text{Original} + \alpha\,(\text{Original} - \text{Blurred})
$$

In filter form, the identity kernel acts as "original" and $W_A$ acts as "blurred":

$$
\delta = \begin{pmatrix} 0 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 0 \end{pmatrix}
$$

So the sharpening kernel is:

$$
S = \delta + \alpha(\delta - W_A) = (1+\alpha)\,\delta - \alpha\, W_A
$$

#### Derivation for $\alpha = 1$

$$
\begin{gather}
S = 2\delta - W_A = \begin{pmatrix} 0 & 0 & 0 \\ 0 & 2 & 0 \\ 0 & 0 & 0 \end{pmatrix} - \frac{1}{9}\begin{pmatrix} 1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{pmatrix}
\end{gather}
$$

$$
\boxed{S_{\alpha=1} = \frac{1}{9}\begin{pmatrix} -1 & -1 & -1 \\ -1 & 17 & -1 \\ -1 & -1 & -1 \end{pmatrix}}
$$

The center weight $(17/9 \approx 1.89)$ amplifies the original pixel, while the negative surround $(-1/9)$ subtracts the local average. The net effect is: anywhere the pixel differs from its neighbors (i.e. at edges), that difference gets boosted.

#### Large $\alpha$ analysis (e.g. $\alpha = 5$)

$$
S_{\alpha=5} = 6\delta - 5W_A = \frac{1}{9}\begin{pmatrix} -5 & -5 & -5 \\ -5 & 49 & -5 \\ -5 & -5 & -5 \end{pmatrix}
$$

**What happens to the weights:** The general form is center weight $= (9 + 8\alpha)/9$ and each surround weight $= -\alpha/9$. As $\alpha$ grows:

- The center weight grows linearly $\to$ the pixel's own value is amplified more
- The surrounding weights become more negative $\to$ neighbors are subtracted more aggressively

**Impact on the image:**

- **Edge overshoot:** Edges develop bright/dark "halos" (ringing artifacts) because the filter overshoots the transition
- **Noise amplification:** Since noise is high-frequency, the filter boosts it alongside real edges
- **Value clipping:** Output values can exceed the valid pixel range $[0, 255]$, requiring clamping

---

### (d) Noise Reduction Analysis (4 pts)

Given an image with **additive Gaussian noise** (zero-mean, affects every pixel independently):

**Prefer a linear filter (box/Gaussian blur) when:**

- The noise is Gaussian. Averaging $N$ pixels reduces Gaussian noise variance by a factor of $\sim 1/N$, since independent zero-mean noise samples cancel out. This is statistically optimal for Gaussian noise.
- Speed matters — linear filters can be computed via separable kernels or FFT.
- Mild, uniform smoothing is acceptable.

**Prefer a median filter when:**

- The noise is **salt-and-pepper** (impulse) type, where isolated pixels take extreme values (0 or 255). A median filter completely ignores these outliers by picking the middle value, removing them cleanly. A linear filter would average the extreme values into the result, creating gray smudges instead of removing them.
- **Edge preservation** is critical. The median picks an actual pixel value from the neighborhood rather than creating a weighted average, so sharp edges survive intact.

For the specific scenario in this problem (additive Gaussian noise), **the linear filter is more appropriate** because averaging is the statistically principled way to reduce Gaussian noise — each pixel's noise is independent and zero-mean, so the average converges to the true signal. The median filter still reduces Gaussian noise, but is less efficient at it (higher residual variance for the same kernel size).
