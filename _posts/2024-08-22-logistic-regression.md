---
layout: post
title: "Logistic Regression: Concepts Deep Dive and Implementation from Scratch"
date: 2024-08-22
tags: ["Machine Learning"]
---

Logistic Regression is a powerful yet simple and explainable ML algorithm. Understanding its intricacies is vital to any ML practitioner.

Here we implement Logistic regression from scratch, we take a very simple example but will go very in-depth, presenting with theory and hand calculation at each step. 

We will also verify our results it with standard implementations fr 

That will make things a lot easier

---
### Assumptions

- Binary Classification
- One Predictor Variable

---
### Linear Model

Each example has just one predictor variable (`x`) which is fed into a linear model characteized by weight `w` and bias `b`

$$ z = w x + b $$

In this case, $ w,b \in {\mathbb{R}} $ i.e. these are scalar values

So, z will also be scalar $ z \in {\mathbb{R}} $

But, logistic regression requires the output as probabilities in the range (0,1)

---
### Sigmoid Function

When binary classification is involved, we can use Sigmoid function to limit the prediction in the range (0,1). 
Since, its output range is limited to (0,1), the Sigmoid function is very much suitable to represent probabilities

Sigmoid function can be given by,

$$ \hat{y} = \sigma(z) = \frac {1}{1+e^{-z}} $$

where, 

$$ z \in {\mathbb{R}} $$

$$ \sigma(z) \in (0,1) $$

---
### Cross Entropy Loss

- Cross entropy measures how well one probability distribution approximates another and is often used to quantify the difference between predicted and true distributions in machine learning tasks.

- Specifically, in the context of machine learning, 
    - $y$ represents the actual probability distribution 
    - $\hat{y}$ represents the predicted probability distribution

In this context, cross entropy for one example can be defined as,

$$ H(y,\hat{y}) = - \sum_{i=1}^n y_i \log \hat{y_i} $$

where, 
- `n` represents the number of output classes 
- `i` represents $i^{th}$ class

For Binary Classification, $n = 2$

$$ H(y,\hat{y}) = - y_1 \log \hat{y_1} - y_2 \log \hat{y_2} $$

Also, since probabilities sum to 1,

$$ y_1 + y_2 = 1 \implies y_2 = 1 - y_1 $$

$$ \hat{y_1} + \hat{y_2} = 1 \implies \hat{y_2} = 1 - \hat{y_1}$$

we can write the Binary Cross Entropy loss for one example as,

$$ l = H \left( y,\hat{y} \right) = - y \log \hat{y} - (1-y) \log (1-\hat{y}) $$

Average Cross Entropy loss over all `m` examples can be given by,

$$ L = \sum_{j=1}^m l_j = \sum_{j=1}^m H \left( y,\hat{y} \right) $$ 

$$ \Rightarrow L = \sum_{j=1}^m [- y_j \log \hat{y_j} - (1-y_j) \log (1-\hat{y_j})] $$

Here,
- y - actual ground truth binary label which is either 0 or 1 

$$ y \in {0,1} \implies $$ either 0 or \ 1

- \hat{y} - predicted probability value which is a real value between 0 and 1 (represented by Sigmoid function)

$$ \hat{y} = \sigma(z) = \frac {1}{1+e^{-z}} $$

$$ \hat{y} \in [0,1) \; real values $$
---

### Gradient of Loss Function

From above, we have derived the Binary cross entropy loss for one example,

$$ \ell_i = - \left[ y_i \log(\hat{y_i}) + (1 - y_i) \log(1 - \hat{y_i}) \right] $$

Taking first derivative w.r.t to model params,

$$ \frac{\partial \ell_i}{\partial \hat{w_i}} = $$ \frac{\partial \ell_i}{\partial \hat{y_i}} \times \frac{\partial \hat{y_i}}{\partial \hat{z_i}} \times \frac{\partial \z_i}{\partial \hat{w_i}}

#### 1. Calculate $ \frac{\partial \ell_i}{\partial \hat{y_i}} $

$$ \frac{\partial \ell_i}{\partial \hat{y_i}} = -\frac{y_i}{\hat{y_i}} + \frac{1 - y_i}{1 - \hat{y_i}} $$

#### 2. Calculate $ \frac{\partial \y_i}{\partial \hat{z_i}} $

$$ \frac{\partial \hat{y_i}}{\partial z_i} = \hat{y_i}(1 - \hat{y_i}) $$

#### 3. Calculate $ \frac{\partial \z_i}{\partial \hat{w_i}} $





