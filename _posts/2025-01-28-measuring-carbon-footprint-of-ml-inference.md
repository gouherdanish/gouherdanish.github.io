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

#### Latency

- It refers to the average time it takes for running one inference
- Refer [this](https://gouherdanish.github.io/2024/12/13/evaluating-ml-models.html) article for implementation details

#### TDP

- TDP stands for Thermal Design Power which refers to the maximum amount of heat a component can generate under normal use
- In other words, it refers to the maximum amount of power the cooling system needs to dissipate
- A unit with lower TDP typically means that it consumes less power
- TDP is usually measured in Watts($W$) or KiloWatts($kW$)
- TDP of the component is determined by the manufacturing company

#### Energy Consumed

- It refers to the amount of Energy consumed in the form of electricity by all the components of the system
- It is usually measured in joules($J$) or kilowatthours($kWh$)
- Major ML computation workload is performed by 3 units specifically
    - Energy Consumed by CPU ($kWh$) = TDP of CPU ($kW$) * Latency ($h$)
    - Energy Consumed by RAM ($kWh$) = TDP of RAM ($kW$) * Latency ($h$)
    - Energy Consumed by GPU ($kWh$) = TDP of GPU ($kW$) * Latency ($h$)
    
- So, we can calculate the Total Energy consumed by summing up the energy consumed by all the units

```
Total Energy Consumed for 1 Inference ($kWh$) = 
    Energy Consumed by CPU 
    + Energy Consumed by RAM 
    + Energy Consumed by GPU
```

Note:
- The above method may not be very accurate in the sense that it assumes that the component is working at the maximum power for the whole the duration which may not be true
- A more accurate measure would be to calculate the energy consumed by different components in real time using system tools[2](https://www.isunshare.com/computer/how-to-measure-cpu-power-consumption.html) [3](https://link.springer.com/chapter/10.1007/978-3-031-11658-2_3#:~:text=In%20a%20very%20simple%20and,about%20the%20energy%20consumption%20instantly)[4](https://www.geeksforgeeks.org/how-to-measure-your-pcs-power-consumption/)

#### Carbon Emission

- It refers to the amount of $CO_2$ gas emitted as a result of doing one ML inference
- It is directly related to the Total Energy consumed by the system during the inference process

```
Carbon Emission for 1 Inference ($gCO_2$) = 
    Carbon Intensity of Country ($gCO_2 / kWh$) 
    * 
    Total Energy Consumed for 1 Inference ($kWh$)
```

Note:
- Carbon Intensity of each country in the world is available in a public database
[1](https://ourworldindata.org/grapher/carbon-intensity-electricity?tab=table#explore-the-data)

---

### Implementation

- Below we provide a basic implementation of calculating the carbon emission during the inference process

```
def compute_carbon_emission(runtime):
    """
    Calculates the carbon footprint of one inference in gCO2eq

    Args:
        runtime: time it takes to run one inference in hours
    """
    factor = 713        # For India, Units: gCO2eq/kWh, Refer [1](https://ourworldindata.org/grapher/carbon-intensity-electricity?tab=table#explore-the-data)
    ram_power = 3       # 3 Watts for 8 GB Refer [5](https://mlco2.github.io/codecarbon/methodology.html#)
    cpu_power = 13      # 13 Watts for MAC M1 [6](https://versus.com/en/apple-m1/cpu-tdp)
    gpu_power = 0       # Not using GPU acceleration
    energy_consumed = (ram_power + cpu_power + gpu_power) * runtime / 1000 # in kWh
    carbon_emission = factor * energy_consumed # in gCO2eq
    return carbon_emission
```

---
### References

Implementation and code can be found on Github
- [Digit Recognition App](https://github.com/gouherdanish/mnist_classification)

Other references
[1]. https://ourworldindata.org/grapher/carbon-intensity-electricity?tab=table#explore-the-data
[2]. https://www.isunshare.com/computer/how-to-measure-cpu-power-consumption.html 
[3]. https://link.springer.com/chapter/10.1007/978-3-031-11658-2_3#:~:text=In%20a%20very%20simple%20and,about%20the%20energy%20consumption%20instantly
[4]. https://www.geeksforgeeks.org/how-to-measure-your-pcs-power-consumption/
[5]. https://mlco2.github.io/codecarbon/methodology.html#
[6]. https://versus.com/en/apple-m1/cpu-tdp

---
### Conclusion
- We evaluated the efficiency of ML model based on following 
    - Training time and Inference latency
    - Parameter Count 
    - Floating Point Operations or Flops

