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

- In this blog, we will understand how we varying the batch size of MLP model affects the Model accuracy

---
### Effect of Batch Size

- In Mini-batch Gradient Descent, Batch Size corresponds to the number of images that is fed forward to the network and back-propagated during one iteration
    - When Batch size = 1, it is called Stochastic Gradient Descent
    - When Batch size = Training Size, it is called Full Batch Gradient Descent

### Case 1 - MLP having 1 Hidden Layer of 1024 Neurons

**Model**
<img src="{{site.url}}/images/mnist/mlp-1024_1.png">

#### Batch Size = 16

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Epoch 01, Train acc=0.911, Val acc=0.935, Train loss=0.018, Val loss=0.013
Epoch 02, Train acc=0.955, Val acc=0.968, Train loss=0.009, Val loss=0.007
Elapsed Time: 19.2111s
Train Accuracy : 93.3%
Val Accuracy : 95.2%
```

**Evaluation**

```
>>> python batch_inference.py --model_name mlp
{'size': '9.8 MB', 'params': 814090, 'flops': 1626112, 'latency': '23.8 μs', 'accuracy': '96.7%', 'CO2eq': '271.6 μgCO2eq'}
```

---
#### Batch Size = 64

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Epoch 01, Train acc=0.911, Val acc=0.936, Train loss=0.005, Val loss=0.003
Epoch 02, Train acc=0.959, Val acc=0.959, Train loss=0.002, Val loss=0.002
Elapsed Time: 8.5666s
Train Accuracy : 93.5%
Val Accuracy : 94.7%
```

**Evaluation**

```
>>> python batch_inference.py --model_name mlp
{'size': '9.8 MB', 'params': 814090, 'flops': 1626112, 'latency': '5.1 μs', 'accuracy': '96.4%', 'CO2eq': '58.2 μgCO2eq'}
```

---
#### Batch Size = 128

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Epoch 01, Train acc=0.898, Val acc=0.945, Train loss=0.003, Val loss=0.001
Epoch 02, Train acc=0.956, Val acc=0.962, Train loss=0.001, Val loss=0.001
Elapsed Time: 6.9615s
Train Accuracy : 92.7%
Val Accuracy : 95.4%
```

**Evaluation**

```
>>> python batch_inference.py --model_name mlp
{'size': '9.8 MB', 'params': 814090, 'flops': 1626112, 'latency': '4.3 μs', 'accuracy': '96.4%', 'CO2eq': '49.0 μgCO2eq'}
```

---

### Case 2 - MLP having 2 Hidden Layers of 1024 Neurons

**Model**
<img src="{{site.url}}/images/mnist/mlp-1024_2.png">

#### Batch Size = 1

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Epoch 01, Train acc=0.901, Val acc=0.931, Train loss=0.409, Val loss=0.315
Epoch 02, Train acc=0.935, Val acc=0.935, Train loss=0.312, Val loss=0.326
Elapsed Time: 408.0490s
Train Accuracy : 91.8%
Val Accuracy : 93.3%
```

**Evaluation**

```
>>> python batch_evaluate.py --model_name mlp
{'size': '17.7 MB', 'params': 1470474, 'flops': 2936832, 'latency': '88.8 μs', 'accuracy': '93.6%', 'CO2eq': '1012.6 μgCO2eq'}
```

#### Batch Size = 16

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Epoch 01, Train acc=0.929, Val acc=0.952, Train loss=0.015, Val loss=0.009
Epoch 02, Train acc=0.964, Val acc=0.953, Train loss=0.008, Val loss=0.010
Elapsed Time: 33.5073s
Train Accuracy : 94.6%
Val Accuracy : 95.3%
```

**Evaluation**

```
>>> python batch_evaluate.py --model_name mlp
{'size': '17.7 MB', 'params': 1470474, 'flops': 2936832, 'latency': '32.0 μs', 'accuracy': '95.3%', 'CO2eq': '364.8 μgCO2eq'}
```

#### Batch Size = 64

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Epoch 01, Train acc=0.932, Val acc=0.945, Train loss=0.003, Val loss=0.003
Epoch 02, Train acc=0.969, Val acc=0.972, Train loss=0.002, Val loss=0.001
Elapsed Time: 13.1912s
Train Accuracy : 95.0%
Val Accuracy : 95.8%
```

**Evaluation**

```
>>> python batch_evaluate.py --model_name mlp
{'size': '17.7 MB', 'params': 1470474, 'flops': 2936832, 'latency': '10.3 μs', 'accuracy': '97.2%', 'CO2eq': '117.2 μgCO2eq'}
```

#### Batch Size = 128

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Epoch 01, Train acc=0.932, Val acc=0.952, Train loss=0.002, Val loss=0.001
Epoch 02, Train acc=0.972, Val acc=0.964, Train loss=0.001, Val loss=0.001
Elapsed Time: 10.1892s
Train Accuracy : 95.2%
Val Accuracy : 95.8%
```

**Evaluation**

```
>>> python batch_evaluate.py --model_name mlp
{'size': '17.7 MB', 'params': 1470474, 'flops': 2936832, 'latency': '9.0 μs', 'accuracy': '96.6%', 'CO2eq': '102.6 μgCO2eq'}
```

### Conclusion

- We explored online retraining of ML models in which we provide the capability to retrain for one mis-classified item
- We observed the pros/cons of performing online retraining as well