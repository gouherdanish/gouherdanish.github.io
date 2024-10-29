---
layout: post
title: "Cross Entropy Loss : PyTorch Implementation"
date: 2024-10-29
tags: ["Machine Learning"]
---

Loss functions are used extensively in Machine Learning. Cross Entropy Loss is used in Classification problems

---
## Background

- In a classification problem, we predict what will be the category of the input data out of given classes
- E.g. In MNIST classification, we predict what is the digit in the given input image 
- Classification problem can be solved using Machine Learning by both traditional ML techniques and Deep Learning using Neural Networks
- Below is a Multilayer Perceptron (MLP) architecture for the MNIST classification

<img src="{{site.url}}/images/loss_fn/nn1.png">

---

## What is Cross Entropy Loss Function ?

- Previously, in [this](https://gouherdanish.github.io/2024/08/15/cross-entropy.html) article, we understood the concept behind Cross Entropy and 
- Also, in [this](https://gouherdanish.github.io/2024/08/16/properties-of-cross-entropy.html) article, we did a detailed mathematical analysis of the properties of cross entropy
- Intuitively Cross Entropy measures how well one probability distribution approximates another
- Cross Entropy Loss can be defined as,

$$ L = - \sum_{i=1}^n y_i \log p_i $$

Here,
- $y$ denotes actual ground truth class vector in one-hot encoded form 
- $p$ denotes predicted probability distribution which is a real value between 0 and 1 (represented by Softmax Output)
- `n` represents the number of output classes 
- `i` represents $i^{th}$ class

---
## How to calculate Cross Entropy ?

There are various approaches we can take to calculate Cross Entropy. However every approach boils down to couple important points

- whether we calculate softmax and apply log separately or we apply both softmax and log in one go
- whether we use our own implementations or we use PyTorch functions

Depending on the above, below we present possible approaches to calculate Cross Entropy in PyTorch

### 1. Using Own Implementation - Unstable Way

- The steps involved in this approach are outlined in the figure below

<img src="{{site.url}}/images/loss_fn/loss1.png">

- Below we explain each step in detail

**Step 1 - Calculate Softmax ($p_i$)**

- Softmax function is defined as follows

$$ p_i = \frac {e^{z_i}}{\sum_{j=1}^C e^{z_j}} $$

- Using Tensor algebra, softmax function can be implemented in PyTorch as follows

```
def softmax(z):
    return z.exp()/(z.exp().sum(-1)).unsqueeze(1)

>>> p = softmax(z)
>>> p
tensor([[0.2312, 0.1402, 0.6285],
        [0.0674, 0.8214, 0.1112]])
```

- Softmax function takes logits as input and gives predicted probability distribution as output

**Step 2 - Apply Log ($\log p_i$)**

- After calculating the probabilities, we apply log transformation to calculate log probabilities

```
>>> logp = torch.log(p)
>>> logp
tensor([[-1.4644, -1.9644, -0.4644],
        [-2.6967, -0.1967, -2.1967]])
```

**Step 3 - Dot Product ($y \dot \log p_i$)**

_Case 1 - Target y is in OHE Form_

```
>>> y = torch.tensor([[0,0,1],[0,1,0]],dtype=torch.float32)
>>> y
tensor([[0., 0., 1.],
        [0., 1., 0.]])
```

- In this case, we can implement dot product using `torch.dot` as follows
- Note that `torch.dot` works on 1D tensors, that's why we have to iterate over the input examples

```
def nll_loss(logp,y):
    torch.tensor([torch.dot(logp_,y_) for logp_,y_ in zip(logp,y)])

>>> loss = nll_loss(logp,y)
>>> loss
tensor([-0.4644, -0.1967])
```

_Case 2 - Target y is in Class Number form (0 to N)_

```
>>> y = torch.tensor([2,1])
>>> y
tensor([2,1])
```

- In this case, calculating dot product becomes much easier as we just have to take the log probability at yth index

```
def nll_loss(logp,y):
    return -logp[range(len(y)),y]

>>> loss = nll_loss(logp,y)
>>> loss
tensor([-0.4644, -0.1967])
```

### 2. Using Own Implementation - Stable Way

- 
- The steps involved in this approach are outlined in the figure below

<img src="{{site.url}}/images/loss_fn/loss1.png">

- Below we explain each step in detail

**Step 1 - Calculate LogSoftmax**

$$ \log p_i = z_i-z_{max} - \log {\sum_{j=1}^C e^{z_j-z_{max}}} $$

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

