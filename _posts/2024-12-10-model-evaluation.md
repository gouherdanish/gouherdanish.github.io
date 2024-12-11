---
layout: post
title: "Evaluating a ML Model"
date: 2024-12-10
tags: ["Machine Learning"]
---

Evaluating a Machine Learning model is essential in understanding the model complexity and eventually its usability

---

### Evaluating Training Time

- Let's see how we can use this function to find training time for the LeNet Model we created for our [Digit Recognition App](https://gouherdanish.github.io/2024/12/09/digit-recognition.html)

**Step 1 - Create function**
- We can use Python `time` module to create a function which can calculate the time elapsed during a function call
- Below is a possible implementation of this function

```
import time

class Utils:
    def timeit(func):
        def inner(*args,**kwargs):
            start = time.time()
            res = func(*args,**kwargs)
            end = time.time()
            print(f"Elapsed Time: {(end-start):.4f}s")
            return res
        return inner
```

- Then we can use the timeit function as a decorator above the train function

**Step 2 - Apply Function**
```
@Utils.timeit
def train(...):
    ...
```

**Step 3 - Evaluate**
- To evaluate the training time, we can run the batch training pipeline for the app for LeNet model for 10 epochs as follows

```
>>> python batch_training.py --model_name lenet --epochs 10
# OUTPUT
Epoch 01, Train acc=0.886, Val acc=0.948, Train loss=0.003, Val loss=0.001
Epoch 05, Train acc=0.986, Val acc=0.984, Train loss=0.000, Val loss=0.000
Epoch 10, Train acc=0.993, Val acc=0.983, Train loss=0.000, Val loss=0.000
Elapsed Time: 47.5396s <-- 
...
```

- Notice, training LeNet model took ~48 seconds for 10 epochs
- Similarly, we can evaluate the training time for MLP model as well

```
>>> python batch_training.py --model_name lenet --epochs 10
# OUTPUT
Epoch 01, Train acc=0.892, Val acc=0.941, Train loss=0.003, Val loss=0.002
Epoch 05, Train acc=0.977, Val acc=0.972, Train loss=0.001, Val loss=0.001
Epoch 10, Train acc=0.987, Val acc=0.970, Train loss=0.000, Val loss=0.001
Elapsed Time: 20.8068s <-- 
...
```
- We can run for multiple epochs for each model to arrive at average time per epoch

| Model | Num Epochs | Elapsed Time (s) |  Avg Time per Epoch (s) |
| ----- | ---------- | ---------------- | ----------------------- |
| LeNet |     10     |       47.54 s    |           4.75 s        |
| MLP   |     10     |       20.80 s    |           2.80 s        |

Note 
- MLP Model had one hidden layer of 512 neurons
- Refer [MLP Model](https://github.com/gouherdanish/mnist_classification/blob/main/model/mlp.py)

---
### Calculating Inference Latency

- Inference latency is a measure of the average time it takes to classify each example
- We can use the same utility function to calculate the inference latency as well

**Using LeNet Model**
```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/1a.png --model_name lenet
# OUTPUT
{'confidence': tensor([0.9999]), 'pred_label': tensor([1])}
Elapsed Time: 0.0111s
```

**Using MLP Model**
```
>>> python single_inference.py --img /Users/gouher/Documents/personal/codes/ml/ml_projects/mnist_classification/data/sample/1a.png --model_name mlp
# OUTPUT
{'confidence': tensor([0.6122]), 'pred_label': tensor([1])}
Elapsed Time: 0.0244s   <--
```


- We can run for multiple epochs for each model to arrive at average time per epoch

| Model |  Avg Time per Epoch (ms) |
| ----- | ------------------------ |
| LeNet |           11.1 ms        |
| MLP   |           24.4 ms        |
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

