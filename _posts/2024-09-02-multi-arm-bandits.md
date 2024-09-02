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

---
## Hand Calculation

Let's continue with the 3-arm bandits example

### Fully Greedy Policy

- The agent selects same arm at each time step
- No exploration, only exploitation 
- $\epsilon=0$

| Time | Constant Policy |
| $t$  | $S_1$ | $S_2$ | $S_3$ |
| ---- | ----- | ----- | ----- |
|  0   |   2   |   3   |  3.0  |
|  1   |   2   |   1   |  2.7  |
|  2   |   2   |   2   |  3.2  |
|  3   |   2   |   6   |  3.1  |
|  4   |   2   |   1   |  3.9  |
|  5   |   2   |   3   |  4.2  |
|  6   |   2   |   6   |  2.0  |
|  7   |   2   |   2   |  1.0  |
|  8   |   2   |   1   |  3.0  |
|  9   |   2   |   3   |  3.0  |

From above table, we can calculate the total reward achievable 

- Constant Policy: $S_1$

$$ R_{1,total} = 2*10 = 20 $$

- Constant Policy: $S_2$

$$ R_{1,total} = (3+1+2+6+1+3+6+2+1+3) = 28 $$

- Constant Policy: $S_3$

$$ R_{1,total} = (3.0+2.7+3.2+3.1+3.9+4.2+2.0+1.0+3.0+3.0) = 30.1 $$




### Random Policy (Fully Greedy, $\epsilon$=1)

Suppose, there are 10 time steps and at each of the steps, our RL agent selects a random slot





---
## Formulation




