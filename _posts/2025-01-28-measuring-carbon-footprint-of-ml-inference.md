---
layout: post
title: "Calculating Carbon Footprint of ML Inference"
date: 2025-01-28
tags: ["Machine Learning"]
---


```
Nothing exists until it is measured.
- Niels Bohr
```

---

### Background

### Important Terms


```
Energy Consumed by CPU = TDP (kW) * Latency (h)
Energy Consumed by RAM = TDP (kW) * Latency (h)
Energy Consumed by GPU = TDP (kW) * Latency (h)

Total Energy Consumed for 1 Inference (kWh) = Energy Consumed by (CPU + RAM + GPU)
```

- 
```
CO_2 Emission = Carbon Intensity of Country ($gCO_2$ per kWh) * Energy Consumed for 1 Inference (kWh)
```

Note:
- Carbon Intensity of each country in the world is available in a public database
https://ourworldindata.org/grapher/carbon-intensity-electricity?tab=table#explore-the-data


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

