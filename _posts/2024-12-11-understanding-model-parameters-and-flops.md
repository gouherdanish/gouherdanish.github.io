---
layout: post
title: "Understanding Parameters and FLOPS Count of ML Model"
date: 2024-12-11
tags: ["Machine Learning"]
---

Parameter Count and Flops count are important variables to evaluate an ML Model

---

### Background

- A Deep Learning Model can have different types of layers 
- As an example, the LeNet-5 Model that we created for our [Digit Recognizer App](https://gouherdanish.github.io/2024/12/09/digit-recognition.html) had 5 layers
    - 2 Convolutional Layers
    - 2 Fully Connected Hidden Layers
    - 1 Fully Connected Output Layers of 10 neurons

<img src="{{site.url}}/images/mnist/lenet.png">

- Calculating the parameter count and flops for each layer is important to get the correct picture

---

### Fully Connected Layer

<img src="{{site.url}}/images/mnist/fc_params.png">


<img src="{{site.url}}/images/mnist/fc_flops.png">

---

### Convolutional Layer

<img src="{{site.url}}/images/mnist/conv_params.png">


<img src="{{site.url}}/images/mnist/conv_flops.png">

---

### Implementation

- We have provided PyTorch implementation of the LeNet-5 model architecture in our [App](https://github.com/gouherdanish/mnist_classification/blob/main/model/lenet.py)
- Below we import that model 

```
>>> from model.lenet import LeNet
>>> model = LeNet()
>>> print(model)
LeNet(
  (conv1): Conv2d(1, 6, kernel_size=(5, 5), stride=(1, 1))
  (conv2): Conv2d(6, 16, kernel_size=(5, 5), stride=(1, 1))
  (fc1): Linear(in_features=256, out_features=120, bias=True)
  (fc2): Linear(in_features=120, out_features=84, bias=True)
  (fc3): Linear(in_features=84, out_features=10, bias=True)
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