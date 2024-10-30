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
- E.g. In Iris Dataset, we have 150 examples covering 3 types of Iris flowers
- Let's say we want to build some classfication model to predict the type of Iris Flower given its features
- During forward propagation, we apply our model over the extracted features which yields us logit values as shown below

<img src="{{site.url}}/images/loss_fn/logit.png">

- Later, we combine these logits with our target values to calculate some loss metric which measures how well our model did
- Cross Entropy Loss function is used to measure loss in case of Classification Problems

---

## What is Cross Entropy Loss Function ?

- Previously, in [this](https://gouherdanish.github.io/2024/08/15/cross-entropy.html) article, we understood the concept behind Cross Entropy 
- Later, in [this](https://gouherdanish.github.io/2024/08/16/properties-of-cross-entropy.html) article, we did a detailed mathematical analysis of the various properties of cross entropy and why it is ideal as a loss function
- Intuitively Cross Entropy measures how well one probability distribution approximates another
- Cross Entropy Loss for one input example can be defined as,

$$ \ell = - \sum_{i=1}^n y_i \log p_i $$

- Here,
    - $y$ denotes actual ground truth class vector either in one-hot encoded form or class number form (explained in later sections)
    - $p$ denotes predicted probability distribution which is a real value between 0 and 1 (represented by Softmax Output)
    - `n` represents the number of output classes 
    - `i` represents $i^{th}$ class

- Cross Entropy Loss for `m` examples can be calculated as the mean of all the individual losses calculated above

$$ L = \frac{1}{m} \sum_{k=1}^m \ell_k $$


---
## How to calculate Cross Entropy ?

- There are various approaches we can take to calculate Cross Entropy. However every approach boils down to the way we apply individual PyTorch functions
- We present possible approaches to calculate Cross Entropy in PyTorch below
- For all the approaches we describe below, we consider the following logits (z) and target values (y)

```
>>> z = torch.tensor([[1,0.5,2],[0.5,3,1]])
>>> y = torch.tensor([2,1])
```

---
### Approach 1 - Using Softmax function from PyTorch 

- We will follow the steps as outlined below

<img src="{{site.url}}/images/loss_fn/loss1.png">

- This can be implemented using PyTorch functions as follows

```
>>> p = nn.functional.softmax(z)                # Step 1 - Calculate Softmax
>>> logp = torch.log(p)                         # Step 2 - Apply log
>>> ce_loss = nn.functional.nll_loss(logp,y)    # Step 3 - Find CE loss
>>> ce_loss
tensor(0.3306)
```

Note:
- PyTorch uses normalized version of softmax to handle underflow/overflow issues with exponentiation
- This is explained in detail in [this](https://gouherdanish.github.io/2024/10/28/softmax.html) article

---

### Approach 2 - Using LogSoftmax function from PyTorch 

- We will follow the steps outlined below

<img src="{{site.url}}/images/loss_fn/loss2.png">

- But now, instead of using our own implementation, we can use PyTorch functions

```
>>> logp = nn.functional.log_softmax(z)         # Step 1 - Calculate log softmax
>>> ce_loss = nn.functional.nll_loss(logp,y)    # Step 2 - Find CE loss
>>> ce_loss
tensor(0.3306)
```

Note:
- Log Softmax function is explained in detail in [this](https://gouherdanish.github.io/2024/10/28/softmax.html) article

---

### Approach 3 - Using Cross Entropy Loss function from PyTorch 

- We will follow the steps as shown below

<img src="{{site.url}}/images/loss_fn/loss3.png">

```
>>> ce_loss = nn.functional.cross_entropy(z,y)  # Step 1 to 4 all combined
>>> ce_loss
tensor(0.3306)
```

---
## References

[Cross Entropy Loss Notebook](https://github.com/gouherdanish/ml_concepts/blob/main/cross_entropy_loss.ipynb)

---
## Conclusion:

- We explored various ways of calculating cross-entropy loss




