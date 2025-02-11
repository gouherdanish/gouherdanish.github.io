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

_De-quantization Formula_

$$ x' = q * s $$

where,
- x' = de-quantized value

_Quantization Loss_

$$ dx = |x' - x| $$

**Example**



---
### Conclusion
- We introduced Quantization and explored various techniques

