---
layout: post
title: "Evaluating ML Models"
date: 2024-12-13
tags: ["Machine Learning"]
---

Evaluating a Machine Learning model is essential in understanding the model complexity and eventually its usability

---

### Background

- In our [Digit Recognition App](https://gouherdanish.github.io/2024/12/09/digit-recognition.html), we have created two ML models, trained them with MNIST data and used them for inference.
- Below is a brief summary of these two models

**MLP Model**
- MLP Model that we created for our App had 2 Fully Connected layers viz.
    - 1 Fully connected hidden layer of 512 neurons
    - 1 Output layer of 10 neurons

<img src="{{site.url}}/images/mnist/mlp.png">

- For implementation, refer [MLP Model](https://github.com/gouherdanish/mnist_classification/blob/main/model/mlp.py) 

**LeNet Model**
- For our App, we had build LeNet-5 model which had 5 layers, viz.
    - 2 Convolutional layers
    - 2 Fully Connected hidden layers
    - 1 Fully Connected output layer of 10 neurons

<img src="{{site.url}}/images/mnist/lenet.png">

- For implementation, refer [LeNet Model](https://github.com/gouherdanish/mnist_classification/blob/main/model/lenet.py)

---

### Model Evaluation

- Evaluating a model means benchmarking it based on its performance, speed, size and usability
- There are various parameters which help us evaluate a model objectively
    - Training Time
    - Inference Latency
    - Parameter Count
    - Flops Count
- Below we study each of these parameter in detail and present a comparative evaluation of MLP and LeNet models

---

#### 1. Training Time

- It refers to the time it takes to train a model for given number of epochs
- We can use Python `time` module to create a function which can calculate the time elapsed during a function call
- Below is a possible implementation of this function

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

- In order to find the training time for different models, we have applied the above created function as a decorator on the train function as shown below

```
@Utils.timeit
def train(...):
    ...
```

- For implementation details, refer [training_factory.py](https://github.com/gouherdanish/mnist_classification/blob/main/factory/training_factory.py)

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

- Notice, training MLP model took ~21 seconds for 10 epochs

**Comparison**

| Model | Num Epochs | Elapsed Time (s) |  Avg Time per Epoch (s) |
| ----- | ---------- | ---------------- | ----------------------- |
| LeNet |     10     |       47.54 s    |           4.75 s        |
| MLP   |     10     |       20.80 s    |           2.80 s        |


---
#### 2. Inference Latency

- Inference latency is a measure of the average time it takes to classify each example
- We can use the above timeit utility function to calculate the inference latency as well

**LeNet Model**
```
>>> python single_inference.py --img ../data/sample/1a.png --model_name lenet
{'confidence': tensor([0.9999]), 'pred_label': tensor([1])}
Elapsed Time: 0.0111s
```

**MLP Model**
```
>>> python single_inference.py --img ../data/sample/1a.png --model_name mlp
{'confidence': tensor([0.6122]), 'pred_label': tensor([1])}
Elapsed Time: 0.0244s   <--
```

**Comparison**

| Model |  Inference Latency (ms)  |
| ----- | ------------------------ |
| LeNet |           11.1 ms        |
| MLP   |           24.4 ms        |

Note:
- There might be cases when doing first inference takes longer but subsequent inferences take less time. This happens due to cold start issues. The time for further inference thereafter.
- Hence, a possibly better way to calculate latency would be to take an average of multiple inferences as follows

```
inferencing = InferenceFactory.get(strategy='batch',model=model)
for batch_X, batch_y in dataloader:
    start_time = time.time()
    batch_acc = inferencing._infer_batch(batch_X, batch_y)
    end_time = time.time()
    runtime = end_time - start_time
    latency = runtime / len(batch_y)
```
- This has been implemented in `eval` module [here](https://github.com/gouherdanish/mnist_classification/tree/main/eval)

---

#### 3. Parameter Count

- It refers to the total number of trainable weights and biases in the model.
- It is a measure of the model size

Note:
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
#### 4. Floating Point Operations (Flops) 

- It represents the number of arithmetic operations (multiplications, additions, etc.) required to process one input sample through the model.
- Flops count depend on the input size (e.g., height and width for images, sequence length for text).
- It is a measure of the computational complexity of the model for a given input.

Note:
- There is no standard function in PyTorch to give Flops Count right off the bat. So, we need to implement this function from scratch
- The method to calculate Flops differs based on the type of layer, so we need to take this into account
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
813056
```

**Comparison**

| Model |  Flops Count |
| ----- | ------------ |
| LeNet |     2176080  |
| MLP   |     813056   |

---

### Observations

- The overall summary looks as follows

| Model | Training Time per Epoch | Inference Latency | Parameters Count | Flops Count |
| ----- | ----------------------- | ----------------- | ---------------- | ----------- |
| LeNet |        4.75 s           |        11 ms      |       44426      |   2176080   |
| MLP   |        2.80 s           |        24 ms      |       407050     |   813056    |

- Training time is higher for LeNet model than MLP model
    - LeNet is computationally more complex as depicted from higher Flops count. It involves performing a large number of multiplications and additions for each filter as they slide across the spatial dimensions of the input image
    - During backpropagation, gradients need to be computed for each filter and propagated across multiple spatial positions in the feature maps which significantly increases the computation compared to MLP
    - Convolutional networks often include additional layers (e.g., pooling, dropout) to improve generalization but at the cost of increased training time
    - Convolutions involve sparse sliding-window operations, which can be less efficient on certain hardware during training
    - Although LeNet has fewer parameters which reduces memory usage but often it requires more training iterations to converge because the model may need to learn more sophisticated spatial features

- LeNet is faster than MLP for inference
    - Inference latency is measured for each input image. 
    - Since LeNet has fewer parameters than MLP hence they require less memory to store weights
    - Convolutional layers reuse the same weights across the entire spatial dimension of the input (weight sharing), it is more efficient. 
    - Although LeNet has higher flops count, the convolutional layers process relatively small, localized regions of the input at a time, making operations more efficient. However, MLP involves dense matrix multiplication which takes time
    - Libraries like PyTorch, Tensoflow have highly optimized implementations for CNNs

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

