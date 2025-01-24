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

- In this blog, we will understand how we varying the batch size of MLP model affects the Model accuracy

---
### Effect of Batch Size

- In Mini-batch Gradient Descent, Batch Size corresponds to the number of images that is fed forward to the network and back-propagated during one iteration
    - When Batch size = 1, it is called Stochastic Gradient Descent
    - When Batch size = Training Size, it is called Full Batch Gradient Descent

#### Case 1 - MLP having 1 Hidden Layer of 512 Neurons

**Model**
<img src="{{site.url}}/images/mnist/mlp-d1.png">

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Elapsed Time: 4.5192s
Train Accuracy : 92.3%
Val Accuracy : 94.9%
```

**Inference**

_Batch Inference_

```
>>> python batch_inference.py --model_name mlp
{'param_count': 407050, 'flops': 813056}
Test Accuracy : 96.2%
Elapsed Time: 0.3216s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.006s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     4.52 s    |        2.26 s           |       0.006 s     |   96.2%  |   813056    |   407050   |

---
#### Case 4 - MLP having 2 Hidden Layers of 1024 Neurons - Batch Size = 1

**Model**
<img src="{{site.url}}/images/mnist/mlp-w1.png">

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

_Batch Inference_

```
>>> python batch_evaluate.py --model_name mlp
{'size': '17.7 MB', 'params': 1470474, 'flops': 2936832, 'latency': '88.8 μs', 'accuracy': '93.6%', 'CO2eq': '1012.6 μgCO2eq'}
```

#### Case 5 - MLP having 2 Hidden Layers of 1024 Neurons - Batch Size = 64

**Model**
<img src="{{site.url}}/images/mnist/mlp-w1.png">

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

_Batch Inference_

```
>>> python batch_evaluate.py --model_name mlp
{'size': '17.7 MB', 'params': 1470474, 'flops': 2936832, 'latency': '10.3 μs', 'accuracy': '97.2%', 'CO2eq': '117.2 μgCO2eq'}
```

#### Case 6 - MLP having 2 Hidden Layers of 1024 Neurons - Batch Size = 100

**Model**
<img src="{{site.url}}/images/mnist/mlp-w1.png">

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Epoch 01, Train acc=0.932, Val acc=0.968, Train loss=0.002, Val loss=0.001
Epoch 02, Train acc=0.970, Val acc=0.970, Train loss=0.001, Val loss=0.001
Elapsed Time: 11.2474s
Train Accuracy : 95.1%
Val Accuracy : 96.9%
```

**Evaluation**

_Batch Inference_

```
>>> python batch_evaluate.py --model_name mlp
{'size': '17.7 MB', 'params': 1470474, 'flops': 2936832, 'latency': '9.1 μs', 'accuracy': '96.7%', 'CO2eq': '103.4 μgCO2eq'}
```

### Conclusion

- We explored online retraining of ML models in which we provide the capability to retrain for one mis-classified item
- We observed the pros/cons of performing online retraining as well