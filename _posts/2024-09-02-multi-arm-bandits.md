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

$$ R_1 = [5] \; constant reward $$

$$ R_2 = [1,5,3,7] \; random reward $$

$$ R_3 = \mathbb{N}(3,1) \; reward according to normal distribution$$

- Our task is to figure out, over multiple plays, which machine has the highest mean reward

