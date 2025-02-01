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

- Next, we evaluate given ML model on multiple criteria which are implemented in the `evaluate.py` file inside the `eval` module
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
### References

Implementation and code can be found on Github
- [Digit Recognition App](https://github.com/gouherdanish/mnist_classification)

---
### Conclusion

- We presented two MLP models and studied the dependency of various model evaluation parameters on Batch size
- We observed that Batch Size affects the Overall Model Performance significantly giving gains in terms of all the important criteria
