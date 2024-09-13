---
layout: post
title: "Reinforcement Learning - Concepts Revision"
date: 2024-09-01
tags: ["Reinforcement Learning"]
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

---
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

---
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

---
### Stationary Problems

- Intuition
    - Environment is fixed
    - The transition probabilities, reward functions, and state dynamics remain constant throughout the learning process.
    - These are relatively simpler and more common in academic examples
- Example
    - N-armed bandit problem is stationary when each arm has a fixed mean reward
- Usage
    - Exploration can gradually decrease over time as the agent becomes more confident in its estimates. 
    - An agent might use strategies like decaying epsilon in the epsilon-greedy method.
    - The agent's policy and value function can converge to the optimal policy as the learning process stabilizes
    - Algorithms like Q-learning, SARSA, and others assume stationarity and perform well when the environment is fixed.

---
### Non-stationary Problems

- Intuition
    - Environment can change over time
    - The reward distribution associated with each action can change over time
- Example
    - N-armed bandit problem is stationary when reward for each arm fluctuates over time. In this case, an arm that is optimal at one point might become suboptimal later
    - user preferences in recommendation systems, market conditions in trading algorithms, or strategies in competitive games can change over time.
- Usage
    - Ongoing exploration is necessary because the optimal action may change over time. 
    - Agent might use techniques like adaptive epsilon-greedy or sliding window averages are used to ensure the agent remains responsive to changes
    - The agent's policy may need to continuously adapt, preventing convergence to a single optimal policy. Instead, the agent might converge to a strategy that balances recent information with exploration
    - Algorithms that incorporate mechanisms to adapt to changing environments, such as contextual bandits, thompson sampling, or meta-learning, are better suited for non-stationary problems.
    - Requires the agent to be more flexible and robust

---
### On-Policy RL

- Intuition
    - The agent learns the value of the policy it is currently following
    - Learns and improves the same policy that is used for decision-making
    - This means that the policy used to make decisions during exploration (**behaviour policy**) is the same as the policy being optimized (**update policy**)
- Pros
    - Can be more stable and may converge to a policy that performs well under the specific conditions of exploration
- Cons
    - It might be less efficient in finding the optimal policy because it is constrained by the need to balance exploration and exploitation simultaneously
- Example
    - SARSA (State-Action-Reward-State-Action) is an on-policy algorithm
- Usage
    - Good for situations where the policy’s behavior needs to be safe and stable.

---
### Off-Policy RL

- Intuition
    - Learns the optimal policy independently of the decision-making process. 
    - It is more flexible and sample-efficient but can be more complex and less stable.
- Pros
    - Typically more sample-efficient and capable of converging to the optimal policy faster since it uses the best possible action (even if it wasn’t taken)
- Cons
    - It can be less stable and may require more careful tuning to ensure convergence, especially when using function approximation
- Example
    - Q-learning is an off-policy algorithm
- Usage
    - When you want to learn from a variety of experiences, such as replaying past experiences (experience replay in DQN) or using data generated by different agents or policies