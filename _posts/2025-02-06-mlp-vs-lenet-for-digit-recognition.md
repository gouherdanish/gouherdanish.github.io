---
layout: post
title: "Comparing MLP and LeNet for Digit Recognition"
date: 2025-02-06
tags: ["Machine Learning"]
---

Two ML models can be compared based on evaluation metrics depending on performance, accuracy and environmental impact.

---

### Background

- In [previous](https://gouherdanish.github.io/2025/02/01/comparing-ml-models.html) article, we compared two MLP models for Digit Recognition task on MNIST data
- Continuing on that, we will introduce LeNet-5 architecture and compare its performance with MLP .

---
### Model Comparison

- Given two ML models, we can evaluate each one based on the above evaluation strategy and compare the values to arrive at a comparative result
    - Model Size
    - Number of Model Parameters
    - Floating Point Operations
    - Training Time
    - Inference Latency
    - Accuracy
    - Carbon Footprint
- Let's compare the two models that we have created above using the evaluation metrics

Note:
- We take a batch size of 64 for both the ML models since it provides a good balance between performance and accuracy. Refer [this](https://gouherdanish.github.io/2024/12/25/effects-of-batch-size.html) article

**Architecture**

_MLP-5_
<img src="{{site.url}}/images/mnist/mlp-5.png" alt="MLP-5">

_LeNet-5_
<img src="{{site.url}}/images/mnist/lenet.png" alt="LeNet-5">

**Model Size**

- It refers to the size of the model on disk
- LeNet is around 14 times smaller in size than MLP

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g1.png">

**Model Parameters**

- It refers to the total number of trainable weights and biases in the model
- MLP has high number of parameters due to the high number of weights and biases associated with fully connected layers
- In comparison, LeNet involves convolutional layers that use relatively smaller filters across the spatial dimension of image (weight sharing)

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g2.png">

**Floating Point Operations**

- It represents the number of arithmetic operations (multiplications, additions, etc.) required to process one input sample through the model 
- MLP-2 has higher Flops count since the extra neurons add more computation overhead 

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g3.png">

**Training Time**

- It represents the time it takes to run one epoch for the given ML model
- MLP-2 takes longer to train due to higher number of parameters

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g4.png">

**Latency**

- It represents the time it takes to run one inference for the given ML model
- MLP-2 takes longer to do inference due to more number of parameters

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g5.png">

**Accuracy**

- It represents the number of correct predictions done per 100 test samples
- MLP-2 (97.2%) has very little and incremental improvement compared to MLP-1 (96.4%)

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g6.png">

**Carbon Footprint**

- It represents the equivalent amount of $CO_2$ emitted during one inference
- MLP-2 has higher carbon footprint since it has higher latency 

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g7.png">

---
### Observations

- Although bigger in size, MLP-2 adds very liitle to the Accuracy compated to MLP-1
- Due to the extra layer of neurons, the computational complexity increases which causes MLP-2 performance to deteriorate as evident from higher Training Time and Inference Latency 
- Due to its higher latency, MLP-2 has higher Carbon Footprint and therefore has worse impact on the environment

---
### References

Implementation and code can be found on Github
- [Digit Recognition App](https://github.com/gouherdanish/mnist_classification)

---
### Conclusion

- We explored multiple ways or criteria to evaluate a given ML model
- We concluded that having a bigger model is not always better. We must focus on balancing performance and accuracy to get better results