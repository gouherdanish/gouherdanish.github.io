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

**Introduction**
- It is the most basic form of quantization
- Here, a floating-point value is scaled and rounded to an integer without a zero-point offset
- It is a special case of affine quantization where the zero point is always 0

**Formula**

_Quantization_

$$ q = round(x/s) $$

where,
- x = Original floating-point value
- s = Scale factor
- q = Quantized integer

_De-quantization_

$$ x' = q * s $$

where,
- x' = de-quantized value

_Quantization Loss_

$$ dx = |x' - x| $$

**Example**

_Quantization_

```
>>> x = torch.tensor([-10.0, -5.0, 0.0, 5.0, 10.0])     # PyTorch creates FP32 floats by default
>>> tmin, tmax = min(t), max(t)
>>> qmin, qmax = -128, 127                              # INT8 Range
>>> scale = (tmax - tmin) / (qmax - qmin)               # Min-Max Scaling
>>> q = torch.round(t/scale)                            # Quantization Formula
>>> q = torch.clamp(q,-128,127)                         # Clipping to INT8 range to avoid overflow
>>> q = q.to(torch.int8)                                # Changing data type explicitly
>>> q
tensor([-127,  -64,    0,   64,  127], dtype=torch.int8)
```

_De-quantization_

```
>>> x' = q.float() * scale
>>> x'
tensor([-10.3404,  -5.7357,   0.0000,   5.7357,  10.2596])
```

_Quantization Loss_

```
>>> loss = torch.abs(x' - x)
>>> loss
tensor([0.0404, 0.0357, 0.0000, 0.0357, 0.0404])
```

---
### Conclusion
- We introduced Quantization and explored various techniques

