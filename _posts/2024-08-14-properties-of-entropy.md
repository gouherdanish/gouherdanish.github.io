---
layout: post
title: "Properties of Entropy"
date: 2024-08-14
tags: ["Information Theory"]
---

Entropy has several desirable properties. Some of these properties are presented below along with their mathematical proofs.

---
### 1. Entropy is always non-negative

**Proof** :

We know, probability of any event lies in (0,1)

$$ \Rightarrow 0 <= p(x) <= 1 $$

$$ \Rightarrow \log \left( p(x) \right) <= 0 $$

$$ \Rightarrow -\log \left( p(x) \right) >= 0 $$

$$ \Rightarrow -p(x) \log \left( p(x) \right) >= 0 $$

$$ \Rightarrow H(X) = -\sum_{i} p(x_i) \log \left(p( x_i) \right) >= 0 $$

---

### 2. Entropy is maximum when all outcomes are equally likely

**Proof** : 

Objective Function for this optimization problem can be formulated as follows

Maximize Entropy, 

$$ H(X) = -\sum_{i} p(x_i) \log  (p( x_i)) $$

subject to constraint that probabilities sum to 1

$$ \sum_{i} p(x_i) = 1 $$

The Lagrangian $\mathcal(L)$ can be formulated using Lagrange multiplier theorem as follows,

$$ L = - \sum_{i=1}^n p(x_i) \log p( x_i) + \lambda \left( \sum_{i=1}^n p(x_i) - 1 \right) $$

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

---
### 3. Maximum Entropy of a random variable is `log n`

**Proof** : 

$$ H(X) = - \sum_{i=1}^n p(x_i) \log {p(x_i)}  $$

From property 2, we know that Entropy is maximum for a uniform distribution for which $ p(x_i) = {1 \over n} $

Substituting,

$$ \Rightarrow H_{max} = -\sum_{i=1}^n {1 \over n} \log \left( {1 \over n} \right) $$

$$ \Rightarrow H_{max} = -{1 \over n} \log \left( {1 \over n} \right) \sum_{i=1}^n 1 $$

$$ \Rightarrow H_{max} = - \left( {1 \over n} \log \left( {1 \over n} \right) \right) (n)$$

$$ \Rightarrow H_{max} = - \log {1 \over n} $$

$$ \Rightarrow H_{max} = \log n $$

Intuitively, the uniform distribution spreads the probabilities evenly across all outcomes thus, achieving the highest possible entropy 
`log n` for `n` distinct outcomes

---
### 4. Entropy is a concave function of the probability distribution

**Proof** : 

$$ H = - \sum_{i=1}^n p_i \log p_i $$

$$ H = - p_1 \log p_1 - p_2 \log p_2 - ... - p_i \log p_i - ... -p_n \log p_n $$

Taking first-order partial derivative wrt each probability,

$$ \frac {\partial H}{\partial p_i} = \frac {\partial}{\partial p_i} ({- p_i \log p_i})

$$ \Rightarrow \frac {\partial H}{\partial p_i} = ({-1 - \log {p_i}}) $$

Taking second-order partial derivative wrt each probability,

$$ \frac {\partial^2 H}{\partial {p_i}^2} = \frac {\partial}{\partial {p_i}} ({-1 - \log {p_i}})$$

$$ \Rightarrow \frac {\partial^2 H}{\partial {p_i}^2} = -\frac {1}{p_i} $$

$$ \Rightarrow \frac {\partial^2 H}{\partial {p_i}^2} < 0 $$ Since, $p_i > 0$ (probability is always positive)

Since its second derivative is always negative, Entropy function is concave.
Intuitively, this means entropy function has decreasing first derivative(slope of tangent), making it bend
downwards, therefore the curvature is concave.

### 5. Entropies of independent random variables are additive

**Proof** : 

If X and Y are independent random variables, then Joint probability distribution of X and Y can be written as,

$$ P(X=x_i and Y=y_i) = P(X=x_i) \times P(Y=y_i) $$

Joint Entropy can be defined as follows

$$ H(X,Y) = -\sum_{i,j} p(x_i,y_i) \log p(x_i,y_i) $$

$$ \Rightarrow H(X,Y) = -\sum_{i,j} p(x_i)p(y_i) \log ({p(x_i) p(y_i)}) $$

$$ \Rightarrow H(X,Y) = -\sum_{i,j} p(x_i)p(y_i) (\log {p(x_i) + \log p(y_i)}) $$

$$ \Rightarrow H(X,Y) = -\sum_{i,j} p(x_i)p(y_i) \log {p(x_i) -\sum_{i,j} p(x_i)p(y_i)  \log p(y_i)} $$

### Implementation
For full implementation, refer following repository

Github - [Stats Concepts](https://github.com/gouherdanish/stats_concepts/git)