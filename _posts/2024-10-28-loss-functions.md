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

<img src="{{site.url}}/images/loss_fn/nn1.png">

- In this article, we will understand what are the different loss functions used for classification

---
## 

<img src="{{site.url}}/images/loss_fn/loss1.png">


<img src="{{site.url}}/images/loss_fn/loss1.png">

### Negative Log Likelihood Loss

- NLL Loss for one example can be defined as,

$$ H(y,\hat{y}) = - \sum_{i=1}^n y_i \log \hat{y_i} $$

Here,
- $y$ denotes actual ground truth binary label which is either 0 or 1 
- $\hat{y}$ denotes predicted probability value which is a real value between 0 and 1 (represented by Softmax Output)
- `n` represents the number of output classes 
- `i` represents $i^{th}$ class

```
def nll(yhat,y):
    return -yhat[range(y.shape[0]),y].log().mean()

>>> loss = nll(yhat,y)
>>> loss
tensor(0.3306)
```

```

```

---



Github 
- [Logistic Regression from Scratch](https://github.com/gouherdanish/ml_concepts/blob/main/logistic_regression.py)
- [Logistic Regression using Sklearn](https://github.com/gouherdanish/ml_concepts/blob/main/logistic_regression_sklearn.py)

