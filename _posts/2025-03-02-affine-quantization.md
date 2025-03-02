---
layout: post
title: "Affine Quantization"
date: 2025-03-02
tags: ["Machine Learning"]
---

Quantization is the art of trading precision for efficiency—where every bit saved fuels faster, leaner, and smarter AI 

---

### Introduction
- It is also known as Asymmetric quantization
- The scale is computed using the min and max values of the tensor.
- The zero point is nonzero
- Useful when data has a skewed distribution.

---

### Formula

_Quantization_

$$ q = round \left(\frac {x}{s} \right) + z $$

where,
- x = Original FP32 tensor
- s = Scale factor
- z = Zero Point
- q = Quantized integer

_De-quantization_

$$ x' = (q - z) * s $$

where,
- x' = de-quantized value

_Quantization Error_

$$ Absolute error = |x' - x| $$

$$ Absolute Percentage error = 100 \times \frac{|x' - x|}{x} $$

### Derivation

Affine quantization maps floating-point values to integer values using a linear transformation

$$ Q(x) = round \left(\frac {x}{s} \right) + z $$

from the range $[x_{min}, x_{max}]$ to $[q_{min}, q_{max}]$

$$ q_{min} = Q(x_{min}) = round \left(\frac {x_{min}}{s} \right) + z  --(1) $$

$$ q_{max} = Q(x_{max}) = round \left(\frac {x_{max}}{s} \right) + z  --(2) $$

Ignoring rounding, we can solve for s and z,

$$ q_{max} - q_{min} = \left(\frac {x_{max}}{s} + z \right) - \left(\frac {x_{min}}{s} + z \right) $$

$$ q_{max} - q_{min} = \frac {x_{max} - x_{min}}{s} $$

$$ s =  \frac{x_{max} - x_{min}}{q_{max} - q_{min}} $$

This means, the scale can be calculated as per Min-Max Scaling Scheme

Next, from equation (1),

$$ z = q_{min} - round \left(\frac {x_{min}}{s} \right) $$

### Important Terms

_Scale_

- Scale represents the step size which is used to map real numbers into integers
    - A small scale means finer precision (small steps between numbers).
    - A large scale means fewer unique values are represented (larger steps).

- From above, we have derived the formula for scale as follows,

$$ s =  \frac{x_{max} - x_{min}}{q_{max} - q_{min}} $$

_Zero Point_

- Zero Point is the integer value in Quantized domain that corresponds to zero value in the input floating point data.

<img src="{{site.url}}/images/quantize/zero_point.png" alt="Zero Point">

#### Implementation

_Define Scale and Zero Point Function_

- Below, we define a function to calculate the scale factor and zero point for a given tensor

```
def calculate_scale(t, qmin=0, qmax=255):
    tmin, tmax = t.min(), t.max()
    scale = (tmax - tmin) / (qmax - qmin)
    zero_point = torch.round(qmin - tmin/s)
    return scale,zero_point
```

_Define Quantization Function_

- We first define a function which will take a FP32 float tensor and return a INT8 quantized tensor

```
def quantize(t, scale, zero_point):
    q = torch.round(t/scale + zero_point)      
    q = q.to(torch.uint8)
    return q
```

_Define Dequantization Function_

- We define a inverse function which takes the quantized tensor and returns us the dequantized value

```
def dequantize(q,scale,zero_point):
    return (q - zero_point) * scale
```

#### Hand Calculation

_Case 1 - For data symmetric around 0_

**Step 1**
- Here, we take an simple example tensor which is symmetric around zero

```
>>> t = torch.tensor([-10.0, -5.0, 0.0, 5.0, 10.0])     # PyTorch creates FP32 tensors by default
```

**Step 2**
- Next we use above created functions to demonstrate the Simple Scaling based Quantization 

```
>>> q = quantize(t)
>>> q
tensor([-127,  -64,    0,   64,  127], dtype=torch.int8)
```

**Step 3**

```
>>> t_dq = dequantize(q)
>>> t_dq
tensor([-9.9608, -5.0196,  0.0000,  5.0196,  9.9608])
```

**Step 4**

```
# Absolute Error
>>> ae = torch.abs(t_dq - t)
>>> ae
tensor([0.0392, 0.0196, 0.0000, 0.0196, 0.0392])

# Absolute Percentage Error
>>> ape = torch.where(t == 0, 0, 100 * torch.abs(t_dq - t)/t)
>>> ape
tensor([-0.3922, -0.3922,  0.0000,  0.3922,  0.3922])
```


