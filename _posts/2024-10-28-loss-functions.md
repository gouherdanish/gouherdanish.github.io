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
>>> z = model(X)    # X: input data, z: logits
>>> z
tensor([[1,0.5,2],
        [0.5,3,1]])
```

### Softmax Function (nn.functional.softmax)

**Definition**
- To interpret these logits as probabilities, we usually apply the softmax function, which converts the logits into values between 0 and 1 that sum to 1
- Softmax is defined as follows

$$ Softmax(z) = \frac {e^{z_j}}{\sum_{j} e^{z_j}} $$

**Implementation**
```
def softmax(z):
    return z.exp()/(z.exp().sum(-1)).unsqueeze(1)

>>> yhat = softmax(z)
>>> yhat
tensor([[0.2312, 0.1402, 0.6285],
        [0.0674, 0.8214, 0.1112]])
```

**Verification**
- This can be verified using PyTorch implementation

```
>>> import torch.nn as nn
>>> yhat = nn.functional.softmax(z)
>>> yhat
tensor([[0.2312, 0.1402, 0.6285],
        [0.0674, 0.8214, 0.1112]])
```

- Note that, for each example row, the probabilities now sum to 1

```
0.2312 + 0.1402 + 0.6285 = 1.0
0.0674 + 0.8214 + 0.1112 = 1.0
```

**Limitation**
z = torch.tensor([[1,0.5,2],[0.5,3,1],[1000,2000,-200],[-1000.0, -1000.0, 1000.0]])
yhat = softmax(z)
yhat

### LogSoftmax Function (nn.functional.log_softmax)

- To interpret these logits as probabilities, we usually apply the softmax function, which converts the logits into values between 0 and 1 that sum to 1
- Softmax is defined as follows

$$ Softmax(z) = \frac {e^{z_j}}{\sum_{j} {1+e^{z_j}}} $$

```
def softmax(z):
    return z.exp()/(z.exp().sum(-1)).unsqueeze(1)

>>> yhat = softmax(z)
>>> yhat
tensor([[0.2312, 0.1402, 0.6285],
        [0.0674, 0.8214, 0.1112]])
```

- This can be verified using PyTorch implementation

```
>>> import torch.nn as nn
>>> yhat = nn.functional.softmax(z)
>>> yhat
tensor([[0.2312, 0.1402, 0.6285],
        [0.0674, 0.8214, 0.1112]])
```

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

