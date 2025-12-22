
### 🎯 The Core Idea: What's the Point?

The main goal of a state estimation filter is to figure out the **true state of a system** (like a drone's exact location) by combining two key pieces of information:
1.  **A prediction:** Where we *think* the system is, based on a model of its movement.
2.  **A measurement:** A noisy reading from a sensor (like GPS).

The filter provides the **optimal way to fuse these two sources of information** to get a new estimate that is more accurate than either one alone. Every filter, including the Kalman filter, follows this same fundamental algorithm.

---

### 🔑 Key Concepts & Terminology

* **State ($S_t$):** A complete summary of the system at a specific time, $t$. We are focusing on **factored states**, which represent the state as a vector of random variables.
* **Action ($a_t$):** An action the agent takes that influences the state (e.g., firing a drone's thrusters).
* **Measurement ($Z_t$):** Raw sensory data that is related to the current state but contains noise. For example, a GPS reading is a measurement of a drone's true position, but with some error ($\epsilon$).
* **Belief ($bel(S_t)$):** Our confidence or probability distribution of the system being in state $S_t$. The filter's job is to update this belief over time.
* **Markov Assumption:** This is a crucial simplification. It assumes the current state $S_t$ is a **complete summary of the past**. This means to predict the next state, we only need the *current* state ($S_{t-1}$) and the *current* action ($a_t$), not the entire history.

---

### 🔄 How The Filter Operates: The Two-Step Cycle

The filter operates in a continuous loop. The core logic is to **predict, then update**.

#### **Step 1: Predict**

In this step, we use our system model to predict the state for the next time step *before* we get a new measurement.
1.  **Predict the New State:** Based on the last known state and the most recent action, we predict where the system will be. (e.g., "The drone was at X, moved forward, so it should now be at Y").
2.  **Increase Uncertainty:** Since our prediction model isn't perfect, our belief becomes more uncertain. The probability distribution of our state "spreads out.".

#### **Step 2: Update (Measurement Update)**

In this step, we incorporate a new measurement to correct our prediction and refine our estimate.
1.  **Get a Measurement:** We receive new sensory data ($Z_t$).
2.  **Calculate the Residual:** We find the difference between our **predicted state** and our new **measurement**. This difference is called the "residual" or "innovation."
3.  **Calculate the Kalman Gain (K):** This is the magic ingredient. The gain is a value between 0 and 1 that decides how much we trust the prediction vs. the measurement.
    * If **measurement uncertainty is high**, the gain is low, and we stick closer to our prediction.
    * If **prediction uncertainty is high**, the gain is high, and we trust the new measurement more.
4.  **Compute the New Estimate:** The final estimate is a weighted average, positioned somewhere between the prediction and the measurement, based on the Kalman Gain.
    > **New Estimate** = **Prediction** + **Gain** \* (**Measurement** - **Prediction**)
5.  **Decrease Uncertainty:** After fusing the measurement, our belief becomes more certain. The probability distribution of our state "shrinks.".

---

### ✨ The Foundation: Bayes' Rule & Gaussians

* **It's all Bayes' Rule:** The "Update" step is a practical application of Bayes' rule.
    > $posterior = \frac{likelihood \times prior}{evidence}$ or simply $bel(x_t) = \eta \cdot p(z_t|x_t) \cdot \overline{bel}(x_t)$
    * **Prior ($\overline{bel}(x_t)$):** Our belief *before* the measurement (from the Predict step).
    * **Likelihood ($p(z_t|x_t)$):** The probability of seeing our measurement, given our predicted state.
    * **Posterior ($bel(x_t)$):** Our new, updated belief *after* the measurement.
* **Gaussian Assumption:** The Kalman filter works so well because it assumes everything is a Gaussian distribution (a bell curve). If your initial belief (prior) is Gaussian and your measurement error is Gaussian, the updated belief (posterior) will also be a perfect Gaussian. This makes the math simple and efficient.


![[Screenshot 2025-10-17 at 7.03.39 AM.png]]