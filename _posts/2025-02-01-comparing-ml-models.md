---
layout: post
title: "Comparing two ML Models"
date: 2025-02-01
tags: ["Machine Learning"]
---

Two ML models can be compared based on evaluation metrics depending on performance, accuracy and environmental impact.

---

### Model Evaluation

- An ML model can be evaluated based on multiple criteria such as
    - Model Size
    - Number of Model Parameters
    - Floating Point Operations
    - Training Time
    - Inference Latency
    - Accuracy
    - Carbon Footprint
- Multiple evaluation functions are implemented in the `evaluate.py` file inside the `eval` module
- This module is invoked while running the `batch_evaluate.py` file as follows which will load the model weights created from above and evaluate it based on the criteria outlined inside the `eval` module
- Refer [Digit Recognition App](https://github.com/gouherdanish/mnist_classification)

```
>>> python batch_evaluate.py --model_name mlp
{'size': '9.8 MB', 'params': 814090, 'flops': 1626112, 'latency': '23.8 μs', 'accuracy': '96.7%', 'CO2eq': '271.6 μgCO2eq'}
```

---
### Model Comparison

- Given two ML models, we can evaluate each one based on the above evaluation strategy and compare the values to arrive at a comparative result
- We take a batch size of 64 for both the ML models since it provides a good balance between performance and accuracy. Refer [this](https://gouherdanish.github.io/2024/12/25/effects-of-batch-size.html) article
- Let's compare the two models that we have created above using the evaluation metrics

**Architecture**

_MLP-1_
<img src="{{site.url}}/images/mnist/mlp-1024_1.png" alt="MLP-1">

_MLP-2_
<img src="{{site.url}}/images/mnist/mlp-1024_2.png" alt="MLP-2">

Note:
- In case of 2nd model, we will observe a huge jump in number of model parameters due to extra layer of neurons
- This will cause training to become extremely slow 
- So, we introduce an extra Centercrop transform to reduce the input size from $28 \times 28$ to $20 \times 20$
- This effectively reduces the number of model parameters and aids in relatively faster training

**Model Size**

- It refers to the size of the model on disk
- MLP-2 is larger in size due to the extra hidden layer

<img src="{{site.url}}/images/mnist/mlp-comp-g1.png">

**Model Parameters**

- It refers to the total number of trainable weights and biases in the model
- MLP-2 has more number of parameters since extra neurons add more weights and biases

<img src="{{site.url}}/images/mnist/mlp-comp-g2.png">

**Floating Point Operations**

- It represents the number of arithmetic operations (multiplications, additions, etc.) required to process one input sample through the model 
- MLP-2 has higher Flops count since the extra neurons add more computation overhead 

<img src="{{site.url}}/images/mnist/mlp-comp-g3.png">

**Training Time**

- It represents the time it takes to run one epoch for the given ML model
- MLP-2 takes longer to train due to higher number of parameters

<img src="{{site.url}}/images/mnist/mlp-comp-g5.png">

**Latency**

- It represents the time it takes to run one inference for the given ML model
- MLP-2 takes longer to do inference due to more number of parameters

<img src="{{site.url}}/images/mnist/mlp-comp-g6.png">

**Accuracy**

- It represents the number of correct predictions done per 100 test samples
- MLP-2 (97.2%) has very little and incremental improvement compared to MLP-1 (96.4%)

<img src="{{site.url}}/images/mnist/mlp-comp-g4.png">

**Carbon Footprint**

- It represents the equivalent amount of $CO_2$ emitted during one inference
- MLP-2 has higher carbon footprint since it has higher latency 

<img src="{{site.url}}/images/mnist/mlp-comp-g7.png">

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