---
layout: post
title: "Cross Entropy"
date: 2024-08-14
tags: ["Information Theory"]
---

Cross entropy is an important concept which is widely used in machined learning.

---
### Intuition
- Cross entropy measures the difference between two probability distributions for a given random variable.
- Specifically, in terms of machine learning, there are two distributions of given input data X
    - p(X) represents the true probability distribution of X
    - q(X) represents the predicted probability distribution of X by the model

### Definition
- In general, Cross entropy $H(p,q)$ for two probability distributions p(X) and q(X) for a given random variable X can be defined as,

$$ H(p,q) = - \sum_{i=1}^n p(x_i) \log q(x_i) $$

---
### Properties
#### 1. Cross entropy is always non-negative

**Proof**

$$ \Rightarrow H(p,q) = -\sum_{i} p(x_i) \log \left(q( x_i) \right) >= 0 $$ 

Since $0 <= p(x), q(x) <= 1 $

---
#### 2. Cross entropy is asymmetric

**Proof**

$$ H(p,q) \ne H(q,p) $$ This follows from the definition

---
#### 3. Cross entropy is minimized when predicted distribution is exactly the same as actual distribution

**Proof** :

Minimize cross entropy

$$ H(p,q) = -\sum_{i} p \log q $$

subject to constraints,

$$ \sum_{i} p(x_i) = 1 $$

$$ \sum_{i} q(x_i) = 1 $$

Lagrangian can be set up as follows,

$$ L = - \sum_{i=1}^n p(x_i) \log q( x_i) + \lambda \left( \sum_{i=1}^n q(x_i) - 1 \right) + \phi \left( \sum_{i=1}^n p(x_i) - 1 \right)$$

Taking first order partial derivative,

$$ \frac {\partial L}{\partial {q_i}} = \frac {\partial }{\partial {q_i}} \left({ -p_i \log q_i} \right) + \frac {\partial }{\partial {q_i}}  \lambda q_i $$

$$ \Rightarrow \frac {\partial L}{\partial {q_i}} = -p_i({1 \over q_i}) + \lambda (1) $$

For minimum cross entropy,

$$ \frac{\partial L}{\partial q_i} = 0 $$

$$ \Rightarrow -({p_i \over q_i}) + \lambda = 0 $$

$$ p_i = \lambda q_i $$

Since probabilities sum to 1, we have

$$ \sum_{i} p(x_i) = 1 $$

$$ \Rightarrow \sum_{i} \lambda q_i = 1 $$

$$ \Rightarrow \lambda \sum_{i} q_i = 1 $$

$$ \Rightarrow \lambda (1) = 1 $$

$$ \Rightarrow \lambda = 1 $$

$$ \Rightarrow p_i = q_i $$

This means, Cross entropy is minimized when $p(x_i) = q(x_i)$

$$ H(p,q)_{min} = H(p,p) = H(p) $$

---

#### 4. Cross Entropy is a convex function

$$ H(p,q) = -\sum_{i} p \log q $$

Taking first order Partial Derivative of $H(p,q)$,

$$ \Rightarrow \frac {\partial H}{\partial {q_i}} = -({p_i \over q_i}) $$

Taking second-order partial derivative wrt each probability,

$$ \frac {\partial^2 H}{\partial {q_i}^2} = \frac {\partial}{\partial {q_i}} -({p_i \over q_i}) $$

$$ \Rightarrow \frac {\partial^2 H}{\partial {q_i}^2} = \frac {p_i}{q_i}^2 $$

$$ \Rightarrow \frac {\partial^2 H}{\partial {q_i}^2} > 0 $$ Since, $p_i > 0$ (probability is always positive)



---
### 