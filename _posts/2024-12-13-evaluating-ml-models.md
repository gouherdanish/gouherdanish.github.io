---
layout: post
title: "Evaluating ML Models"
date: 2024-12-13
tags: ["Machine Learning"]
---

Evaluating a Machine Learning model is essential in understanding the model complexity and eventually its usability

---

### Background

- In our [Digit Recognition App](https://gouherdanish.github.io/2024/12/09/digit-recognition.html), we have created two models, trained them with MNIST data and used them for inference.
- Below is a brief summary of these two models

**LeNet Model**
- LeNet Model had 5 layers
    - 2 Convolutional Layers
    - 2 Fully Connected Layers
    - 1 Output Layers of 10 neurons
- Refer [LeNet Model](https://github.com/gouherdanish/mnist_classification/blob/main/model/lenet.py)

**Evaluate MLP Model**
- MLP Model that we created for our App had 
    - 1 Fully connected layer of 512 neurons
    - 1 Output layer of 10 neurons
- Refer [MLP Model](https://github.com/gouherdanish/mnist_classification/blob/main/model/mlp.py)

---
### Training Time

- It refers to the time it takes to train a model for given number of epochs

**Create Timeit function**
- We can use Python `time` module to create a function which can calculate the time elapsed during a function call
- Below is a possible implementation of this function
- We will see how we can use this function to find training time for different models

```
import time

class Utils:
    def timeit(func):
        def inner(*args,**kwargs):
            start = time.time()
            res = func(*args,**kwargs)
            end = time.time()
            print(f"Elapsed Time: {(end-start):.4f}s")
            return res
        return inner
```

- Then we can use the above created function as a decorator as follows

```
@Utils.timeit
def train(...):
    ...
```

**LeNet Model**
- To evaluate the training time for LeNet model, we can run the batch training pipeline as follows
```
>>> python batch_training.py --model_name lenet --epochs 10
Epoch 01, Train acc=0.886, Val acc=0.948, Train loss=0.003, Val loss=0.001
Epoch 05, Train acc=0.986, Val acc=0.984, Train loss=0.000, Val loss=0.000
Epoch 10, Train acc=0.993, Val acc=0.983, Train loss=0.000, Val loss=0.000
Elapsed Time: 47.5396s <-- 
...
```

- Notice, training LeNet model took ~48 seconds for 10 epochs

**MLP Model**
- On similar lines, we can evaluate the training time for MLP model as follows

```
>>> python batch_training.py --model_name lenet --epochs 10
Epoch 01, Train acc=0.892, Val acc=0.941, Train loss=0.003, Val loss=0.002
Epoch 05, Train acc=0.977, Val acc=0.972, Train loss=0.001, Val loss=0.001
Epoch 10, Train acc=0.987, Val acc=0.970, Train loss=0.000, Val loss=0.001
Elapsed Time: 20.8068s <-- 
...
```

---
**Comparison**
- We can run for multiple epochs for each model to arrive at average time per epoch

| Model | Num Epochs | Elapsed Time (s) |  Avg Time per Epoch (s) |
| ----- | ---------- | ---------------- | ----------------------- |
| LeNet |     10     |       47.54 s    |           4.75 s        |
| MLP   |     10     |       20.80 s    |           2.80 s        |


---
### Inference Latency

- Inference latency is a measure of the average time it takes to classify each example
- We can use the same utility function to calculate the inference latency as well

**LeNet Model**
```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/1a.png --model_name lenet
{'confidence': tensor([0.9999]), 'pred_label': tensor([1])}
Elapsed Time: 0.0111s
```

**MLP Model**
```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/1a.png --model_name mlp
{'confidence': tensor([0.6122]), 'pred_label': tensor([1])}
Elapsed Time: 0.0244s   <--
```

**Comparison**
- We can run for multiple epochs for each model to arrive at average time per epoch

| Model |  Avg Time per Epoch (ms) |
| ----- | ------------------------ |
| LeNet |           11.1 ms        |
| MLP   |           24.4 ms        |

---

### Parameter Count

- It refers to the total number of trainable weights and biases in the model.
- It is a measure of the model size
- In PyTorch, there is a `numel()` method which gives the number of elements in a given tensor
- We can use this method to create a function which calculates the total parameter count of a given model

```
def count_params(model):
    return sum([p.numel() for p in model.parameters() if p.requires_grad])
```

**LeNet Model**

```
>>> from model.lenet import LeNet 
>>> model = LeNet()
>>> count_params(model)
44426
```

**MLP Model**

```
>>> from model.mlp import MLP
>>> model = MLP()
>>> count_params(model)
407050
```

**Comparison**

| Model |  Parameter Count |
| ----- | ---------------- |
| LeNet |     44426        |
| MLP   |     407050       |

---
### Floating Point Operations (Flops) 

- It represents the number of arithmetic operations (multiplications, additions, etc.) required to process one input sample through the model.
- FLOPs depend on the input size (e.g., height and width for images, sequence length for text).
- It is a measure of the computational complexity of the model for a given input.

Note:
- There is no standard function in PyTorch to give Flops Count right off the bat. So, we need to implement this function from scratch
- Note that, the method to calculate Flops differs based on the type of layer, so we need to take this into account
- We have provided the implementation [here](https://github.com/gouherdanish/mnist_classification/blob/main/eval/evaluate.py)

**LeNet Model**

```
>>> from model.lenet import LeNet
>>> model = LeNet()
>>> from eval.evaluate import ModelEvaluator
>>> ModelEvaluator.count_flops(model,input_size=(1,28,28))
2176080
```

**MLP Model**
```
>>> from model.mlp import MLP
>>> model = MLP()
>>> from eval.evaluate import ModelEvaluator
>>> ModelEvaluator.count_flops(model,input_size=(1,28,28))
2176080
```

**Comparison**

| Model |  Parameter Count |
| ----- | ---------------- |
| LeNet |     44426        |
| MLP   |     407050       |


---
### Reference

Implementation and code can be found on Github
- [Digit Recognition App](https://github.com/gouherdanish/mnist_classification)

---
### Conclusion
- We evaluated the efficiency of ML model based on following 
    - Training time and Inference latency
    - Parameter Count 
    - Floating Point Operations or Flops

