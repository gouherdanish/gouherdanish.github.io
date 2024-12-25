---
layout: post
title: "Effect of Model Depth and Width on Accuracy"
date: 2024-12-25
tags: ["Machine Learning"]
---

Accuracy of ML model depends on multiple factors

---

### Background

- In our [Digit Recognition App](https://gouherdanish.github.io/2024/12/09/digit-recognition.html), we created very simple MLP model with just one hidden layer, trained it with MNIST data and used it for inference.

In this blog, we will understand how we varying the depth and width of MLP model affects the Model accuracy

---
### Case Studies

#### Case 1 - MLP having 1 Hidden Layer of 512 Neurons

**Model**
<img src="{{site.url}}/images/mnist/mlp-d1.png">

#### Case 2 - MLP having 1 Hidden Layer of 256 Neurons

**Model**
<img src="{{site.url}}/images/mnist/mlp-w1.png">

#### Case 3 - MLP having 1 Hidden Layer of 128 Neurons

**Model**
<img src="{{site.url}}/images/mnist/mlp-w2.png">

#### Case 4 - MLP having 1 Hidden Layer of 1024 Neurons

**Model**
<img src="{{site.url}}/images/mnist/mlp-w3.png">

#### Case 5 - MLP having 2 Hidden Layers

**Model**
<img src="{{site.url}}/images/mnist/mlp-d2.png">

#### Case 6 - MLP having 3 Hidden Layers

**Model**
<img src="{{site.url}}/images/mnist/mlp-d3.png">

---
### Conclusion

- We explored online retraining of ML models in which we provide the capability to retrain for one mis-classified item
- We observed the pros/cons of performing online retraining as well