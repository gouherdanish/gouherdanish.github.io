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
### The Sigmoid Function

$$ \sigmoid(x) = \frac {1}{1+e^{-x}} $$

$where x \in {\mathbb{R}} \; \sigmoid(x) \in (0,1)$

Since, its output range is limited to (0,1), the Sigmoid function is very much suitable to represent probabilities

---
