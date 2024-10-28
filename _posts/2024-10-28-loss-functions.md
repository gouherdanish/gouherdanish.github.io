---
layout: post
title: "Loss Functions in Classification"
date: 2024-10-28
tags: ["Machine Learning"]
---

Loss functions are used extensively in Machine Learning.

---
## Background

- In a classification problem, we predict what will be the category of the input data out of given classes
- E.g. In MNIST classification, we predict what is the digit in the given input image 
- Classification problem can be solved using Machine Learning by both traditional ML techniques and Deep Learning using Neural Networks
- Below is a Multilayer Perceptron (MLP) architecture for the MNIST classification

<img src="{{site.url}}/images/loss_fn/nn.png">

- In this article, we will understand what are the different loss functions used for classification

---
## Concepts

- Before diving into details, it is customary to understand few key concepts

### Logits

- When a neural network is used for classification, the final layer typically outputs a vector of scores, where each element corresponds to a specific class. These scores are known as logits

- These values are not probabilities; they are simply the model's raw outputs.
- For example, in a classification problem with 3 classes, the logits might look like 

```
>>> z = model(X)
>>> z
tensor([[1,0.5,2],
        [0.5,3,1]])
```

### Softmax Function

$$ Softmax(z) = \frac {e^{z_j}}{\sum_{j}{1+e^{z_j}}} $$

```
def softmax(z):
    return z.exp()/(z.exp().sum(-1)).unsqueeze(1)

>>> softmax(z)
tensor([[0.2312, 0.1402, 0.6285],
        [0.0674, 0.8214, 0.1112]])
```

### Negative Log Likelihood Loss (NLL Loss)


---



Github 
- [Logistic Regression from Scratch](https://github.com/gouherdanish/ml_concepts/blob/main/logistic_regression.py)
- [Logistic Regression using Sklearn](https://github.com/gouherdanish/ml_concepts/blob/main/logistic_regression_sklearn.py)

