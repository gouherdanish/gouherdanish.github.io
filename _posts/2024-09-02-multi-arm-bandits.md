---
layout: post
title: "Multi-arm Bandits: Exploration vs Exploitation"
date: 2024-09-02
tags: ["Machine Learning","Reinforcement Learning"]
---

The N-armed bandit problem is a classic reinforcement learning problem that is often used to illustrate the exploration-exploitation trade-off.

---

## Problem Setup

- Imagine there are 3 slot machines in a Casino and each machine has a different but fixed reward distribution

$$ Slots = \{S_1,S_2,S_3\} $$

- Reward distribution for each slot is as follows

$$ R_1 = [2] \; constant \; reward $$

$$ R_2 = [1,5,3,7] \; random \; reward $$

$$ R_3 = \mathbb{N}(3,1) \; normal \; distribution $$

- Our task is to figure out, over multiple plays, which machine has the highest mean reward

---
## Action Selection

- It is a process by which the agent selects an action while choosing from available actions
- Example - In slot machine example, at any time step, the agent has an option to either 
    - _Exploit_
        - select the slot machine that has the highest estimated reward based on the information collected so far
        - This action is called _greedy action_
        - Exploitation is the right thing to do to maximize the expected reward on the one step
    - _Explore_
        - select a new slot machine to expand its knowledge about the environment so that it can maximize future rewards
        - This action is called _exploration_
        - Exploration may produce greater total reward in the long run
- During exploration, reward is lower in the short run, but higher in the long run because after you have discovered the better actions, you can exploit them many times.
- Choosing between _Exploration_ and _Exploitation_ can be based on 
    - precise values of the estimates, 
    - uncertainties, and 
    - the number of remaining steps
- Action selection can be based on a policy or strategy 
    - Greedy or $\epsilon$-greedy are types of policy that agent follows at each step
    - e.g. $\epsilon = 0.1$ means the agent will explore a random new slot machine with 10% probabity

## Action-value Estimates

- Q-value for an action `a` at a given time step `t` can be estimated as follows

$$ Q_{t}(a) = \frac{R_1 + R_2 + ... + R_k}{k} $$

- Let $Q_k$ denote the estimate for the kth reward i.e. the average of first $k-1$ awards

$$ Q_{k} = \frac{1}{k-1}\sum_{i=1}^{k-1} R_i $$

Similarly,

$$ Q_{k+1} = \frac{1}{k}\sum_{i=1}^k R_i $$

$$ \Rightarrow Q_{k+1} = \frac{1}{k} \left (R_k + \sum_{i=1}^{k-1} R_i \right)$$

$$ \Rightarrow Q_{k+1} = \frac{1}{k} \left (R_k + (k-1)Q_k \right)$$

$$ \Rightarrow Q_{k+1} = Q_k + \frac{1}{k} \left (R_k - Q_k \right)$$

- This is the incremental update policy for Q-value

- _Exploitation_ chooses the action with the maximum q-value at a given time step is chosen

---
## Hand Calculation

Let's continue with the 3-arm bandits example

### Greedy Policy

- The agent selects same arm at each time step
- No exploration, only exploitation 
- $\epsilon=0$

| $t$  | Action | Reward | Q | Action | Reward | Q | Action | Reward | Q |
| ---- | ----- | ----- | --- | ----- | ----- | ----- | ----- | --- | --- |
|  0   | $S_1$ |   2   |  2  | $S_2$ |   3   |  0  | $S_3$ |  3.0  | 0   |
|  1   | $S_1$ |   2   |  2  | $S_2$ |   1   |  3  | $S_3$ |  2.7  | 3   |
|  2   | $S_1$ |   2   |  2  | $S_2$ |   2   |  2  | $S_3$ |  3.2  | 2.85 |
|  3   | $S_1$ |   2   |  2  | $S_2$ |   6   |  2  | $S_3$ |  3.1  | 2.96 |
|  4   | $S_1$ |   2   |  2  | $S_2$ |   1   |  3  | $S_3$ |  3.9  | 3 |
|  5   | $S_1$ |   2   |  2  | $S_2$ |   3   | 2.6   | $S_3$ |  4.2  | 3.18 |
|  6   | $S_1$ |   2   |  2  | $S_2$ |   6   | 2.33  | $S_3$ |  2.0  | 3.35 |
|  7   | $S_1$ |   2   |  2  | $S_2$ |   2   | 3.14  | $S_3$ |  1.0  | 3.15 |
|  8   | $S_1$ |   2   |  2  | $S_2$ |   1   |  3   | $S_3$ |  3.0  | 2.88 |
|  9   | $S_1$ |   2   |  2  | $S_2$ |   3   | 2.7  | $S_3$ |  3.0  | 2.9 |
| ---- | ----- | ----- |  20  | ----- | ----- | 28 | ----- | --- | 30.1 |


From above table, we can calculate the total reward achievable 

- Constant Policy: $S_1$

$$ R_{1,total} = 2*10 = 20 $$

- Constant Policy: $S_2$

$$ R_{1,total} = (3+1+2+6+1+3+6+2+1+3) = 28 $$

- Constant Policy: $S_3$

$$ R_{1,total} = (3.0+2.7+3.2+3.1+3.9+4.2+2.0+1.0+3.0+3.0) = 30.1 $$

---
### Non-Greedy Policy

- The agent selects random arm at each time step
- Full exploration, no exploitation 
- $\epsilon=1$

| $t$  | Action | Reward |
| ---- | ----- | ------  |
|  0   | $S_1$ |    2    |
|  1   | $S_3$ |   4.1   |
|  2   | $S_1$ |    2    |
|  3   | $S_2$ |    5    |
|  4   | $S_2$ |    1    |
|  5   | $S_3$ |   2.6   |
|  6   | $S_1$ |    2    |
|  7   | $S_2$ |    3    |
|  8   | $S_1$ |    2    |
|  9   | $S_3$ |   3.0   |


From above table, we can calculate the total reward achieved

$$ R_{total} = 2+4.1+2+5+1+2.6+2+3+2+3.0 = 26.7 $$


### Epsilon-Greedy Policy

- Most of the times, the agent selects arm from its knowledge so far that it estimates will give immediate maximum reward
    - represented by $1-\epsilon$
- Sometimes, the agent explores and selects a new arm
    - represented by $\epsilon$
- Assume $\epsilon=0.5$

| $t$  | Type | Action | Reward | $k_{S1}$ | $k_{S2}$ | $k_{S3}$ | $Q_{S1}$ | $Q_{S2}$ | $Q_{S3}$ |
| ---- | ----- | ----- | ------  | -------- | -------- | -------- | -------- | -------- | -------- |
|  0   | Exploit | $S_1$ |    2    |     0    |     0    |     0    |     0    |     0    |     0    |
|  1   | Exploit | $S_1$ |    2    |     1    |     0    |     0    |     2    |     0    |     0    |
|  2   | Exploit | $S_1$ |    2    |     2    |     0    |     0    |     2    |     0    |     0    |
|  3   | Explore | $S_2$ |    5    |     3    |     0    |     0    |     2    |     0    |     0    |
|  4   | Exploit | $S_2$ |    1    |     3    |     1    |     0    |     2    |     5    |     0    | 
|  5   | Explore | $S_3$ |   2.6   |     3    |     2    |     0    |     2    |     3    |     0    |
|  6   | Explore | $S_1$ |    2    |     3    |     2    |     1    |     2    |     3    |   2.6    |
|  7   | Exploit | $S_2$ |    6    |     4    |     2    |     1    |     2    |     3    |   2.6    |
|  8   | Explore | $S_1$ |    2    |     4    |     3    |     1    |     2    |     4    |   2.6    |
|  9   | Explore | $S_3$ |   3.0   |     5    |     3    |     1    |     2    |     4    |   2.6    |
| 10   |   -   |   -   |    -    |     5    |     3    |     2    |     2    |     4    |   2.8    |

From above table, we can calculate the total reward achieved

$$ R_{total} = 2+2+2+5+1+2.6+2+6+2+3 = 27.6 $$

---
## Formulation




