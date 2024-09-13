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

#### Episode
- when the agent–environment interaction breaks naturally into subsequences of repeated interactions
- e.g. plays of a game, trips through a maze
- Each episode ends in a special state called the terminal state

#### Visit
- Each occurrence of state `s` in an episode is called a visit to `s`
- State `s` may be visited multiple times in the same episode also

#### First-Visit MC
- The first time a state `s` is visited in an episode is called the _first visit_ to `s`
- This method estimates the value of state `s` as the average of the returns following _first visits_ to s

#### Every-Visit MC
- This method estimates the value of state `s` as the average of the returns following _all visits_ to s

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

---
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


### Hand Calculation

#### Example

- Let's take an example of 2*2 Gridworld 

```
1 - 2
3 - 4
```

#### Initial Conditions
- Start State: (1,1) or cell 1 (top-left corner)
- Goal State: (2,3) or cell 4 (bottom-right corner)
- Actions: Up, Down, Left, Right
- Reward Structure: +1 for reaching the goal (cell 9), and 0 for all other cells.
- Initial Q-values: Assume all Q-values are initialized to 0.
- Learning Rate ($\alpha$): 0.5
- Discount Factor ($\gamma$): 0.9

Initial Q-table looks like this

| State | Action | $Q(s,a)$ |
| - | - |
| 1 | Down | 0 |
| 1 | Right | 0 |
| 2 | Down | 0 |
| 2 | Left | 0 |
| 3 | Up | 0 |
| 3 | Right | 0 |
| 4 | Up | 0 |
| 4 | Left | 0 |

Other (s,a) pairs are not valid as they will lead to crossing the grid boundary

#### Episode 1

Let's assume the agent follows the following path during episode 1

$$ State 1 \rightarrow Right \rightarrow State 2 \rightarrow Down \rightarrow State 4 $$

**Step 1**

- Agent moves right from state 1 to state 2
- No reward in the next state, so $R_{t+1} = 0$

$$ {Q}(1,Right) \leftarrow {Q}(1,Right) + \alpha \left (R_{t+1} + \gamma Q(2,Down) - {Q}(1,Right) \right ) $$

$$ {Q}(1,Right) \leftarrow 0 + 0.5(0 + 0.9*0 - 0) $$

$$ {Q}(1,Right) \leftarrow 0 $$

**Step 2**

- Agent moves down from state 2 to state 4
- Since 4 is goal state, so $R_{t+1} = 1$
- Since 4 is goal state, any action that takes agent away from 4 should have no value, so $Q(4,a') = 0$

$$ {Q}(2,Down) \leftarrow {Q}(2,Down) + \alpha \left (R_{t+1} + \gamma Q(4,a') - {Q}(2,Down) \right ) $$

$$ {Q}(2,Down) \leftarrow 0 + 0.5(1 + 0.9*0 - 0) $$

$$ {Q}(2,Down) \leftarrow 0.5 $$

**Backpropagation**

- Since agent has reached the goal state, it will now update its q-value estimates for all the steps it tool along the path traced

_State 2, Action Down -> State 4 (Goal)_

$$ {Q}(2,Down) \leftarrow 0.5 $$

_State 1, Action Right -> State 2_

$$ {Q}(1,Right) \leftarrow {Q}(1,Right) + \alpha \left (R_{t+1} + \gamma Q(2,Down) - {Q}(1,Right) \right ) $$

$$ {Q}(1,Right) \leftarrow 0 + 0.5(0 + 0.9*0.5 - 0) $$

$$ {Q}(1,Right) \leftarrow 0.225 $$


**Updated Q-table**

| State | Action | $Q(s,a)$ |
| - | - |
| 1 | Down | 0 |
| 1 | Right | 0.225 |
| 2 | Down | 0.5 |
| 2 | Left | 0 |
| 3 | Up | 0 |
| 3 | Right | 0 |
| 4 | Up | 0 |
| 4 | Left | 0 |

**Episode 1 ends**


#### Episode 2

Let's assume the agent follows the following path during episode 2

$$ State 1 \rightarrow Down \rightarrow State 3 \rightarrow Up \rightarrow State 1 \rightarrow Down \rightarrow State 3 \rightarrow Right \rightarrow State 4 $$

**Step 1**

- Agent moves right from state 1 to state 3
- Since 3 is not the goal state, so $R_{t+1} = 0$

$$ {Q}(1,Down) \leftarrow {Q}(1,Down) + \alpha \left (R_{t+1} + \gamma Q(3,Up) - {Q}(1,Down) \right ) $$

$$ {Q}(1,Down) \leftarrow 0 + 0.5(0 + 0.9*0 - 0) $$

$$ {Q}(1,Down) \leftarrow 0 $$

**Step 2**

- Agent moves down from state 3 to state 1
- Since 1 is not the goal state, so $R_{t+1} = 0$

$$ {Q}(3,Up) \leftarrow {Q}(3,Up) + \alpha \left (R_{t+1} + \gamma Q(1,Down) - {Q}(3,Up) \right ) $$

$$ {Q}(3,Up) \leftarrow 0 + 0.5(0 + 0.9*0 - 0) $$

$$ {Q}(3,Up) \leftarrow 0 $$

**Step 3**

- Agent moves down from state 1 to state 3
- Since 3 is not the goal state, so $R_{t+1} = 0$

$$ {Q}(1,Down) \leftarrow {Q}(1,Down) + \alpha \left (R_{t+1} + \gamma Q(3,Right) - {Q}(1,Down) \right ) $$

$$ {Q}(1,Down) \leftarrow 0 + 0.5(0 + 0.9*0 - 0) $$

$$ {Q}(1,Down) \leftarrow 0 $$

**Step 4**

- Agent moves down from state 3 to state 4
- Since 4 is goal state, so $R_{t+1} = 1$
- Since 4 is goal state, any action that takes agent away from 4 should have no value, so $Q(4,a') = 0$

$$ {Q}(3,Right) \leftarrow {Q}(3,Right) + \alpha \left (R_{t+1} + \gamma Q(4,a') - {Q}(3,Right) \right ) $$

$$ {Q}(3,Right) \leftarrow 0 + 0.5(1 + 0.9*0 - 0) $$

$$ {Q}(3,Right) \leftarrow 0.5 $$

**Backpropagation**

- Since agent has reached the goal state, it will now update its q-value estimates for all the steps it tool along the path traced

_State 3, Action Right -> State 4 (Goal)_

$$ {Q}(3,Right) \leftarrow 0.5 $$


_State 1, Action Down -> State 3_

$$ {Q}(1,Down) \leftarrow {Q}(1,Down) + \alpha \left (R_{t+1} + \gamma Q(3,Right) - {Q}(1,Down) \right ) $$

$$ {Q}(1,Down) \leftarrow 0 + 0.5(0 + 0.9*0.5 - 0) $$

$$ {Q}(1,Down) \leftarrow 0.225 $$


_State 3, Action Up -> State 1_

$$ {Q}(3,Up) \leftarrow {Q}(3,Up) + \alpha \left (R_{t+1} + \gamma Q(1,Down) - {Q}(3,Up) \right ) $$

$$ {Q}(3,Up) \leftarrow 0 + 0.5(0 + 0.9*0.225 - 0) $$

$$ {Q}(3,Up) \leftarrow 0.10125 $$


_State 1, Action Down -> State 3_

$$ {Q}(1,Down) \leftarrow {Q}(1,Down) + \alpha \left (R_{t+1} + \gamma Q(3,Up) - {Q}(1,Down) \right ) $$

$$ {Q}(1,Down) \leftarrow 0.225 + 0.5(0 + 0.9*0.10125 - 0.225) $$

$$ {Q}(1,Down) \leftarrow 0.158 $$

**Updated Q-table**

| State | Action | $Q(s,a)$ |
| - | - |
| 1 | Down | 0.158 |
| 1 | Right | 0.225 |
| 2 | Down | 0.5 |
| 2 | Left | 0 |
| 3 | Up | 0.10125 |
| 3 | Right | 0.5 |
| 4 | Up | 0 |
| 4 | Left | 0 |

**Episode 2 ends**

