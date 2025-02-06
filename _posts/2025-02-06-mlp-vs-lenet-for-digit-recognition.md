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
- Continuing on that, we will introduce LeNet-5 architecture and compare its performance with MLP.

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
- With just 0.5 MB on disk, LeNet is around 14 times smaller in size than MLP as it has less number of parameters involved

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g1.png">

**Model Parameters**

- It refers to the total number of trainable weights and biases in the model
- MLP has high number of parameters due to the high number of weights and biases associated with fully connected layers
- In comparison, LeNet involves convolutional layers that use relatively smaller filters across the spatial dimension of image (weight sharing)

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g2.png">

**Floating Point Operations**

- It represents the number of arithmetic operations (multiplications, additions, etc.) required to process one input sample through the model 
- LeNet has higher Flops count since it involves performing a large number of multiplications and additions for each filter as they slide across the spatial dimensions of the input image
- This implies that LeNet is a computationally more complex model than MLP

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g3.png">

**Training Time**

- It represents the time it takes to run one epoch for the given ML model
- LeNet takes ~1.5 times longer to train due to higher Flops count

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g4.png">

**Latency**

- It represents the time it takes to run one inference for the given ML model
- LeNet takes ~4 times longer to do inference due to higher Flops count

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g5.png">

**Accuracy**

- It represents the number of correct predictions done per 100 test samples
- With 97.8% accuracy, LeNet gives substantial (>2%) improvement in Accuracy compared to MLP (95.7%)

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g6.png">

**Carbon Footprint**

- It represents the equivalent amount of $CO_2$ emitted during one inference
- LeNet has higher carbon footprint since it has higher latency 

<img src="{{site.url}}/images/mnist/mlp-vs-lenet-comp-g7.png">

---
### Observations

- LeNet gives substantial increase in Accuracy compared to MLP.
- But, being an inherently complex model, LeNet performs slower in Training and Inference. 
    - However, this can be improved using optimized hardware accelerators e.g. Nvidia cuDNN 
- LeNet's small size makes it ideal for use in edge devices

---
### References

Implementation and code can be found on Github
- [Digit Recognition App](https://github.com/gouherdanish/mnist_classification)

---
### Conclusion

- We introduced LeNet architecture and compared its performance with MLP
- We concluded that even with its extremely small size, LeNet is able to give higher Accuracy and comparable performance with respect to MLP 