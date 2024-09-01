---
layout: post
title: "Reinforcement Learning - Concepts"
date: 2024-09-01
tags: ["Machine Learning"]
---

Reinforcement Learning is a powerful framework for developing intelligent agents capable of learning and making decisions through trial and error

---

## Intuition

- Reinforcement Learning (RL) is a branch of machine learning where a agent/bot interacts with its environment, learns from its mistakes and successes and ultimately aims to maximize its rewards.
- By interacting with the environment and learning from the consequences of actions, RL agents can solve complex problems in various domains, from games to robotics and autonomous driving

---
## Elements of RL

### 1. Agent

- The learner or decision-maker that interacts with the environment.
- Example: A robot trying to navigate a maze.

**Properties**
- An agent is an active decision-making entity which interacts with its surroundings (environment)
- The agent must be able to sense the state of the environment to some extent and must be able to take actions that affect the state
- The agent has a goal to maximize its rewards by taking certain steps (actions)
- The agent must try a variety of actions and progressively favor those that appear to be best
- The agent must monitor its environment frequently and react appropriately since the effects of its actions cannot be fully predicted

---

### 2. Environment

- The world in which the agent operates and takes actions.
- Example: The maze in which the robot navigates.

**Properties**
- The environment sends signal (reward) to agent based on the agent's actions
- The environment has a 'state' which the agent must monitor

---
### 3. State (e)

- A representation of the current situation of the environment.
- Example: The current position of the robot in the maze.

**Properties**
- When the agent performs some action, the environment takes a certain form which is called 'state'

---
### 4. Action (a)

- The set of all possible moves the agent can make in a given state.
- Example: Moving up, down, left, or right in the maze.

---
### 5. Reward (r)

- A scalar feedback signal indicating the immediate benefit of the action taken.
- Example: +10 for reaching the goal, -1 for each move, -100 for hitting a wall.

---
### 6. Policy ($\pi$)

- A strategy used by the agent to determine its actions based on the current state.
- Example: A map that tells the robot which direction to move at each position in the maze.

---
### 7. Value Function ($V(s)$)

- The expected cumulative reward the agent can obtain from state `s` under a given policy.
- Example: The robot’s estimate of how close it is to the goal from a particular position.

---
### 8. Q-value or Action value ($Q(s,a)$)

- The expected cumulative reward of taking action `a` in state `s` and then following the policy.
Example: The robot’s estimate of the benefit of moving in a particular direction from a specific position.

---
### 9. Discount Factor($\gamma$)

- A factor between 0 and 1 that determines the importance of future rewards.
- Example: A discount factor of 0.9 means future rewards are considered less important than immediate rewards.

## Types of RL

### Model-Based RL

- Intuition
    - The agent builds or utilizes a model of the environment rather than directly interacting with the environment
    - Usually a state transition model or rewards model is learned from experience by observing transitions and rewards in the environment. In other cases, the model may be known a priori
    - It uses the model to simulate different action sequences and plan the best course of action
- Example 
    - a self driving car which uses a model of the city routes and traffic signals in a virtual environment and learns the dynamics of driving
- Pros
    - It is used where real-world interactions are costly or dangerous
    - The ability to plan and foresee future consequences can lead to faster learning
- Cons
    - Model may not represent the real environment correctly thereby inducing inaccurate, suboptimal or even harmful results
    - Building accurate model can be complex and computationally expensive (especially in high dimensional environments)

### Model-Free RL

- Intuition
    - The agent learns directly from experience without building a model of the environment
    - The agent learns the value of actions (Q-values) or the value of states (V-values) based on the rewards received from interacting with the environment.
    - The agent iteratively improves its policy by updating value functions based on observed rewards, without predicting future states or rewards
- Example
    - Q-learning, SARSA, and Deep Q-Networks (DQN)
- Pros
    - It is simpler to implement as it learns directly from enviroment without creating a model of the environment
    - No risk of model inaccuracies
    - It can scale effectively to high dimensional state and action spaces (e.g. DQN)
- Cons
    - It requires a large number of interactions with the environment to learn effectively, which can be costly or impractical in many real-world scenarios
    - Without the ability to plan, model-free agents may take longer to discover optimal policies, especially in complex environments