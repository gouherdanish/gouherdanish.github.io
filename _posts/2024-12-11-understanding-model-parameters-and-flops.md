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
- It remains constant regardless of the input size because it depends solely on the architecture of the model.

**Flops Count**
- It represents the number of arithmetic operations (multiplications, additions, etc.) required to process one input sample through the model.
- FLOPs depend on the input size (e.g., height and width for images, sequence length for text).
- Measures the computational complexity of the model for a given input.

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

```
def count_flops(model,input_size):
    total_flops = 0
    prev_size = input_size
    for layer in model.children():
        if isinstance(layer,nn.Conv2d):
            # Convolutional FLOPs
            C_in, H_in, W_in = prev_size
            C_out = layer.out_channels
            kernel_h, kernel_w = layer.kernel_size
            stride_h, stride_w = layer.stride
            padding_h, padding_w = layer.padding
            H_out = (H_in - kernel_h + 2 * padding_h) // stride_h + 1
            W_out = (W_in - kernel_w + 2 * padding_w) // stride_w + 1
            
            flops_conv = H_out * W_out * C_out * kernel_h * kernel_w * C_in
            total_flops += flops_conv
            prev_size = (C_out, H_out, W_out)
        elif isinstance(layer, nn.MaxPool2d) or isinstance(layer, nn.AvgPool2d):
            # Pooling FLOPs
            C_in, H_in, W_in = prev_size
            kernel_h, kernel_w = layer.kernel_size
            stride_h, stride_w = layer.stride
            H_out = (H_in - kernel_h) // stride_h + 1
            W_out = (W_in - kernel_w) // stride_w + 1
            flops_pool = H_out * W_out * C_in * (kernel_h * kernel_w)
            total_flops += flops_pool
            prev_size = (C_in, H_out, W_out)
        elif isinstance(layer,nn.Linear):
            # Fully connected FLOPs
            flops_mul = layer.in_features * layer.out_features
            flops_add = layer.out_features
            total_flops += flops_mul + flops_add
    return total_flops
```

- Using the function defined above let's calculate the Total Flops Count for our model

```
>>> model = LeNet()
>>> count_flops(model,input_size=(1,28,28))

```