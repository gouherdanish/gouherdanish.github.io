---
layout: post
title: "Cross Entropy"
date: 2024-08-15
tags: ["Information Theory"]
---

Cross entropy is an important concept which is widely used in machined learning.

---
### Intuition
- Cross entropy measures the difference between two probability distributions for a given random variable.
- Intuitively it measures how well one probability distribution approximates another, often used to quantify the difference between predicted and true distributions in machine learning tasks.

### Definition
- In general, Cross entropy $H(p,q)$ for two probability distributions p(X) and q(X) for a given random variable X can be defined as,

$$ H(p,q) = - \sum_{i=1}^n p(x_i) \log q(x_i) $$

- Specifically, in terms of machine learning,
    - p(X) represents the true probability distribution of X
    - q(X) represents the predicted probability distribution of X by the model

---
### Properties
- Cross entropy is always non-negative
- Cross entropy is asymmetric
- Cross entropy is minimized when predicted distribution is exactly the same as actual distribution
- Minimum value of Cross entropy is equal to the entropy of actual distribution
- Cross Entropy is a convex function

---

### Hand Calculation

#### Example 1 - Biased Coin Toss



---

### Formulation

```
import numpy as np

def entropy(p):
    return np.dot(p, np.log(p))

def cross_entropy(p,q):
    return np.dot(p, np.log(q))

def kl_divergence(p,q):
    return cross_entropy(p,q) - entropy(p)

p = [0.8, 0.2]  # true distribution
q = [0.5, 0.5]  # predicted distribution

print(entropy(p))           # 0.5004024235381879
print(cross_entropy(p,q))   # 0.6931471805599453
print(kl_divergence(p,q))   # 0.19274475702175742
```

---

### Implementation

```
from random_variable import RandomVariable

p = RandomVariable([('H',0.8),('T',0.2)])   # true distribution
q = RandomVariable([('H',0.5),('T',0.5)])   # predicted distribution

print(p.entropy())          # H(p)          = 0.5004024235381879
print(p.cross_entropy(q))   # H(p,q)        = 0.6931471805599453
print(p.kl_divergence(q))   # $D_{KL}(p,q)$ = 0.19274475702175742
```

For full implementation, refer following repository

Github - [Stats Concepts](https://github.com/gouherdanish/stats_concepts/blob/main/random_variable.py)