---
layout: post
title: "Multi-arm Bandits: Exploration vs Exploitation"
date: 2024-09-02
tags: ["Machine Learning","Reinforcement Learning"]
---

The N-armed bandit problem is a classic reinforcement learning problem that is often used to illustrate the exploration-exploitation trade-off.

---

## Problem Setup

- Imagine there are 2 slot machines in a Casino and each machine has a different but fixed reward distribution

$$ Slots = \{S_1,S_2,S_3\} $$

- Reward distribution for each slot is as follows

$$ R_1 = [5] \; constant \; reward $$

$$ R_2 = [1,5,3,7] \; random \; reward $$

$$ R_3 = \mathbb{N}(3,1) \; normal \; distribution $$

- Our task is to figure out, over multiple plays, which machine has the highest mean reward

---
## Action Selection

- It is a process by which an agent selects action while choosing from available actions
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
    - Greedy or $\epsilon-greedy$ are types of policy that agent follows at each step
    - Here, $\epsilon$ represents the probability that the agent will go for exploration at a time step
    - e.g. $\epsilon = 0.1$ means the agent will explore a random new slot machine with 10% probabity

---




