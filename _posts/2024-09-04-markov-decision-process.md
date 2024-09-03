---
layout: post
title: "Markov Decision Process - Concepts"
date: 2024-09-04
tags: ["Machine Learning","Reinforcement Learning","Probability Theory"]
---

The Markov Property is a fundamental concept in probability theory and reinforcement learning

---

## Concepts

### 1. State

**N-arm Bandit Context**
- In the N-armed bandit problem, the notion of "state" is static i.e. there is essentially only one state.
- This is because the bandit problem doesn't involve transitions between different states as a function of actions taken
- The environment remains the same after each action; hence, the "state" doesn't change.
- The focus is purely on selecting actions (choosing an arm to pull) and updating action-value estimates based on the rewards received, without any transitions between different states.

**Traditional RL Context**
- In traditional reinforcement learning problems, a "state" usually refers to the specific situation or configuration in which an agent finds itself in an environment. 
- The state influences the actions available and the rewards received.

**Definition**
- In the most general, causal case this response may depend on everything that has happened earlier

$$ P(R_{t+1}=r, S_{t+1}=s' | S_0,A_0,R_1,S_1,A_1,...,R_t,S_t,A_t) $$

---
### 2. Markov Property

- It states that the future state of a process depends only on the current state, not on the sequence of events that preceded it. In other words, the process is "memoryless"
- If the state signal has the Markov property, then the environment’s response at t + 1 depends only on the state and action representations at t
- This means that we can predict all future states and expected rewards from knowledge only of the current state

**Definition**

$$ P(R_{t+1}=r, S_{t+1}=s' | S_t, A_t) $$

---
### 3. Finite Markov Decision Process
- A reinforcement learning task that satisfies the Markov property is called a _Markov decision process (MDP)_
- If the state and action spaces are finite,then it is called a _finite Markov decision process (finite MDP)_
- 90% of modern reinforcement learning problems involve Finite MDP assumptions

**Definition**
- A particular finite MDP is defined by its state and action sets and by the one-step dynamics of the environment.

$$ p(s',r|s,a) = P(S_{t+1}=s', R_{t+1}=r | S_t=s, A_t=a) $$

---
