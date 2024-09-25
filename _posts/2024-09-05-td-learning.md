---
layout: post
title: "Temporal Difference Learning"
date: 2024-09-05
tags: ["Reinforcement Learning"]
---

TD Learning is a central and novel idea in reinforcement learning

---
### Intuition

- TD Learning is a combination of the Monte Carlo method and Dynamic Programming. 
    - Like Monte Carlo methods, TD methods can learn directly from raw experience without a model of the environment’s dynamics.
    - Like DP, TD methods update estimates based in part on other learned estimates, without waiting for a final outcome
- It updates the value of the current state based on the estimated value of the next state. 
- This means they learn a guess from a guess that's why they are called _bootstrapping methods_ like DP

### Advantages

#### TD vs DP

- TD methods have an advantage over DP methods in that they do not require a model of the environment, of its reward and next-state probability distributions

#### TD vs Monte Carlo

- With Monte Carlo methods one must wait until the end of an episode, because only then is the return known. 
- Some applications have very long episodes, so that delaying all learning until an episode’s end is too slow.
- Other applications are continuing tasks and have no episodes at all.
- TD methods have usually been found to converge faster than constant-$\alpha$ MC methods

---

### Derivation

**Goal**: 

- Estimate the state-value function $v_{\pi}(S_t)$ for a given policy $\pi$.

**Actual value:** 

- $v_{\pi}(S_t)$ is the value of the state $S_t$

**Hypothesis Function:**

- Let $\hat{v}_{\theta}(S_t)$ denote the estimated value function 

**Objective Function:** 

- Need to minimize the MSE between actual value and estimated value
- The objective function can be written as follows

$$ J(\theta) = \mathbb{E}\left (v_{\pi}(S_t) - \hat{v}_{\theta}(S_t) \right )^2 $$

**Gradient:**

$$ \frac{\partial J}{\partial \theta} = \frac{\partial}{\partial \theta} \mathbb{E}\left (v_{\pi}(S_t) - \hat{v}_{\theta}(S_t) \right )^2 $$

$$ \Rightarrow \frac{\partial J}{\partial \theta} = \mathbb{E}[\frac{\partial}{\partial \theta}  \left (v_{\pi}(S_t) - \hat{v}_{\theta}(S_t) \right )^2]  $$

$$ \Rightarrow \frac{\partial J}{\partial \theta} = -2 \mathbb{E}[\left (v_{\pi}(S_t) - \hat{v}_{\theta}(S_t) \right )\frac{\partial}{\partial \theta}  \hat{v}_{\theta}(S_t)] $$

**Stochastic Gradient Descent**

- Parameter update rule is governed by

$$ \theta(S_t) \leftarrow \theta(S_t) - \alpha \frac{\partial J}{\partial \theta}$$

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \mathbb{E}[\left (v_{\pi}(S_t) - \hat{v}_{\theta}(S_t) \right )\frac{\partial}{\partial \theta}  \hat{v}_{\theta}(S_t)]$$

**Assumptions**

_Assumption 1_

Let's assume that in the simplest case, we have one parameter per state, so $\theta$ is essentially a lookup table.

$$ \hat{v}_{\theta}(S_t) = \theta (S_t)$$

$$ \frac{\partial}{\partial \theta}  \hat{v}_{\theta}(S_t) = 1 $$

Substituting,

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \mathbb{E}[\left (v_{\pi}(S_t) - \hat{v}_{\theta}(S_t) \right )] $$

_Assumption 2_

Since, We don't know $v_{\pi}(S_t)$, but we can use the Bellman equation as a target

$$ v_{\pi}(S_t) = \mathbb{E}[R_{t+1} + \gamma v_{\pi}(S_{t+1})] $$

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \left (R_{t+1} + \gamma v_{\pi}(S_{t+1}) - \theta(S_t) \right ) $$

_Assumption 3_

Since we still don't know $v_{\pi}(S_{t+1})$, so we approximate it with our current estimate $v(S_{t+1})$

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \left (R_{t+1} + \gamma v(S_{t+1}) - \theta(S_t) \right ) $$

$$ {v}(S_t) \leftarrow {v}(S_t) + \alpha \left (R_{t+1} + \gamma v(S_{t+1}) - {v}(S_t) \right ) $$

**TD(0) Update**

_Update Rule for State Value Function_
- As derived above, here is the update rule for state-value function for TD(0) method
- It updates the value of the current state $v_t$ based on the estimated value of the next state $v_{t+1}$

$$ {v}(S_t) \leftarrow {v}(S_t) + \alpha \left (R_{t+1} + \gamma v(S_{t+1}) - {v}(S_t) \right ) $$

_Update Rule for Action Value Function - SARSA_
- On similar lines, we can derive the update rule for action-value function for TD(0) method
- It updates the value of the current state-action $q_t$ based on the estimated value of the next state $q_{t+1}$

$$ {q}(S_t,A_t) \leftarrow {q}(S_t,A_t) + \alpha \left (R_{t+1} + \gamma q(S_{t+1},A_{t+1}) - {q}(S_t,A_t) \right ) $$

_Update Rule for Action Value Function - Q-Learning_
- Q-Learning uses Bellman optimality conditions
- It updates the value of the current state-action $q_t$ based on the maximum estimated q-value of the next state$

$$ {q}(S_t,A_t) \leftarrow {q}(S_t,A_t) + \alpha \left (R_{t+1} + \gamma max_{a'}Q(S_{t+1},a') - {q}(S_t,A_t) \right ) $$

### Conclusion
- We presented a deep dive into Temporal Difference Learning technique
- We used Gradient Descent optimization to derive the TD update rule
- Also, finally we presented the update rules for some key TD algorithms e.g. SARSA, Q-Learning
