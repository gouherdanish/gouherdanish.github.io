---
layout: post
title: "Effect of Batch Size on Accuracy"
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
    - Energy Consumption
- These evaluation functions are implemented in the `evaluate.py` file inside the `eval` module
- This module is invoked while running the `batch_evaluate.py` file as follows which will load the model weights created from above and evaluate it based on the criteria outlined inside the `eval` module

```
>>> python batch_evaluate.py --model_name mlp
{'size': '9.8 MB', 'params': 814090, 'flops': 1626112, 'latency': '23.8 μs', 'accuracy': '96.7%', 'CO2eq': '271.6 μgCO2eq'}
```

---
### Model Comparison

**Architecture**

<img src="{{site.url}}/images/mnist/mlp-1024_1.png" alt="MLP-1">

<img src="{{site.url}}/images/mnist/mlp-1024_2.png" alt="MLP-2">

**Model Size**

<img src="{{site.url}}/images/mnist/mlp-comp-g1.png">

**Model Parameters**

<img src="{{site.url}}/images/mnist/mlp-comp-g2.png">

**Floating Point Operations**

<img src="{{site.url}}/images/mnist/mlp-comp-g3.png">

Note:
- We can observe that the 2nd MLP Model is bigger in terms of all three criteria presented above

---

### Effects of Batch Size

**Batch Size vs Training Time**

<img src="{{site.url}}/images/mnist/mlp-hp-g1.png">

**Batch Size vs Inference Latency**

<img src="{{site.url}}/images/mnist/mlp-hp-g2.png">

**Batch Size vs Accuracy**

<img src="{{site.url}}/images/mnist/mlp-hp-g3.png">

**Batch Size vs Energy Consumption**

<img src="{{site.url}}/images/mnist/mlp-hp-g4.png">

Note:
- We can observe that the Training Time, Inference Latency and the Energy Consumption decreases as the Batch Size increases all the while improving the Accuracy

### Conclusion

- We presented two MLP models and studied the dependency of various model evaluation parameters on Batch size
- We observed that Batch Size affects the Overall Model Performance significantly giving gains in terms of all the important criteria
