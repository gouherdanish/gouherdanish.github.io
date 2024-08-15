---
layout: post
title: "Properties of Cross Entropy"
date: 2024-08-15
tags: ["Information Theory"]
---
Entropy has several desirable properties. Some of these properties are presented below along with their mathematical proofs.

---
### 1. Cross entropy is always non-negative

**Proof**

$$ \Rightarrow H(p,q) = -\sum_{i} p(x_i) \log \left(q( x_i) \right) >= 0 $$ 

Since $0 <= p(x), q(x) <= 1 $

---
### 2. Cross entropy is asymmetric

**Proof**
$$ H(p,q) = -\sum_{i} p_i \log q_i $$

$$ H(q,p) = -\sum_{i} q_i \log p_i $$

$$ \Rightarrow H(p,q) \ne H(q,p) $$ 

---
### 3. Cross entropy is minimized when predicted distribution is exactly the same as actual distribution

**Proof** :

Minimize cross entropy

$$ H(p,q) = -\sum_{i} p_i \log q_i $$

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

---

### 4. Minimum value of Cross entropy is equal to the entropy of actual distribution

$$ H(p,q) = -\sum_{i} p_i \log q_i $$

From Property 3, Cross entropy is minimized when $p(x_i) = q(x_i)$

$$ H(p,q)_{min} = H(p,p) = -\sum_{i} p_i \log p_i = H(p) $$

---

### 5. Cross Entropy is a convex function

$$ H(p,q) = -\sum_{i} p_i \log q_i $$

Taking first order Partial Derivative of $H(p,q)$,

$$ \Rightarrow \frac {\partial H}{\partial {q_i}} = -({p_i \over q_i}) $$

Taking second-order partial derivative wrt each probability,

$$ \frac {\partial^2 H}{\partial {q_i}^2} = \frac {\partial}{\partial {q_i}} \left(-{p_i \over q_i} \right) $$

$$ \Rightarrow \frac {\partial^2 H}{\partial {q_i}^2} = {p_i} \over {q_i}^2 $$

$$ \Rightarrow \frac {\partial^2 H}{\partial {q_i}^2} > 0 $$ Since, $p_i, q_i > 0$ (probability is always positive)

Since its second derivative is always negative, Cross Entropy function is concave (curved upwards).

---
### 6. Relative Entropy denotes the information lost when q is used to estimate p

$$ H(p,q) = -\sum_{i} p_i \log q_i $$

$$ H(p,q) = -\sum_{i} p_i \log p_i \left({q_i} \over {p_i} \right) $$

$$ H(p,q) = -\sum_{i} p_i \log p_i -\sum_{i} p_i \log \left({q_i} \over {p_i} \right) $$

$$ H(p,q) = H(p) + D_{KL}(p,q) $$