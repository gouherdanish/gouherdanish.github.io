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
Test Accuracy : 95.9%
Elapsed Time: 0.3216s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0185s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     4.52 s    |        2.26 s           |       0.018 s     |   95.9%  |   813056    |   407050   |

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
Test Accuracy : 95.2%
Elapsed Time: 0.3012s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0204s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     4.15 s    |        2.08 s           |       0.020 s     |   95.2%  |   406528    |   203530   |

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
Test Accuracy : 94.3%
Elapsed Time: 0.3069s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0115s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     3.93 s    |        1.97 s           |       0.011 s     |   94.3%  |   203264    |   101770   |

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
Test Accuracy : 96.0%
Elapsed Time: 0.3648s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0175s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     4.96 s    |        2.48 s           |       0.017 s     |   96.0%  |   1626112   |   814090   |

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
Test Accuracy : 95.9%
Elapsed Time: 0.3216s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0185s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     4.52 s    |        2.26 s           |       0.018 s     |   95.9%  |   813056    |   407050   |

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
Test Accuracy : 96.1%
Elapsed Time: 0.3371s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0158s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     4.83 s    |        2.41 s           |       0.016 s     |   96.1%  |   1337344   |   669706   |

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
Test Accuracy : 96.5%
Elapsed Time: 0.3450s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0171s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     5.30 s    |        2.65 s           |       0.017 s     |   96.5%  |   1861632   |   932362   |

#### Case 4 - MLP having 2 Hidden Layers

**Model**
<img src="{{site.url}}/images/mnist/mlp-d2.png">

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Elapsed Time: 4.7826s
Train Accuracy : 92.5%
Val Accuracy : 95.4%
```

**Inference**

_Batch Inference_

```
>>> python batch_inference.py --model_name mlp
{'param_count': 535818, 'flops': 1070080}
Test Accuracy : 95.9%
Elapsed Time: 0.3257s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0103s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     4.78 s    |        2.39 s           |       0.010 s     |   95.9%  |   1070080   |   535818   |

#### Case 4 - MLP having 3 Hidden Layers

**Model**
<img src="{{site.url}}/images/mnist/mlp-d3.png">

**Training**

```
>>> python batch_training.py --model_name mlp --epochs 2
Elapsed Time: 4.7195s
Train Accuracy : 91.9%
Val Accuracy : 95.0%
```

**Inference**

_Batch Inference_

```
>>> python batch_inference.py --model_name mlp
{'param_count': 567434, 'flops': 1133056}
Test Accuracy : 95.8%
Elapsed Time: 0.3286s
```

_Incremental Inference_

```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/3.png
{'confidence': tensor([0.9987]), 'pred_label': tensor([3])}
Elapsed Time: 0.0112s
```

**Summary**

| Model | Num Epochs | Training Time | Training Time per Epoch | Inference Latency | Accuracy |    Flops    | Parameters |
| ----- | ---------- | ------------- | ----------------------- | ----------------- | -------- | ----------- | ---------- |
|  MLP  |      2     |     4.72 s    |        2.36 s           |       0.011 s     |   95.8%  |   1133056   |   567434   |

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