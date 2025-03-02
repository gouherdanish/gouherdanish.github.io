---
layout: post
title: "Introduction to Quantization"
date: 2025-02-10
tags: ["Machine Learning"]
---

Quantization is the art of trading precision for efficiency—where every bit saved fuels faster, leaner, and smarter AI 

---

### Definition
- Quantization is a technique used in deep learning which converts high-precision floating-point values into lower-precision formats such as integers

- Quantization helps in reducing memory usage and speeds up computation while trying to preserve model accuracy

**High Precision Formats**
- Double Precision (FP64)
- Single Precision (FP32)

**Low Precision Formats**
- Half Precision (FP16)
- 8-bit Integer (INT8)
- 16-bit Integer (INT16)

---
### Symmetric Quantization

#### Introduction
- It is the most basic form of quantization
- The scale is computed based on the absolute maximum value of the tensor.
- The zero point is always 0
- Used when data is roughly symmetric around zero.

#### Formula

_Quantization_

$$ q = round \left(\frac {t}{s} \right) $$

where,
- t = Original FP32 tensor
- s = Scale factor
- q = Quantized integer

_De-quantization_

$$ x' = q * s $$

where,
- x' = de-quantized value

_Quantization Error_

$$ Absolute error = |x' - x| $$

$$ Absolute Percentage error = 100 \times \frac{|x' - x|}{x} $$

#### Implementation

_Scale Factor_

- First, we define a function to calculate the scale factor for a given tensor

```
def calculate_scale(t, qmin=-127, qmax=127):
    tmax = t.abs().max()
    scale = tmax / qmax
    return scale
```

Note: 
- The function uses INT8 range by default. We can provide any range we want

_Quantization Function_

- We first define a function which will take a FP32 float tensor and return a INT8 quantized tensor
```
def quantize(t, scale):
    q = torch.round(t/scale)
    q = q.to(torch.int8)
    return q
```

_De-quantization Function_

- We define a inverse function which takes the quantized tensor and returns us the dequantized value

```
def dequantize(t, scale):
    return t * scale
```

#### Hand Calculation

_Case 1 - For data symmetric around 0_

**Step 1 - Example Tensor**

```
>>> t = torch.tensor([-12., -5., 0., 5., 10.])     # PyTorch creates FP32 tensors by default
>>> t.dtype
torch.float32
```

**Step 2 - Calculate Scale**

```
>>> scale = calculate_scale(t, qmin=0, qmax=127)
>>> scale
tensor(0.7874)
```

**Step 3 - Quantization**

```
>>> q = quantize(t,scale)
>>> q
tensor([-127,  -53,    0,   53,  106], dtype=torch.int8)
```

**Step 4 - Dequantization**

```
>>> t_dq = dequantize(q,scale)
>>> t_dq
tensor([-12.0000,  -5.0079,   0.0000,   5.0079,  10.0157])
```

**Step 5 - Error**

```
# Absolute Error
>>> ae = torch.abs(t_dq - t)
>>> ae
tensor([0.0000, 0.0079, 0.0000, 0.0079, 0.0157])

# Absolute Percentage Error
>>> ape = torch.where(t == 0, 0, 100 * torch.abs(t_dq - t)/t)
>>> ape
tensor([-0.0000, -0.1575,  0.0000,  0.1575,  0.1575])
```

Note 
- When the input data is symmetric around 0, the symmetric quantization produces output data in symmetrically quantized range
- Also, the percentage error is very minimal in this case (~0.16%)

---
_Case 2 - For asymmetric data (non-skewed)_

**Step 1 - Example Tensor**

```
>>> t = torch.tensor([0.,12.,30.,70.,90.,100.])
>>> t.dtype
torch.float32
```

**Step 2 - Calculate Scale**

```
>>> scale = calculate_scale(t, qmin=0, qmax=127)
>>> scale
tensor(0.7874)
```

**Step 3 - Quantization**

```
>>> q = quantize(t,scale)
>>> q
tensor([  0,  15,  38,  89, 114, 127], dtype=torch.int8)
```

**Step 4 - Dequantization**

```
>>> t_dq = dequantize(q,scale)
>>> t_dq
tensor([  0.0000,  11.8110,  29.9213,  70.0787,  89.7638, 100.0000])
```

**Step 5 - Error**

```
# Absolute Error
>>> ae = torch.abs(t_dq - t)
>>> ae
tensor([0.0000, 0.1890, 0.0787, 0.0787, 0.2362, 0.0000])

# Absolute Percentage Error
>>> ape = torch.where(t == 0, 0, 100 * torch.abs(t_dq - t)/t)
>>> ape
tensor([0.0000, 1.5748, 0.2625, 0.1125, 0.2625, 0.0000])
```

Note:
- When the input data is all positive and thus asymmetric around 0, the output of symmetric quantization is asymmetric
- However, since the data is not skewed, we get minimal precision loss (1.6%)

---

_Case 3 - For asymmetric data (skewed)_

**Step 1 - Example Tensor**

```
>>> t = torch.tensor([-2.,1.,2.,10.,50.,200.])
>>> t.dtype
torch.float32
```

**Step 2 - Calculate Scale**

```
>>> scale = calculate_scale(t, qmin=0, qmax=127)
>>> scale
tensor(1.5748)
```

**Step 3 - Quantization**

```
>>> q = quantize(t,scale)
>>> q
tensor([ -1,   1,   1,   6,  32, 127], dtype=torch.int8)
```

Note:
- The scale is highly influenced by large positive value, which causes poor representation for small values 
- 1 and 2 in input data are both being represented as 1 in the quantized domain

**Step 4 - Dequantization**

```
>>> t_dq = dequantize(q,scale)
>>> t_dq
tensor([ -1.5748,   1.5748,   1.5748,   9.4488,  50.3937, 200.0000])
```

**Step 5 - Error**

```
# Absolute Error
>>> ae = torch.abs(t_dq - t)
>>> ae
tensor([0.4252, 0.5748, 0.4252, 0.5512, 0.3937, 0.0000])

# Absolute Percentage Error
>>> ape = torch.where(t == 0, 0, 100 * torch.abs(t_dq - t)/t)
>>> ape
tensor([21.2598, 57.4803, 21.2598,  5.5118,  0.7874,  0.0000])
```

Note:
- Since the scale is very high, the lower values possess very high precision loss (~58%)

---
### Conclusion

- We introduced Symmetric Quantization and explored various cases
- We understood that Symmetric Quantization can work for non-skewed data

