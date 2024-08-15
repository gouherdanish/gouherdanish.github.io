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