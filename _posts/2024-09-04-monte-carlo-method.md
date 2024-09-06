---
layout: post
title: "Monte Carlo Method"
date: 2024-09-04
tags: ["Reinforcement Learning"]
---

Monte Carlo methods are ways of solving the reinforcement learning problem based on averaging sample returns.

---
### Intuition

- The term “Monte Carlo” is often used more broadly for any estimation method whose operation involves a significant random component.
- In the context of RL, Monte Carlo methods are based on averaging complete returns
- Monte Carlo method estimates the value of a state based on the average return received from that state in multiple episodes

---
### Why Average

- Monte Carlo methods are used for learning the state-value function for a given policy
- We know that value of a state is the expected return i.e. expected cumulative future discounted reward, starting from that state
- During multiple episodes, the agent visits a particular state multiple times and observes actual returns from that state
- If we keep track of average returns observed after visits to that state, it gradually converges to the expected value
- That's why average is used

---

### Comparison with Multi-Arm Bandits

- Multi-arm bandits methods deal with just 1 state while Monte Carlo methods deal with multiple state
    - each state can be thought of as acting like a different bandit problem (like an associative-search or contextual bandit) and that the different bandit problems are interrelated
- Bandit methods sample and average _rewards_ for each action while Monte Carlo methods sample and average _returns_ for each state–action 
- Bandit problem is stationary while Monte Carlo problem is non-stationary because each state and action-selection is undergoing learning

---
### Concepts

#### Experience
- In Monte Carlo methods, "experience" refers to the observed sequences of states, actions, and rewards that an agent encounters while interacting with an environment
- We assume experience is divided into episodes, and that all episodes eventually terminate no matter what actions are selected

---

### Properties

- Episodic Nature:
    - Monte Carlo methods are typically used for episodic tasks, where the agent's interaction with the environment has a clear beginning and end.
    - Monte Carlo methods can thus be incremental in an episode-by-episode sense, but not in a step-by-step (online) sense. 
- Complete Returns:
    - In Monte Carlo methods, we wait until the end of an episode to update our value estimates. 
    - This means we use the complete, actual return from each state, rather than estimated returns.
- Sampling:
    - Each episode provides a sample of the agent's experience in the environment. 
    - Multiple episodes give us multiple samples, allowing us to estimate expected values.
- State-Action-Reward Sequences:
    - An episode consists of a sequence of (state, action, reward) tuples, typically denoted as (S₀, A₀, R₁, S₁, A₁, R₂, ..., Sₜ).
- No Bootstrapping:
    - Unlike temporal difference methods, Monte Carlo doesn't use estimates of other states' values to update the current state's value. 
    - It relies solely on actual experienced returns.

### Derivation

**Goal**

- Estimate the state-value function $v_{\pi}(S_t)$ for a given policy $\pi$.

**Actual Returns** 

- $G_t$ is the return (sum of discounted rewards) from time t onwards

**Hypothesis Function**

- Let $\hat{v}_{\theta}(S_t)$ denote the estimated value function 

**Objective Function**

- Need to minimize the MSE between actual return and estimated value
- The objective function can be written as follows

$$ J(\theta) = \mathbb{E}\left (G_t - \hat{v}_{\theta}(S_t) \right )^2 $$

**Gradient**

- Taking first order partial derivative 

$$ \frac{\partial J}{\partial \theta} = \frac{\partial}{\partial \theta} \mathbb{E}\left (G_t - \hat{v}_{\theta}(S_t) \right )^2 $$

$$ \Rightarrow \frac{\partial J}{\partial \theta} = \mathbb{E}[\frac{\partial}{\partial \theta}  \left (G_t - \hat{v}_{\theta}(S_t) \right )^2]  $$

$$ \Rightarrow \frac{\partial J}{\partial \theta} = -2 \mathbb{E}[\left (G_t - \hat{v}_{\theta}(S_t) \right )\frac{\partial}{\partial \theta}  \hat{v}_{\theta}(S_t)] $$

**Stochastic Gradient Descent**

- Parameter update is governed by

$$ \theta(S_t) \leftarrow \theta(S_t) - \alpha \frac{\partial J}{\partial \theta}$$

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \mathbb{E}[\left (G_t - \hat{v}_{\theta}(S_t) \right )\frac{\partial}{\partial \theta}  \hat{v}_{\theta}(S_t)]$$

**Assumptions**

_Assumption 1_

Let's assume that in the simplest case, we have one parameter per state, so $\theta$ is essentially a lookup table.

$$ \hat{v}_{\theta}(S_t) = \theta (S_t)$$

$$ \frac{\partial}{\partial \theta}  \hat{v}_{\theta}(S_t) = 1 $$

Substituting,

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \mathbb{E}[\left (G_t - \hat{v}_{\theta}(S_t) \right )] $$

_Assumption 2_

Since, we don't know the true expectation, so we use a sample 

$$ \theta(S_t) \leftarrow \theta(S_t) + 2 \alpha \left (G_t - \theta(S_t) \right ) $$

$$ {v}(S_t) \leftarrow {v}(S_t) + \alpha \left (G_t - {v}(S_t) \right ) $$

- This is update rule for a _simple every-visit Monte Carlo method_ suitable for nonstationary environments 
-  Monte Carlo methods must wait until the end of the episode to determine the increment to $v(S_t)$ (only then is $G_t$ known)

