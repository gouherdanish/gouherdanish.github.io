---
layout: post
title: "Understanding Parameters and FLOPS Count of ML Model"
date: 2024-12-11
tags: ["Machine Learning"]
---

Parameter Count and Flops count are important variables to evaluate an ML Model

---

### Definition

**Parameter Count**
- Refers to the total number of trainable weights and biases in the model.
- It is a measure of the model size
- It remains constant regardless of the input size because it depends solely on the architecture of the model.

**Flops Count**
- It represents the number of arithmetic operations (multiplications, additions, etc.) required to process one input sample through the model.
- FLOPs depend on the input size (e.g., height and width for images, sequence length for text).
- It is a measure of the computational complexity of the model for a given input.

---

### Background

- A Deep Learning Model can have different types of layers 
- As an example, the LeNet-5 Model that we created for our [Digit Recognizer App](https://gouherdanish.github.io/2024/12/09/digit-recognition.html) had 5 layers
    - 2 Convolutional Layers
    - 2 Fully Connected Hidden Layers
    - 1 Fully Connected Output Layers of 10 neurons

<img src="{{site.url}}/images/mnist/lenet.png">

- Calculating the parameter and flops count for each layer is important to get the correct picture

---

### Fully Connected Layer

<img src="{{site.url}}/images/mnist/fc_params.png">


<img src="{{site.url}}/images/mnist/fc_flops.png">

Note:
- For fully connected layer, FLOPs and parameter count are the same because the layer is purely linear.

---

### Convolutional Layer

<img src="{{site.url}}/images/mnist/conv_params.png">


<img src="{{site.url}}/images/mnist/conv_flops.png">

Note:
- For a Convolution Layer, FLOPs count is much larger than the parameter count because convolution operations are repeated across the spatial dimensions of the input.

---

### Implementation

- We have provided PyTorch implementation of the LeNet-5 model architecture [here](https://github.com/gouherdanish/mnist_classification/blob/main/model/lenet.py)
- Below we import that model and develop methods to calculate parameter and flops count

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

#### Implementing Parameter Count Function
- In PyTorch, there is a `numel()` method which gives the number of elements in a given tensor

```
>>> for p in model.parameters():
        print(p.size(), p.numel())
torch.Size([6, 1, 5, 5]) 150        <-- Weights of C1 Layer
torch.Size([6]) 6                   <-- Bias of C1 Layer
torch.Size([16, 6, 5, 5]) 2400
torch.Size([16]) 16
torch.Size([120, 256]) 30720        <-- Weights of FC1 Layer
torch.Size([120]) 120               <-- Bias of FC1 Layer
torch.Size([84, 120]) 10080
torch.Size([84]) 84
torch.Size([10, 84]) 840
torch.Size([10]) 10
```

- Based on this, we can create a function which iterates over each parameter tensor and sums up the element count

```
def count_params(model):
    return sum([p.numel() for p in model.parameters() if p.requires_grad])

>>> model = LeNet()
>>> count_params(model)
44426
```

---

#### Implementing Flops Count Function

- There is no standard function in PyTorch to give Flops Count right off the bat. So, we need to implement this function from scratch
- Note that, the method to calculate Flops differs based on the type of layer, so we need to take this into account
- We have provided the implementation [here](https://github.com/gouherdanish/mnist_classification/blob/main/eval/evaluate.py)

```
>>> from eval.evaluate import ModelEvaluator
>>> ModelEvaluator.count_flops(model,input_size=(1,28,28))
2176080
```

---

### Conclusion

- We explored two Model evaluation metrics namely Parameter Count and Flops Count which measure the model size and computational cost respectively
- We also provided hand calculation and implementation for these metrics in PyTorch
