---
layout: post
title: "Monte Carlo Method"
date: 2024-09-05
tags: ["Reinforcement Learning"]
---

The Monte Carlo method estimates the value of a state based on the average return received from that state in multiple episodes

---

## Derivation

- Goal: Estimate the state-value function $v_{\pi}(S_t)$ for a given policy $\pi$.

- Actual Returns: $G_t$ is the return (sum of discounted rewards) from time t onwards

- Hypothesis Function: Let $\hat{v}_{\theta}(S_t)$ denote the estimated value function 

- Objective Function: Need to minimize the MSE between actual return and e

$$ J(\theta) = \mathbb{E}\left (G_t - \hat{v}_{\theta}(S_t) \right )^2 $$

- Gradient:

$$ \frac{\partial J}{\partial \theta} = \frac{\partial}{\partial \theta} \mathbb{E}\left (G_t - \hat{v}_{\theta}(S_t) \right )^2 $$

$$ \Rightarrow \frac{\partial J}{\partial \theta} = \mathbb{E}[\frac{\partial}{\partial \theta}  \left (G_t - \hat{v}_{\theta}(S_t) \right )^2]  $$

$$ \Rightarrow \frac{\partial J}{\partial \theta} = -2 \mathbb{E}[\left (G_t - \hat{v}_{\theta}(S_t) \right )\frac{\partial}{\partial \theta}  \hat{v}_{\theta}(S_t)] $$

- Stochastic Gradient Descent

$$ \theta(S_t) \leftarrow \theta(S_t) - \alpha \frac{\partial J}{\partial \theta}$$

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \mathbb{E}[\left (G_t - \hat{v}_{\theta}(S_t) \right )\frac{\partial}{\partial \theta}  \hat{v}_{\theta}(S_t)]$$

- Assumption 1: 

Let's assume that in the simplest case, we have one parameter per state, so $\theta$ is essentially a lookup table.

$$ \hat{v}_{\theta}(S_t) = \theta (S_t)$$

$$ \frac{\partial}{\partial \theta}  \hat{v}_{\theta}(S_t) = 1 $$

Substituting,

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \mathbb{E}[\left (G_t - \hat{v}_{\theta}(S_t) \right )] $$

- Assumption 2: 

Since, we don't know the true expectation, so we use a sample 

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \left (G_t - \theta(S_t) \right ) $$

$$ {v}(S_t) \leftarrow {v}(S_t) + \alpha \left (G_t - {v}(S_t) \right ) $$

- This is the monte carlo update rule

