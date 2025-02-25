---
layout: post
title: "Introduction to Quantization"
date: 2025-02-10
tags: ["Machine Learning"]
---

Quantization is the art of trading precision for efficiency—where every bit saved fuels faster, leaner, and smarter AI 

---

### Definition
- Quantization is a technique used in deep learning which converts high-precision floating-point values into lower-precision formats while trying to preserve model accuracy.

**High Precision Formats**
- Double Precision (FP64)
- Single Precision (FP32)

**Low Precision Formats**
- Half Precision (FP16)
- 8-bit Integer (INT8)
- 16-bit Integer (INT16)

---
### Simple Scaling Quantization

#### Introduction
- It is the most basic form of quantization
- Here, a floating-point value is scaled and rounded to an integer without a zero-point offset
- It is a special case of affine quantization where the zero point is always 0
- It is good for symmetric data

#### Formula

_Quantization_

$$ q = round(t/scale) $$

where,
- t = Original FP32 tensor
- scale = Scale factor
- q = Quantized integer

_De-quantization_

$$ x' = q * scale $$

where,
- x' = de-quantized value

_Quantization Error_

$$ Absolute error = |x' - x| $$

$$ Absolute Percentage error = 100 \times \frac{|x' - x|}{x} $$

#### Implementation

_Quantization Function_

- We first define a function which will take a FP32 float tensor and return a INT8 quantized tensor
```
def quantize(t: torch.Tensor) -> torch.Tensor:
    tmin, tmax = min(t), max(t)
    qmin, qmax = -128, 127                              # INT8 Range
    scale = (tmax - tmin) / (qmax - qmin)               # Min-Max Scaling
    q = torch.round(t/scale)                            # Quantization Formula
    q = torch.clamp(q,-128,127)                         # Clipping to INT8 range to avoid overflow
    q = q.to(torch.int8)                                # Changing data type explicitly
    return q
```

_De-quantization Function_

- We define a inverse function which takes the quantized tensor and returns us the dequantized value

```
def dequantize(t: torch.Tensor) -> torch.Tensor:
    return t.float() * scale
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
- Similarly we can apply the inverse function to get the dequantized tensor
```
>>> dq = dequantize(q)
>>> dq
tensor([-9.9608, -5.0196,  0.0000,  5.0196,  9.9608])
```

**Step 4**
- Finally, we can calculcate the error due to quantization as follows

```
# Absolute Error
>>> ae = torch.abs(dq - t)
>>> ae
tensor([0.0392, 0.0196, 0.0000, 0.0196, 0.0392])

# Absolute Percentage Error
>>> ape = torch.where(t == 0, 0, 100 * torch.abs(dq - t)/t)
>>> ape
tensor([-0.3922, -0.3922,  0.0000,  0.3922,  0.3922])
```

_Case 2 - Asymmetric Data_


---
### Conclusion

- We introduced Quantization and explored various techniques

