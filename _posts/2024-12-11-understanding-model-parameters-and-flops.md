---
layout: post
title: "Understanding Parameters and FLOPS Count of ML Model"
date: 2024-12-11
tags: ["Machine Learning"]
---

Parameter Count and Flops count are important variables to evaluate an ML Model

---

### Background

- A Deep Learning Model can have different types of Layers e.g.
    - Fully Connected Layer
    - Convolutional Layer
    - Pooling Layer
- Calculating the parameter count and flops for each layer is important to get the correct picture

---

### Example : LeNet

- LeNet Model that we created for our [Digit Recognizer App](https://gouherdanish.github.io/2024/12/09/digit-recognition.html) had 5 layers
    - 2 Convolutional Layers
    - 2 Fully Connected Layers
    - 1 Output Layers of 10 neurons
- Refer [LeNet Model](https://github.com/gouherdanish/mnist_classification/blob/main/model/lenet.py)

---

### Fully Connected Layer



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

---