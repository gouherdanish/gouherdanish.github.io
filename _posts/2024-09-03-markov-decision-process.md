---
layout: post
title: "Markov Decision Process - Concepts"
date: 2024-09-03
tags: ["Reinforcement Learning"]
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
- In the most general, causal case, the state of the environment at time step $t+1$ may depend on everything that has happened earlier till the time step $t$

$$ P(R_{t+1}=r, S_{t+1}=s' | S_0,A_0,R_1,S_1,A_1,...,R_t,S_t,A_t) $$

---
### 2. Markov Property

- It states that the future state of a process depends only on the current state, not on the sequence of events that preceded it. 

> The future is independent of the past given the present

- This means that we can predict all future states and expected rewards from knowledge only of the current state
- The state captures all relevant information from the history
- Once the state is known, the history may be thrown away
- The state is a sufficient statistic of the future

**Definition**

- If the state signal has the Markov property, then the environment’s response at t + 1 depends only on the state and action representations at t

$$ P(R_{t+1}=r, S_{t+1}=s' | S_t, A_t) $$

---
### 3. Finite Markov Decision Process
- A reinforcement learning task that satisfies the Markov property is called a _Markov decision process (MDP)_
- Since the process only needs to know the current state, we can call this process "memoryless"
- If the state and action spaces are finite,then it is called a _finite Markov decision process (finite MDP)_
- 90% of modern reinforcement learning problems involve Finite MDP assumptions

**Definition**
- A particular finite MDP is defined by its state and action sets and by the one-step dynamics of the environment.

$$ p(s',r|s,a) = P(S_{t+1}=s', R_{t+1}=r | S_t=s, A_t=a) $$

---
### 4. Returns ($G_t$)

**Definition**

- The return is the total cumulative reward from time step t to the end of the episode

$$ G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + ... $$

where $\gamma$ is the _discount factor_

**Discount Factor**

- The discount factor determines the present value of future rewards
- Case 1: $\gamma = 0$ 
    - the agent is “myopic” i.e. it is concerned only with maximizing immediate rewards
    - its objective in this case is to learn how to choose action $A_t$ so as to maximize only $R_{t+1}$
    - but in general, acting to maximize immediate reward can reduce access to future rewards so that the return may actually be reduced.
- Case 2: $\gamma \rightarrow 1$ 
    -  the agent becomes far-sighted i.e. the objective takes future rewards into account more strongly

**Incremental Update Formula**

$$ G_t = R_{t+1} + \gamma (R_{t+2} + \gamma R_{t+3} + ...) $$

$$ G_t = R_{t+1} + \gamma G_{t+1} $$

---

### 5. State Value Function

**Definition**
- Intuitively, it means how good it is for the agent to be in a given state 

**Bellman Equation**
- The value function represents the expected return from state `s` under a given policy $\pi$

$$ v_{\pi}(S_t) = \mathbb{E}[G_t | S_t = s] $$

$$ v_{\pi}(S_t) = \mathbb{E}[R_{t+1} + \gamma G_{t+1} | S_t = s] $$

$$ v_{\pi}(S_t) = \mathbb{E}[R_{t+1} + \gamma \mathbb{E}[G_{t+1}]] $$

$$ v_{\pi}(S_t) = \mathbb{E}[R_{t+1} + \gamma v_{\pi}(S_{t+1})] $$

$$ v = R + \gamma Pv $$

> Value of current state = Immediate Reward + Discounted value of next state

### 6. Action Value Function

**Definition**
- Intuitively, it means how good it is for the agent to take a certain action in a given state 

**Bellman Equation**
- The action value function represents the expected return from taking action `a` in a given state `s` and thereafter following a given policy $\pi$

$$ q_{\pi}(S_t) = \mathbb{E}[G_t | S_t = s, A_t = a] $$

$$ q_{\pi}(S_t) = \mathbb{E}[R_{t+1} + \gamma G_{t+1} | S_t = s, A_t = a] $$

$$ q_{\pi}(S_t) = \mathbb{E}[R_{t+1} + \gamma \mathbb{E}[G_{t+1}]] $$

$$ q_{\pi}(S_t) = \mathbb{E}[R_{t+1} + \gamma q_{\pi}(S_{t+1})] $$

$$ q = R + \gamma Pq $$

> Action value of current state = Immediate Reward + Discounted action value of next state

