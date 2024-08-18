---
layout: post
title: "Cross Entropy - Measuring randomness induced while estimation"
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

#### Example - Biased Coin Toss

Consider a biased coin which gives Head 4 out of 5 times. The Random variable, X can be represented as,

$$ X = (H,T) $$

True Probability distribution of X can be given by,

$$ p(H) = 0.8, \; p(T) = 0.2 $$

Entropy (Uncertainty) in the true distribution can be calculated as,

$$ H(p) = -p(H) \log p(H) - p(T) \log p(T) $$

$$ H(p) = -0.8 \log 0.8 - 0.2 \log 0.2 $$

$$ H(p) = 0.5004024235381879 $$

Assume, there is a model which predicts that the coin is un-biased 

Predicted Probability distribution of X is given by,

$$ Q(H) = 0.5, \; Q(T) = 0.5 $$

Cross Entropy induced because of using Q to predict P can be given by

$$ H(p,q) = -p(H) \log q(H) - p(T) \log q(T) $$

$$ H(p,q) = -0.8 \log 0.5 - 0.2 \log 0.5 $$

$$ H(p,q) = 0.6931471805599453 $$

KL-Divergence can be calculated as,

$$ D_{KL}(p,q) = H(p,q) - H(p) $$

$$ D_{KL}(p,q) = 0.6931471805599453 - 0.5004024235381879 $$

$$ D_{KL}(p,q) = 0.19274475702175742 $$

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

print(p.entropy())          # H(p)      = 0.5004024235381879
print(p.cross_entropy(q))   # H(p,q)    = 0.6931471805599453
print(p.kl_divergence(q))   # D(p,q)    = 0.19274475702175742
```

For full implementation, refer following repository

Github - [Stats Concepts](https://github.com/gouherdanish/stats_concepts/blob/main/random_variable.py)