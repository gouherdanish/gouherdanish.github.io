
**Evaluation**

- Next, we evaluate these two models on multiple criteria as highlighted below
    - Model Size
    - Number of Model Parameters
    - Floating Point Operations
    - Training Time
    - Inference Latency
    - Accuracy
    - Carbon Footprint
- These evaluation functions are implemented in the `evaluate.py` file inside the `eval` module
- This module is invoked while running the `batch_evaluate.py` file as follows which will load the model weights created from above and evaluate it based on the criteria outlined inside the `eval` module

```
>>> python batch_evaluate.py --model_name mlp
{'size': '9.8 MB', 'params': 814090, 'flops': 1626112, 'latency': '23.8 μs', 'accuracy': '96.7%', 'CO2eq': '271.6 μgCO2eq'}
```

---
### Model Comparison

- We have seen that for most practical purposes, a batch size of 64 can be considered since it provides a good balance between performance and accuracy 
- Let's compare the two models that we have created above using the evaluation metrics

**Architecture**

_MLP-1_
<img src="{{site.url}}/images/mnist/mlp-1024_1.png" alt="MLP-1">

_MLP-2_
<img src="{{site.url}}/images/mnist/mlp-1024_2.png" alt="MLP-2">

**Model Size**

- It refers to the size of the model on disk
- MLP-2 is larger in size due to the extra hidden layer

<img src="{{site.url}}/images/mnist/mlp-comp-g1.png">

**Model Parameters**

- It refers to the total number of trainable weights and biases in the model
- MLP-2 has more number of parameters since extra neurons add more weights and biases

<img src="{{site.url}}/images/mnist/mlp-comp-g2.png">

**Floating Point Operations**

- It represents the number of arithmetic operations (multiplications, additions, etc.) required to process one input sample through the model 
- MLP-2 has higher Flops count since the extra neurons add more computation overhead 

<img src="{{site.url}}/images/mnist/mlp-comp-g3.png">

**Training Time**

- It represents the time it takes to run one epoch for the given ML model
- MLP-2 takes longer to train due to higher number of parameters

<img src="{{site.url}}/images/mnist/mlp-comp-g5.png">

**Latency**

- It represents the time it takes to run one inference for the given ML model
- MLP-2 takes longer to do inference due to more number of parameters

<img src="{{site.url}}/images/mnist/mlp-comp-g6.png">

**Accuracy**

- It represents the number of correct predictions done per 100 test samples
- MLP-2 (97.2%) has very little and incremental improvement compared to MLP-1 (96.4%)

<img src="{{site.url}}/images/mnist/mlp-comp-g4.png">

**Carbon Footprint**

- It represents the equivalent amount of $CO_2$ emitted during one inference
- MLP-2 has higher carbon footprint since it has higher latency 

<img src="{{site.url}}/images/mnist/mlp-comp-g4.png">

---
### Observations

- Although bigger in size, MLP-2 adds very liitle to the Accuracy compated to MLP-1
- Due to the extra layer of neurons, the computational complexity increases which causes MLP-2 performance to deteriorate as evident from higher Training Time and Inference Latency 
- Due to its higher latency, MLP-2 has higher Carbon Footprint and therefore has worse impact on the environment

---
### Conclusion

- We explored multiple ways or criteria to evaluate a given ML model
- We concluded that having a bigger model is not always better. We must focus on balancing performance and accuracy to get better results