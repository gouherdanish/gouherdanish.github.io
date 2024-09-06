---
layout: post
title: "Temporal Difference Learning"
date: 2024-09-05
tags: ["Reinforcement Learning"]
---

TD Learning is a central and novel idea in reinforcement learning

---
## Intuition

- TD Learning is a combination of the Monte Carlo method and Dynamic Programming. 
    - Like Monte Carlo methods, TD methods can learn directly from raw experience without a model of the environment’s dynamics.
    - Like DP, TD methods update estimates based in part on other learned estimates, without waiting for a final outcome
- It updates the value of the current state based on the estimated value of the next state. That's why they are called _bootstrapping methods_ like DP

---

## Derivation

- Goal: Estimate the state-value function $v_{\pi}(S_t)$ for a given policy $\pi$.

- Actual value: $v_{\pi}(S_t)$ is the value of the state $S_t$

- Hypothesis Function: Let $\hat{v}_{\theta}(S_t)$ denote the estimated value function 

- Objective Function: Need to minimize the MSE between actual value and estimated value

$$ J(\theta) = \mathbb{E}\left (v_{\pi}(S_t) - \hat{v}_{\theta}(S_t) \right )^2 $$

- Gradient:

$$ \frac{\partial J}{\partial \theta} = \frac{\partial}{\partial \theta} \mathbb{E}\left (v_{\pi}(S_t) - \hat{v}_{\theta}(S_t) \right )^2 $$

$$ \Rightarrow \frac{\partial J}{\partial \theta} = \mathbb{E}[\frac{\partial}{\partial \theta}  \left (v_{\pi}(S_t) - \hat{v}_{\theta}(S_t) \right )^2]  $$

$$ \Rightarrow \frac{\partial J}{\partial \theta} = -2 \mathbb{E}[\left (v_{\pi}(S_t) - \hat{v}_{\theta}(S_t) \right )\frac{\partial}{\partial \theta}  \hat{v}_{\theta}(S_t)] $$

- Stochastic Gradient Descent

$$ \theta(S_t) \leftarrow \theta(S_t) - \alpha \frac{\partial J}{\partial \theta}$$

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \mathbb{E}[\left (v_{\pi}(S_t) - \hat{v}_{\theta}(S_t) \right )\frac{\partial}{\partial \theta}  \hat{v}_{\theta}(S_t)]$$

- Assumption 1: 

Let's assume that in the simplest case, we have one parameter per state, so $\theta$ is essentially a lookup table.

$$ \hat{v}_{\theta}(S_t) = \theta (S_t)$$

$$ \frac{\partial}{\partial \theta}  \hat{v}_{\theta}(S_t) = 1 $$

Substituting,

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \mathbb{E}[\left (v_{\pi}(S_t) - \hat{v}_{\theta}(S_t) \right )] $$

- Assumption 2: 

Since, We don't know $v_{\pi}(S_t)$, but we can use the Bellman equation as a target

$$ v_{\pi}(S_t) = \mathbb{E}[R_{t+1} + \gamma v_{\pi}(S_{t+1})] $$

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \left (R_{t+1} + \gamma v_{\pi}(S_{t+1}) - \theta(S_t) \right ) $$

- Assumption 3:

Since we still don't know $v_{\pi}(S_{t+1})$, so we approximate it with our current estimate $v(S_{t+1})$

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \left (R_{t+1} + \gamma v(S_{t+1}) - \theta(S_t) \right ) $$

$$ {v}(S_t) \leftarrow {v}(S_t) + \alpha \left (R_{t+1} + \gamma v(S_{t+1}) - {v}(S_t) \right ) $$

- This is update rule for TD(0) method
-  Monte Carlo methods must wait until the end of the episode to determine the increment to $v(S_t)$ (only then is $G_t$ known)

