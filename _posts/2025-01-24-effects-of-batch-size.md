---
layout: post
title: "Effect of Batch Size"
date: 2024-12-25
tags: ["Machine Learning"]
---

Accuracy of ML model depends on multiple factors

---

### Background

- In our [Digit Recognition App](https://gouherdanish.github.io/2024/12/09/digit-recognition.html), we created very simple MLP model with just one hidden layer, trained it with MNIST data and used it for inference.

- In this blog, we will understand how varying the batch size affects the Model accuracy and others

---
### Concept

- In Gradient Descent, Batch Size corresponds to the number of images that is fed forward to the network and back-propagated during one iteration
    - When Batch size = 1, it is called Stochastic Gradient Descent
    - When Batch size = Training Size, it is called Full Batch Gradient Descent
    - For intermediate scenarios, it is termed as Mini-Batch Gradient Descent

---
### Methodology

**Case Studies**

- For this exercise, we will study two MLP models with the following configurations
    - MLP having 1 Hidden Layer of 1024 Neurons
    - MLP having 2 Hidden Layers of 1024 Neurons each

Note:
    - In case of 2nd model, we will observe a huge jump in number of model parameters due to extra layer of neurons
    - This will cause training to become extremely slow 
    - So, we introduce an extra Centercrop transform to reduce the input size from 28*28 to 20*20
    - This effectively reduces the number of model parameters and aids in faster training

- For each of these models, we will study the effects of following Batch sizes
    - Batch size = 1
    - Batch size = 16
    - Batch size = 64
    - Batch size = 128

Note:
    - We can modify the batch size and number of neurons inside the `constants.py` file

**Training**

- We can use the `batch_training.py` file to train an MLP model as follows

```
>>> python batch_training.py --model_name mlp --epochs 2
Epoch 01, Train acc=0.911, Val acc=0.935, Train loss=0.018, Val loss=0.013
Epoch 02, Train acc=0.955, Val acc=0.968, Train loss=0.009, Val loss=0.007
Elapsed Time: 19.2111s
Train Accuracy : 93.3%
Val Accuracy : 95.2%
```

- This creates a `mnist_mlp_weights.pth` file in the `out` directory

**Evaluation**

- Next, we evaluate these two models on multiple criteria as highlighted below
    - Model Size
    - Number of Model Parameters
    - Floating Point Operations
    - Training Time
    - Inference Latency
    - Accuracy
    - Carbon Footprint
- These evaluation functions are implemented in the `evaluate.py` file inside the `eval` module
- This module is invoked while running the `batch_evaluate.py` file as follows which will load the model weights created from above and evaluate it based on the criteria outlined inside the `eval` module

```
>>> python batch_evaluate.py --model_name mlp
{'size': '9.8 MB', 'params': 814090, 'flops': 1626112, 'latency': '23.8 μs', 'accuracy': '96.7%', 'CO2eq': '271.6 μgCO2eq'}
```

---

### Effects of Batch Size

**Batch Size vs Training Time**

- From below graph, we observe that Training time decreases as Batch size increases
- However, at Batch Size = 64, the training time starts to level off and increasing the Batch Size does not show much improvement

<img src="{{site.url}}/images/mnist/mlp-hp-g1.png">

**Batch Size vs Inference Latency**

- From below graph, we observe that Inference Latency decreases as Batch size increases
- However at Batch Size = 64, the latency starts to level off and increasing the Batch Size does not show much improvement

<img src="{{site.url}}/images/mnist/mlp-hp-g2.png">

**Batch Size vs Accuracy**

- From below graph, we observe that Accuracy initially increases as Batch size increases
- Interestingly, increasing the Batch size beyond a certain point causes the Accuracy to decrease. So there exists an "Ideal" batch size which works best
- We note that depending on the model, there can be different "ideal" Batch sizes
    - Batch Size = 16 works best for 1st MLP model
    - Batch Size = 64 works best for 2nd MLP model
- In general, it is up to the user to select the Batch size for their specific use case

<img src="{{site.url}}/images/mnist/mlp-hp-g3.png">

**Batch Size vs Energy Consumption**

- In technical terms, the Energy Consumption represents the amount of $CO_2$ emitted with one inference
- It is directly dependent on the Inference Latency as defined in this [article](https://gouherdanish.github.io/2025/01/28/measuring-carbon-footprint-of-ml-inference.html)
- From below graph, we observe that Energy Consumption decreases as Batch size increases
- However at Batch Size = 64, the latency starts to level off and increasing the Batch Size does not show much improvement

<img src="{{site.url}}/images/mnist/mlp-hp-g4.png">

---
### Model Comparison

- We have seen that for most practical purposes, a batch size of 64 can be considered since it provides a good balance between performance and accuracy 
- Let's compare the two models that we have created above using the evaluation metrics

**Architecture**

_MLP-1_
<img src="{{site.url}}/images/mnist/mlp-1024_1.png" alt="MLP-1">

_MLP-2_
<img src="{{site.url}}/images/mnist/mlp-1024_2.png" alt="MLP-2">

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
- - MLP-2 has higher Flops count since the extra neurons add more computation overhead 

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

<img src="{{site.url}}/images/mnist/mlp-comp-g4.png">

---
### Observations

- Although bigger in size, MLP-2 adds very liitle to the Accuracy compated to MLP-1
- Due to the extra layer of neurons, the computational complexity increases which causes MLP-2 performance to deteriorate as evident from higher Training Time and Inference Latency 
- Due to its higher latency, MLP-2 has higher Carbon Footprint and therefore has worse impact on the environment

---
### Conclusion

- We presented two MLP models and studied the dependency of various model evaluation parameters on Batch size
- We observed that Batch Size affects the Overall Model Performance significantly giving gains in terms of all the important criteria
- Also we concluded that having a bigger model is not always better. We must focus on balancing performance and accuracy to get better results
