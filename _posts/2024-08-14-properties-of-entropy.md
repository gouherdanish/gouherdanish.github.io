---
layout: post
title: "Properties of Entropy"
date: 2024-08-14
tags: ["Information Theory"]
---

Entropy has several desirable properties. Some of these properties are presented below along with their mathematical proofs.

---
### 1. Entropy is always non-negative

Proof:

We know, probability of any event lies in (0,1)

$$ \Rightarrow 0 <= p(x) <= 1 $$

$$ \Rightarrow \log (p(x)) <= 0 $$

$$ \Rightarrow -\log (p(x)) >= 0 $$

$$ \Rightarrow -p(x) \log (p(x)) >= 0 $$

$$ \Rightarrow H(X) = -\sum_{i} p(x_i) \log  (p( x_i)) >= 0 $$

---

### 2. Entropy is maximum when all outcomes are equally likely

Proof : 

Objective Function: Maximize Entropy $H(X)$, 

$$ H(X) = -\sum_{i} p(x_i) \log  (p( x_i)) $$

subject to constraint that probabilities sum to 1

$$ \sum_{i} p(x_i) = 1 $$

The Lagrangian $\mathcal(L)$ can be formulated using Lagrange multiplier theorem as follows,

$$ L = - \sum_{i=1}^n p(x_i) \log p( x_i) + \lambda (\sum_{i=1}^n p(x_i) - 1) $$

$$ \Rightarrow L = - p(x_1) \log p(x_1) - p(x_2) \log p(x_2) - ... - p(x_n) \log p(x_n) + \lambda (p(x_1) + p(x_2) + ... + p(x_n) - 1) $$

Finding partial derivatives of the Lagrangian with respect to $p(x_1)$,

$$ \frac{\partial L}{\partial p(x_1)} = -p(x_1){1 \over p(x_1)} - \log p(x_1)(1) + \lambda (1) $$ 

$$ \Rightarrow \frac{\partial L}{\partial p(x_1)} = \lambda - 1 - \log p(x_1)$$

Similarly we can find the partial derivates with respect to each $p(x_i)$,

$$ \frac{\partial L}{\partial p(x_i)} = \lambda - 1 - \log p(x_i) $$

For maximum entropy,

$$ \frac{\partial L}{\partial p(x_i)} = 0 $$

$$ \Rightarrow \lambda - 1 - \log p(x_i) = 0 $$

$$ \Rightarrow p(x_i) = e^{\lambda - 1} = Constant = C (say) $$

Hence, for maximum entropy, each probability has to be same and equal to a constant

Since, probabilities sum to 1

$$ \sum_{i=1}^n p(x_i) = 1 $$

$$ \Rightarrow \sum_{i=1}^n C = 1 $$

$$ \Rightarrow C \sum_{i=1}^n 1 = 1 $$

$$ \Rightarrow C \times n = 1 $$

$$ \Rightarrow C = p(x_i) = {1 \over n} $$

which is the Uniform Probability Distribution.

This means that entropy is maximum when all outcomes are equally likely.

Intuitively, the uniform distribution spreads the probabilities evenly across all outcomes thus, achieving the highest possible entropy 
$\log n$ for `n` distinct outcomes

### Implementation
For full implementation, refer following repository

Github - [Stats Concepts](https://github.com/gouherdanish/stats_concepts/git)

---