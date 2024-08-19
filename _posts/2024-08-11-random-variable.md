---
layout: post
title: "Random Variable: How to quantify random events"
date: 2024-08-11
tags: ["Probability"]
---

Random Variable is a very important concept in Probability and forms the base in the study of Probability Distributions

---

### Intuition
- Random variable assings a numerical value to the outcome of a random experiment
- The random variable always takes on real values

### Types 

#### 1. Discrete Random Variable

- It can take distinct and countable values
- E.g. Tossing a fair coin three times, if `X` is random variable which represents the number of heads obtained, then it can take integer values 0, 1, 2 or 3 corresponding to getting no heads, one heads, two heads or three heads respectively
- In this case, the probability distribution is a **Probability Mass Function** given by,

$$ P(X=0) = {1 \over 8} \; P(X=1) = {3 \over 8} \; P(X=2) = {3 \over 8} \; P(X=3) = {1 \over 8} $$

#### 2. Continuous Random Variable

- It can take any value within a continuous range
- E.g. Time to complete a task, if X represents the time taken in minutes, then it can take any positive real values
- In this case, the probability distribution is a **Probability Density Function** which (assuming normal distribution) can be given by

$$ f(x) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right) $$

### Hand Calculation

#### Experiment 1 - Die Roll
Consider an experiment where a fair die is rolled and the number on top is observed. Here are the following outcomes

$$ Sample Space = \{1,2,3,4,5,6\} $$

Let $X$ denote a random variable which represents an boolean event whether or not 6 was observed

$$ X = \{0,1\} $$

The probability $p(X)$ associated with each $X$

$$ p(X=0) = p(outcome is 1 or\ 2 or\ 3 or\ 4 or\ 5) = {5 \over 6} $$

$$ p(X=1) = p(outcome is 6) = {1 \over 6} $$

#### Experiment 2 - Coin Flip
Consider an experiment where a fair coin is flipped 3 times. Here are following outcomes

$$ Sample Space = \{HHH,HHT,HTH,THH,HTT,THT,TTH,TTT\} $$

Let $X$ denote a random variable which represents the number of heads in 3 consecutive coin flips

$$ X = \{0,1,2,3\} $$

The probability $p(X)$ associated with each $X$

$$ p(X=0) = p(TTT) = 1/8 $$

$$ p(X=1) = p(HTT or THT or TTH) = 3/8 $$

$$ p(X=2) = p(HHT or HTH or THH) = 3/8 $$

$$ p(X=3) = p(HHH) = 1/8 $$

---
### Implementation
```
from random_event import RandomEvent
from random_variable import RandomVariable

events = [
        RandomEvent(0,1/6),
        RandomEvent(1,5/6)
    ]
X = RandomVariable(events)  # X = {0: 0.167, 1: 0.833}
```

For full implementation, refer following repository

Github - [Stats Concepts](https://github.com/gouherdanish/stats_concepts/blob/main/random_variable.py)