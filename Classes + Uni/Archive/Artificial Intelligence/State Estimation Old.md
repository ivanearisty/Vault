The professor says:
Nobody is going to ask anything about the equations so dont spend time on them.

You need to understand the essence of the kalam filter. 

you must understand how the filter operates.

spend time understanding 



Three types of states:

- Atomic, frequent in planning problems, devide a plot to square of some size and you dont have to decompose state it's basically just a number
- Factored states consist of other random variables and we make it into a long vector, everything in the perception module is basically a random state
- Structured (relational) states are when we're interested in creating relationships (im on top of a table)

We're focusing on factored states.

# Recursive State Estimation

notations
1. Action is $\alpha_{t}$ Agent first takes an action and then senses the environment.
2. $\alpha_{t_{2}}:a_{t_{2}}$ all the actions from time 1 to time 2
3. measurment of $Z_{t}$ could be predictions from perception. For example, $\hat{y}$ could be raw sensory information.
4. $z_{t_{1}}:z_{t_{2}}$ same as before

## State Transition Model

Generative Model for the state evolution.

$P(S_{t} | S_{0: t_{-1}}, \alpha_{1:t}, Z_{1:t-1})$

Probability of reaching state St given the history of previous states, history of actions, and history of measurements.

A state S_t is complete if it is a complete summary of the past.
St is a sufficient statistic for all past measurements and actions. (Markovian Assumption)

This simplifies things to:
$P(S_{t} | S_{t_{-1}}, \alpha_{t})$

This says that the action at time t induces the state transition from st-1 to st

## Generative Measurement Model

$P(Z_{t}|S_{t})$

Probabilistic Graphical Models: 
Hidden Merkov Model
![[Screenshot 2025-10-17 at 5.59.52 AM.png]]

1. $\hat{bel}(S_{t})$ = belief of being in state St before receiving measurment Zt
2. $bel(S_{t})$ = belief of being in state St after receiving measurment Zt

bayer-filter(bel(st-1), alphat, zt)

involves 2 steps: 
![[Screenshot 2025-10-17 at 6.07.25 AM.png]]

Why Bayes?
![[Screenshot 2025-10-17 at 6.10.02 AM.png]]

## The Drone Localization Problem - Kalman

![[Screenshot 2025-10-17 at 6.21.53 AM.png]]

$Z_{t} = x_{t}+ \epsilon_{z}$ 

Where $\epsilon ~ N(0,\sigma_{z}^{2})$  epsilon is normally distributed.

Assumptions:
1. Drone is hovering, no wind
2. Every $\Delta t$ we get a measurment.

![[Screenshot 2025-10-17 at 6.24.25 AM.png]]

We want to quickly make the drone converge to the actual location.

What is the probability distribution of Zt? Well, it's a gaussian with xt mean and sigma z squared.

$Z_{t}~ N(X_{t}, \sigma_{z}^{2})$

![[Screenshot 2025-10-17 at 6.41.18 AM.png]]
![[Screenshot 2025-10-17 at 6.42.20 AM.png]]
![[Screenshot 2025-10-17 at 6.44.58 AM.png]]
![[Screenshot 2025-10-17 at 6.45.48 AM.png]]

### Kalman Equation

$$
\begin{gather}
\text{Kalman}\big( \text{bel}(s_{t-1}), a_t, z_t \big) \equiv 
\text{Kalman}\big( \mu_{t-1}, \Sigma_{t-1}, a_t, z_t \big)
\end{gather}
$$

Prediction
![[Screenshot 2025-10-17 at 6.58.22 AM.png]]

Measurment
![[Screenshot 2025-10-17 at 6.59.34 AM.png]]


Steps:

Recall the diagram we used for the g-h filter:

We’ve been doing the same thing in this chapter. The Kalman filter makes a prediction, takes a measurement, and then forms a new estimate somewhere between the two.

**This is extremely important to understand**: Every filter in this book implements the same algorithm, just with different mathematical details. The math can become challenging in later chapters, but the idea is easy to understand.

It is important to see past the details of the equations of a specific filter and understand _what_ the equations are calculating and _why_. There are a tremendous number of filters. They all use different math to implement the same algorithm. The choice of math affects the quality of results and what problems can be represented, but not the underlying ideas.

Here is the generic algorithm:

**Initialization**

```
1. Initialize the state of the filter
2. Initialize our belief in the state
```

**Predict**

```
1. Use system behavior to predict state at the next time step
2. Adjust belief to account for the uncertainty in prediction
```

**Update**

```
1. Get a measurement and associated belief about its accuracy
2. Compute residual between estimated state and measurement
3. Compute scaling factor based on whether the measurement
or prediction is more accurate
4. set state between the prediction and measurement based 
on scaling factor
5. update belief in the state based on how certain we are 
in the measurement
```

You will be hard pressed to find a Bayesian filter algorithm that does not fit into this form. Some filters will not include some aspects, such as error in the prediction, and others will have very complicated methods of computation, but this is what they all do.

The equations for the univariate Kalman filter are:

![[Screenshot 2025-10-17 at 7.03.39 AM.png]]


![[Screenshot 2025-10-17 at 7.05.13 AM.png]]![[Screenshot 2025-10-17 at 7.05.28 AM.png]]

posterior gaussian
![[Screenshot 2025-10-17 at 7.06.20 AM.png]]

