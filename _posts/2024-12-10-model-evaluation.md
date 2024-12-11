---
layout: post
title: "Evaluating a ML Model"
date: 2024-12-10
tags: ["Machine Learning"]
---

Evaluating a Machine Learning model is essential in understanding the model complexity and eventually its usability

---

### Evaluating Training Time

- Let's see how we can use this function to find training time for the LeNet Model we created for our [Digit Recognition App](https://gouherdanish.github.io/2024/12/09/digit-recognition.html)

**Create Timeit function**
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

- Then we can use the above created function as a decorator as follows

```
@Utils.timeit
def train(...):
    ...
```

**Evaluate LeNet Model**
- LeNet Model that we created for our App had 5 layers
    - 2 Convlutional Layers
    - 2 Fully Connected Layers
    - 1 Output Layers of 10 neurons
- Refer [MLP Model](https://github.com/gouherdanish/mnist_classification/blob/main/model/mlp.py)
- To evaluate the training time, we can run the batch training pipeline for the app for LeNet model for 10 epochs as follows

```
>>> python batch_training.py --model_name lenet --epochs 10
Epoch 01, Train acc=0.886, Val acc=0.948, Train loss=0.003, Val loss=0.001
Epoch 05, Train acc=0.986, Val acc=0.984, Train loss=0.000, Val loss=0.000
Epoch 10, Train acc=0.993, Val acc=0.983, Train loss=0.000, Val loss=0.000
Elapsed Time: 47.5396s <-- 
...
```

- Notice, training LeNet model took ~48 seconds for 10 epochs

**Evaluate MLP Model**
- MLP Model that we created for our App had 
    - 1 Fully connected layer of 512 neurons
    - 1 Output layer of 10 neurons
- Refer [MLP Model](https://github.com/gouherdanish/mnist_classification/blob/main/model/mlp.py)
- We can evaluate the training time for MLP model using the same utility function that we created above

```
>>> python batch_training.py --model_name lenet --epochs 10
Epoch 01, Train acc=0.892, Val acc=0.941, Train loss=0.003, Val loss=0.002
Epoch 05, Train acc=0.977, Val acc=0.972, Train loss=0.001, Val loss=0.001
Epoch 10, Train acc=0.987, Val acc=0.970, Train loss=0.000, Val loss=0.001
Elapsed Time: 20.8068s <-- 
...
```

**Comparison**
- We can run for multiple epochs for each model to arrive at average time per epoch

| Model | Num Epochs | Elapsed Time (s) |  Avg Time per Epoch (s) |
| ----- | ---------- | ---------------- | ----------------------- |
| LeNet |     10     |       47.54 s    |           4.75 s        |
| MLP   |     10     |       20.80 s    |           2.80 s        |


---
### Calculating Inference Latency

- Inference latency is a measure of the average time it takes to classify each example
- We can use the same utility function to calculate the inference latency as well

**Using LeNet Model**
```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/1a.png --model_name lenet
{'confidence': tensor([0.9999]), 'pred_label': tensor([1])}
Elapsed Time: 0.0111s
```

**Using MLP Model**
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

### Calculating Parameter Count

**Define Model**
- Let's say, we have an MLP model for our Digit Recognizer App
    - Input is an Mnist image of size 28*28 pixels
    - MLP model consisting of a single hidden layer of 512 neurons
    - Output is of size 10 corresponding to 10 digit classes
- We can define the model architecture in PyTorch as follows

```
class MLP(nn.Module):
    def __init__(self):
        super(MLP,self).__init__()
        self.model_name = 'mlp'
        self.fc1 = nn.Linear(28*28, 512)
        self.relu = nn.ReLU()
        self.output = nn.Linear(512, 10)
    
    def forward(self, x):
        x = x.view(-1,28*28)
        x = self.relu(self.fc1(x))
        return self.output(x)
```

**Observe Model**
- We can create a model instance and observe its layers

```
>>> model = MLP()
>>> print(model)
MLP(
  (fc1): Linear(in_features=784, out_features=512, bias=True)
  (relu): ReLU()
  (output): Linear(in_features=512, out_features=10, bias=True)
)
```

**Parameters**
- In PyTorch, there is a `numel()` method which gives the number of elements in a given tensor

```
>>> for p in model.parameters():
        print(p.size(), p.numel())
torch.Size([512, 784]) 401408   <-- Weights of fc1 layer (512 * 784 = 401408)
torch.Size([512]) 512           <-- Bias of fc1 layer
torch.Size([10, 512]) 5120      <-- Weights of output layer (512 * 10 = 5120)
torch.Size([10]) 10             <-- Bias of output layer
```

- We can use this method to create a function which calculates the total parameter count of a given model
```
def count_params(model):
    return sum([p.numel() for p in model.parameters() if p.requires_grad])
```

**MLP Model**

```
>>> from model.mlp import MLP
>>> model = MLP()
>>> count_params(model)
407050
```

**LeNet Model**

```
>>> from model.lenet import LeNet 
>>> model = LeNet()
>>> count_params(model)
44426
```

**Comparison**
```
| Model |  Parameter Count |
| ----- | ---------------- |
| LeNet |     44426        |
| MLP   |     407050       |
```


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

