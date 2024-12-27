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
### Effect of Hidden Layer Width

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
#### Case 2 - MLP having 1 Hidden Layer of 256 Neurons

**Model**
<img src="{{site.url}}/images/mnist/mlp-w1.png">

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Elapsed Time: 4.1488s
Train Accuracy : 91.4%
Val Accuracy : 93.6%
```

**Inference**

_Batch Inference_

```
>>> python batch_inference.py --model_name mlp
{'param_count': 203530, 'flops': 406528}
Test Accuracy : 95.8%
Elapsed Time: 0.3012s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0055s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     4.15 s    |        2.08 s           |      0.0055 s     |   95.8%  |   406528    |   203530   |

---
#### Case 3 - MLP having 1 Hidden Layer of 128 Neurons

**Model**
<img src="{{site.url}}/images/mnist/mlp-w2.png">

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Elapsed Time: 3.9315s
Train Accuracy : 90.5%
Val Accuracy : 92.7%
```

**Inference**

_Batch Inference_

```
>>> python batch_inference.py --model_name mlp
{'param_count': 101770, 'flops': 203264}
Test Accuracy : 93.7%
Elapsed Time: 0.3069s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0053s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     3.93 s    |        1.97 s           |       0.0053 s     |   93.7%  |   203264    |   101770   |

---

#### Case 4 - MLP having 1 Hidden Layer of 1024 Neurons

**Model**
<img src="{{site.url}}/images/mnist/mlp-w3.png">

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Elapsed Time: 4.9569s
Train Accuracy : 93.1%
Val Accuracy : 95.5%
```

**Inference**

_Batch Inference_

```
>>> python batch_inference.py --model_name mlp
{'param_count': 814090, 'flops': 1626112}
Test Accuracy : 96.7%
Elapsed Time: 0.3648s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0062s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     4.96 s    |        2.48 s           |      0.0062 s     |   96.7%  |   1626112   |   814090   |

---
#### Observations

_Accuracy vs Flops_

- Flops represent the number of floating-point operations which is related to the computational complexity of a model
- Increasing the width of the model increases the number of neurons thereby increasing flops
- As flops increase, model becomes more and more complex which enable it to capture more complex patterns in the data thereby increasing the accuracy
- However, after a certain point, increasing the width yields minimal increase in accuracy 
- It might so happen that accuracy starts decreasing if the network becomes excessively wide and starts overfitting

<img src="{{site.url}}/images/mnist/mlp-w-g1.png">

_Accuracy vs Latency_

- Latency represents the time it takes to do inference on one sample
- Increasing the width of the model increases the number of neurons which enables the model to capture complex patterns in data, thereby increasing the accuracy
- However it comes at cost of increased number of calculations thereby increasing the latency
- Higher the latency, higher the accuracy

<img src="{{site.url}}/images/mnist/mlp-w-g2.png">

_Flops vs Latency_

- As neural network becomes wider, number of neurons increase thereby increasing flops count
nes due to which latency also increases

<img src="{{site.url}}/images/mnist/mlp-w-g3.png">

---
### Effect of Depth of Neural Network

#### Case 1 - MLP having 1 Hidden Layer

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
Test Accuracy : 96.0%
Elapsed Time: 0.3216s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0057s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     4.52 s    |        2.26 s           |      0.0057 s     |   96.0%  |   813056    |   407050   |

#### Case 2 - MLP having 2 Hidden Layers

**Model**
<img src="{{site.url}}/images/mnist/mlp-d2_1.png">

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Elapsed Time: 4.8258s
Train Accuracy : 93.0%
Val Accuracy : 95.4%
```

**Inference**

_Batch Inference_

```
>>> python batch_inference.py --model_name mlp
{'param_count': 669706, 'flops': 1337344}
Test Accuracy : 96.3%
Elapsed Time: 0.3371s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0061s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     4.83 s    |        2.41 s           |      0.0061 s     |   96.3%  |   1337344   |   669706   |

#### Case 3 - MLP having 3 Hidden Layers

**Model**
<img src="{{site.url}}/images/mnist/mlp-d3_1.png">

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Elapsed Time: 5.3044s
Train Accuracy : 92.4%
Val Accuracy : 95.2%
```

**Inference**

_Batch Inference_

```
>>> python batch_inference.py --model_name mlp
{'param_count': 932362, 'flops': 1861632}
Test Accuracy : 96.4%
Elapsed Time: 0.3450s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0062s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     5.30 s    |        2.65 s           |      0.0062 s     |   96.4%  |   1861632   |   932362   |

---
#### Observations

_Accuracy vs Flops_

- Flops represent the number of floating-point operations which is related to the computational complexity of a model
- Increasing the depth of the model increases the number of neurons thereby increasing flops
- As flops increase, model becomes more and more complex which enable it to capture more complex patterns in the data thereby increasing the accuracy
- However, after a certain point, increasing the depth yields minimal increase in accuracy 
- It might so happen that accuracy starts decreasing if the network becomes excessively deep and starts overfitting

<img src="{{site.url}}/images/mnist/mlp-d-g1.png">

_Accuracy vs Latency_

- Latency represents the time it takes to do inference on one sample
- Increasing the depth of the model increases the number of neurons which enables the model to capture complex patterns in data, thereby increasing the accuracy
- However it comes at cost of increased number of calculations thereby increasing the latency
- Higher the latency, higher the accuracy

<img src="{{site.url}}/images/mnist/mlp-d-g2.png">

_Flops vs Latency_

- As neural network becomes deeper, number of neurons increase thereby increasing flops count
nes, due to which latency also increases

<img src="{{site.url}}/images/mnist/mlp-d-g3.png">

---
### Effect of Batch Size

#### Case 1 - MLP having 1 Hidden Layer of 512 Neurons with Batch Size 16

**Model**
<img src="{{site.url}}/images/mnist/mlp-d1.png">

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Elapsed Time: 7.3879s
Train Accuracy : 93.2%
Val Accuracy : 95.5%
```

**Inference**

_Batch Inference_

```
>>> python batch_inference.py --model_name mlp
{'param_count': 407050, 'flops': 813056}
Test Accuracy : 96.0%
Elapsed Time: 0.3899s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0176s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     7.39 s    |        3.70 s           |       0.017 s     |   96.0%  |   813056    |   407050   |

#### Case 2 - MLP having 1 Hidden Layer of 512 Neurons with Batch Size 32

**Model**
<img src="{{site.url}}/images/mnist/mlp-d1.png">

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Elapsed Time: 7.3879s
Train Accuracy : 93.2%
Val Accuracy : 95.5%
```

**Inference**

_Batch Inference_

```
>>> python batch_inference.py --model_name mlp
{'param_count': 407050, 'flops': 813056}
Test Accuracy : 96.0%
Elapsed Time: 0.3899s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0176s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     7.39 s    |        3.70 s           |       0.017 s     |   96.0%  |   813056    |   407050   |

### Conclusion

- We explored online retraining of ML models in which we provide the capability to retrain for one mis-classified item
- We observed the pros/cons of performing online retraining as well