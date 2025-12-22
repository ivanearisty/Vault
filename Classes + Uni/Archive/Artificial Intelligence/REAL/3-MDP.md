### **Topic 3: MDP Estimation (The Prediction Problem)**

**The Goal:** Given a fixed policy $\pi$ (e.g., "always move right" or "move randomly"), calculate the value $V^{\pi}(s)$ for every state. You are _not_ trying to find the best strategy yet; you are just grading the current one.

#### **1. The Formula: Bellman Expectation Equation (Linear)**

You must memorize this equation for calculation.

$$V^{\pi}(s) = \sum_{a} \pi(a|s) \sum_{s'} P(s'|s,a) [ R(s,a,s') + \gamma V^{\pi}(s') ]$$

Simplified for Hand Calculation:

Usually, the problem gives you deterministic transitions or simple probabilities.

$$V(s) = \text{Immediate Reward} + \gamma \times (\text{Weighted Average of Neighbor Values})$$

#### **2. Numerical Example (Likely Exam Question)**

**Scenario:** A 2-state world ($A$ and $B$).

- **Policy:** From $A$, always go to $B$. From $B$, always stay at $B$.
    
- **Rewards:** Moving $A \to B$ gives $+10$. Staying $B \to B$ gives $+1$.
    
- **Discount ($\gamma$):** $0.9$.
    

**Calculate $V(A)$ and $V(B)$:**

1. **Write equations for each state:**
    
    - $V(A) = R(A \to B) + \gamma V(B)$
        
    - $V(B) = R(B \to B) + \gamma V(B)$
        
2. **Plug in numbers:**
    
    - $V(A) = 10 + 0.9 V(B)$
        
    - $V(B) = 1 + 0.9 V(B)$
        
3. **Solve the system (Algebra):**
    
    - Solve for $V(B)$ first:
        
        $$V(B) - 0.9V(B) = 1$$
        
        $$0.1V(B) = 1 \implies V(B) = 10$$
        
    - Plug $V(B)$ into $V(A)$:
        
        $$V(A) = 10 + 0.9(10) = 19$$
        

**Answer:** $V(A) = 19, V(B) = 10$.

---

### **Topic 4: MDP Optimal Solution (The Control Problem)**

**The Goal:** Find the **Optimal Policy $\pi^*$** and **Optimal Value $V^*$**. This requires solving the **Bellman Optimality Equation**, which includes a `max` operator.

#### **1. The Algorithm: Value Iteration / Policy Iteration**

Since you cannot solve the `max` operator with linear algebra, you must iterate. The exam will likely ask you to perform **1 or 2 iterations** by hand.

The "Greedy" Update Step:

$$V_{k+1}(s) = \max_{a} \sum_{s'} P(s'|s,a) [ R(s,a,s') + \gamma V_k(s') ]$$

#### **2. Step-by-Step Procedure for Exam (Grid World)**

**Scenario:**

- **Grid:** 1D Grid: [Left Cell] -- [Right Cell] (Target)
    
- **Goal:** Reach Right Cell (Reward +100, Terminal). Left Cell has Reward 0.
    
- **Actions:** Left, Right.
    
- **Discount $\gamma$:** $0.9$.
    
- **Transition:** Deterministic.
    

**Iteration 0 (Initialization):**

- $V_0(\text{Left}) = 0$
    
- $V_0(\text{Right}) = 0$
    

Iteration 1 (Update Left Cell):

Calculate value for every action using $V_0$:

- **Action Right:** Reward(0) + $0.9 \times V_0(\text{Right})$ = $0 + 0 = 0$ (Note: If moving _into_ terminal gives reward, add it here). Let's say entering Right gives +100.
    
    - **Revised Action Right:** Reward(+100) + $0.9 \times 0$ = 100.
        
- **Action Left:** Reward(0) + $0.9 \times V_0(\text{Left})$ = $0 + 0 = 0$.
    
- **Take Max:** $V_1(\text{Left}) = \max(100, 0) = 100$.
    

**Iteration 2 (Update Left Cell using $V_1$):**

- **Action Right:** Reward(+100) + $0.9 \times V_1(\text{Right})$. (If Right is terminal, its value usually stays 0 or fixed).
    
- **Action Left:** Reward(0) + $0.9 \times V_1(\text{Left})$.
    
    - $0 + 0.9 \times 100 = 90$.
        
- **Take Max:** $\max(100, 90) = 100$. (Converged).
    

---

### **Crucial Exam Tricks (Watch Out)**

1. **Terminal States:**
    
    - The value of a terminal state is **always 0** (or the final fixed reward). It does _not_ have a future value because the episode ends.
        
    - Mathematically: $V(\text{Terminal}) = 0$.
        
2. **Discount Factor ($\gamma$):**
    
    - If $\gamma = 0$: The agent is "Myopic" (cares only about immediate reward).
        
    - If $\gamma \to 1$: The agent is "Far-sighted".
        
    - **Calculation Tip:** If $\gamma = 0$, $V(s)$ is just the max immediate reward.
        
3. **Stochastic Transitions (The "Slip" Factor):**
    
    - If the problem says "80% chance move Forward, 10% Left, 10% Right", your calculation for **one action** must include **three terms**.
        
    - $Q(s, \text{Forward}) = 0.8[\text{Reward} + \gamma V(\text{Front})] + 0.1[\text{Reward} + \gamma V(\text{Left})] + 0.1[\text{Reward} + \gamma V(\text{Right})]$.
        

---

### **Final Checklist: Can you do this?**

1. **System of Equations:** Can you write down $V(A) = \dots$ and $V(B) = \dots$ and solve for $V(A)$? (Topic 3)
    
2. **One-Step Update:** Given $V_k$ values, can you calculate $V_{k+1}$ by checking all actions and picking the max? (Topic 4)
    
3. **Policy Extraction:** Once you have the values, can you draw the arrow? (The arrow points to the neighbor with the highest $V$ value).

