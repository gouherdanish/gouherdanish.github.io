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

$$ z =  $$

### Sigmoid Function

$$ \sigma(x) = \frac {1}{1+e^{-x}} $$

$where x \in {\mathbb{R}} \; \sigma(x) \in (0,1)$

Since, its output range is limited to (0,1), the Sigmoid function is very much suitable to represent probabilities

---
### Cross Entropy Loss

We know, Cross entropy $H(p,q)$ for two probability distributions p(X) and q(X) for a given random variable X can be defined as,

$$ H(p,q) = - \sum_{i=1}^n p(x_i) \log q(x_i) $$

- Specifically, in terms of machine learning,
    - p(X) represents the true probability distribution of X \implies y
    - q(X) represents the predicted probability distribution of X by the model \Rightarrow \hat{y}

- Intuitively, Cross entropy measures how well one probability distribution approximates another and is often used to quantify the difference between predicted and true distributions in machine learning tasks. So, we can write,

$$ H(y,\hat{y}) = - \sum_{i=1}^n y \log \hat{y} $$
