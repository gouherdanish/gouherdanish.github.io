---
layout: post
title: "Logistic Regression: Concepts Deep Dive and Implementation from Scratch"
date: 2024-08-22
tags: ["Machine Learning"]
---

Logistic Regression is a powerful yet simple and explainable ML algorithm. Understanding its intricacies is vital to any ML practitioner.

Here we implement Logistic regression from scratch, we take a very simple example but will go very in-depth, presenting with theory and hand calculation at each step. 

We will also verify our results it with standard implementations fr 

That will make things a lot easier

---
### Assumptions

- Binary Classification
- Two Predictor Variables

---
### Concepts

#### Linear Model

Each example is fed into a linear model characteized by weight `w` and bias `b`

$$ z = w_1 x_1 + w_2 x_2 + b $$

In this case, $ w_1,w_2,b \in {\mathbb{R}} $ i.e. these are scalar values

So, z will also be scalar $ z \in {\mathbb{R}} $

But, logistic regression requires the output as probabilities in the range (0,1)

---

#### Sigmoid Function

When binary classification is involved, we can use Sigmoid function to limit the prediction in the range (0,1). 
Since, its output range is limited to (0,1), the Sigmoid function is very much suitable to represent probabilities

Sigmoid function can be given by,

$$ \hat{y} = \sigma(z) = \frac {1}{1+e^{-z}} $$

where, 

$$ z \in {\mathbb{R}} $$

$$ \sigma(z) \in (0,1) $$

---
#### Cross Entropy Loss

- Cross entropy measures how well one probability distribution approximates another and is often used to quantify the difference between predicted and true distributions in machine learning tasks.

- Cross entropy for one example can be defined as,

$$ H(y,\hat{y}) = - \sum_{i=1}^n y_i \log \hat{y_i} $$

Here,
- $y$ denotes actual ground truth binary label which is either 0 or 1 
- $\hat{y}$ denotes predicted probability value which is a real value between 0 and 1 (represented by Sigmoid function)
- `n` represents the number of output classes 
- `i` represents $i^{th}$ class

For Binary Classification, $n = 2$

$$ H(y,\hat{y}) = - y_1 \log \hat{y_1} - y_2 \log \hat{y_2} $$

Also, since probabilities sum to 1,

$$ y_1 + y_2 = 1 \implies y_2 = 1 - y_1 $$

$$ \hat{y_1} + \hat{y_2} = 1 \implies \hat{y_2} = 1 - \hat{y_1}$$

we can write the Binary Cross Entropy loss for one example as,

$$ \ell = H \left( y,\hat{y} \right) = - y \log \hat{y} - (1-y) \log (1-\hat{y}) $$

Average Cross Entropy loss over all `m` examples can be given by,

$$ L = \frac{1}{m} \sum_{i=1}^m \ell_i = \frac{1}{m} \sum_{i=1}^m H \left( y,\hat{y} \right) $$ 

$$ \Rightarrow L = \frac{1}{m} \sum_{i=1}^m [- y_i \log \hat{y_i} - (1-y_i) \log (1-\hat{y_i})] $$

---

#### Gradient of Loss Function

From above, we have derived the Binary cross entropy loss for $i^{th}$ example,

$$ \ell_i = - \left( y_i \log(\hat{y_i}) - (1 - y_i) \log(1 - \hat{y_i}) \right) $$

We can calculate required partial derivatives as follows,

#### 1. Calculate $ \frac{\partial \ell_i}{\partial \hat{y_i}} $

$$ \frac{\partial \ell_i}{\partial \hat{y_i}} = -\frac{y_i}{\hat{y_i}} + \frac{1 - y_i}{1 - \hat{y_i}} $$

$$ \Rightarrow \frac{\partial \ell_i}{\partial \hat{y_i}} = \frac{-y_i(1-\hat{y_i})+\hat{y_i}(1-y_i)}{\hat{y_i}(1-\hat{y_i})} $$

$$ \Rightarrow \frac{\partial \ell_i}{\partial \hat{y_i}} = \frac{\hat{y_i}-y_i}{\hat{y_i}(1-\hat{y_i})} $$

#### 2. Calculate $ \frac{\partial \hat{y_i}}{\partial z_i} $

$$ \hat{y_i} = \sigma(z_i) = \frac {1}{1+e^{-z_i}} $$

$$ \Rightarrow \frac{\partial \hat{y_i}}{\partial z_i} = \frac{-1}{(1+e^{-z_i})^2} \left({-e^{-z_i}} \right) $$

$$ \Rightarrow \frac{\partial \hat{y_i}}{\partial z_i} = \left( \frac{1}{1+e^{-z_i}} \right) \left( \frac{e^{-z_i}}{1+e^{-z_i}} \right) $$

$$ \Rightarrow \frac{\partial \hat{y_i}}{\partial z_i} = \frac{1}{m} \hat{y_i}(1 - \hat{y_i}) $$

#### 3. Calculate $ \frac{\partial z_i}{\partial w_i} $

$$ z_i = w_ix_i + b_i $$

$$ \frac{\partial z_i}{\partial w_i} = x_i $$

Using product rule, the first derivative of loss function w.r.t to model params can be given by,

$$ \frac{\partial \ell_i}{\partial w_i} = \frac{\partial \ell_i}{\partial \hat{y_i}} \times \frac{\partial \hat{y_i}}{\partial z_i} \times \frac{\partial z_i}{\partial w_i} $$

Substituting,

$$ \frac{\partial \ell_i}{\partial w_i} = \frac{\hat{y_i}-y_i}{\hat{y_i}(1-\hat{y_i})} \times \hat{y_i}(1 - \hat{y_i}) \times x_i $$

$$ \Rightarrow \frac{\partial \ell_i}{\partial w_i} = (\hat{y_i} - y_i)x_i $$

Averaging over all the `m` examples,

$$ \frac{\partial L}{\partial w} = \frac{1}{m} \sum_{i=1}^m \frac{\partial \ell_i}{\partial w_i} $$ 

$$ \Rightarrow \frac{\partial L}{\partial w} = \frac{1}{m} \sum_{i=1}^m (\hat{y_i} - y_i)x_i $$

### Hand Calculations

Let's consider an example for a Binary Classification Problem

"_Gouher browses movies through OTT and his decision to watch a particular movie is based on its rating and release date._"

Let's take some historical data from Gouher's OTT watchlist history

| movie name    | rating | released date | watched | 
| ------------- | ------ | ------------- | ------- |
| kalki         |   6.2  |     2024      |    1    |
| tumbbad       |   7.8  |     2018      |    1    | 
| indiana jones |   8.1  |     1990      |    0    | 
| Tiger 3       |   4.5  |     2023      |    0    | 

Let's see how we can formulate this problem from the ground up

#### Step 1 - Data Preparation

From the problem statement, 

- Features (Predictor Variables)
    - rating \rightarrow $x_1$
    - released date \rightarrow $x_2$
- Label (Target Variable)
    - watched \rightarrow $y$

|   i   | $x_1$ | $x_2$ | $y$ |
| ----- | ----- | ----- | --- |
|   1   |  6.2  | 2024  |  1  |
|   2   |  7.8  | 2018  |  1  |
|   3   |  8.1  | 1990  |  0  |
|   4   |  4.5  | 2023  |  0  |

#### Step 2 - Linear Model

if `w` and `b` are weights

$$ z = w_1x_1 + w_2x_2 + b $$

Initially, $ w_1 = w_2 = b = 0 $

$$ \Rightarrow z = 0 $$

#### Step 3 - Sigmoid Function

$$ \hat{y} = \frac{1}{1+e^{-z}} $$

$$ \Rightarrow  \hat{y} = 0.5 \; since \; z=0 $$ 

|   i   | $x_1$ | $x_2$ | $y$ | $\hat{y}$ |
| ----- | ----- | ----- | --- | --------- |
|   1   |  6.2  | 2024  |  1  |    0.5    |
|   2   |  7.8  | 2018  |  1  |    0.5    |
|   3   |  8.1  | 1990  |  0  |    0.5    |
|   4   |  4.5  | 2023  |  0  |    0.5    |

#### Step 4 - Residuals (Prediction Error)

$$ residual = \hat{y} - y $$

|  $i$  | $x_1$ | $x_2$ | $y$ | $\hat{y}$ | $\hat{y} - y$ |
| ----- | ----- | ----- | --- | --------- | ------------- |
|   1   |  6.2  | 2024  |  1  |    0.5    |     -0.5      |
|   2   |  7.8  | 2018  |  1  |    0.5    |     -0.5      |
|   3   |  8.1  | 1990  |  0  |    0.5    |      0.5      |
|   4   |  4.5  | 2023  |  0  |    0.5    |      0.5      |

#### Step 4 - Cross Entropy Loss

$$ \ell = - y \log \hat{y} - (1-y) \log (1-\hat{y}) $$

For the 1st example, 

$$ \ell^{(1)} = - 1 \log (0.5) - (1-1) \log (1-0.5) $$

$$ \ell^{(1)} = 0.693 $$

|   i   | $x_1$ | $x_2$ | $y$ | $\hat{y}$ | $\hat{y} - y$ |  $\ell$  |
| ----- | ----- | ----- | --- | --------- | ------------- | -------- |
|   1   |  6.2  | 2024  |  1  |    0.5    |     -0.5      |   0.693  |
|   2   |  7.8  | 2018  |  1  |    0.5    |     -0.5      |   0.693  |
|   3   |  8.1  | 1990  |  0  |    0.5    |      0.5      |   0.693  |
|   4   |  4.5  | 2023  |  0  |    0.5    |      0.5      |   0.693  |

Averaging over all $m=4$ examples,

$$ L = \frac{1}{m} \sum_{i=1}^m \ell_i = \frac{0.693*4}{4} = 0.693 $$

#### Step 5 - Gradient Computation

$$ \frac{\partial L}{\partial b} = \frac{1}{m} \sum_{i=1}^m (\hat{y_i} - y_i)(1) $$

$$ \frac{\partial L}{\partial w_1} = \frac{1}{m} \sum_{i=1}^m (\hat{y_i} - y_i)x_{1}^{(i)} $$

$$ \frac{\partial L}{\partial w_2} = \frac{1}{m} \sum_{i=1}^m (\hat{y_i} - y_i)x_{2}^{(i)} $$

For the 1st example, 

$$ \left( \frac{\partial L}{\partial b} \right)^{(1)} = (\hat{y^{(1)}} - y^{(1)})(1) = -0.5 $$

$$ \frac{\partial L}{\partial w_1}^{(1)} = (\hat{y^{(1)}} - y^{(1)})(x_{1}^{(1)}) = (-0.5)(6.2) = -3.1 $$

$$ \frac{\partial L}{\partial w_2}^{(1)} = (\hat{y^{(1)}} - y^{(1)})(x_{2}^{(1)}) = (-0.5)(2024) = -1012  $$

|   i   | $x_1$ | $x_2$ | $y$ | $\hat{y}$ | $\hat{y} - y$ | $\ell$ | $\frac{\partial L}{\partial b}$ | $\frac{\partial L}{\partial w_1}$ | $\frac{\partial L}{\partial w_2}$ |
| ----- | ----- | ----- | --- | --------- | -------- | -------- | ---------- | ----------- | --------- |
|   1   |  6.2  | 2024  |  1  |    0.5    |   -0.5   |   0.693  |    -0.5    |    -3.1     |   -1012   |
|   2   |  7.8  | 2018  |  1  |    0.5    |   -0.5   |   0.693  |    -0.5    |    -3.9     |   -1009   |
|   3   |  8.1  | 1990  |  0  |    0.5    |    0.5   |   0.693  |     0.5    |    4.05     |    995    |
|   4   |  4.5  | 2023  |  0  |    0.5    |    0.5   |   0.693  |     0.5    |    2.25     |   1011.5  |

Averaging over all $m=4$ examples,

$$\frac{\partial L}{\partial b} = \frac{1}{4} \left( -0.5 - 0.5 + 0.5 + 0.5 \right) = 0 $$

$$\frac{\partial L}{\partial w_1} = \frac{1}{4} \left( -3.1 - 3.9 + 4.05 + 2.25 \right) = \frac{-0.7}{4} = -0.175 $$

$$\frac{\partial L}{\partial w_2} = \frac{1}{4} \left( -1012 - 1009 + 995 + 1011.5 \right) = \frac{-14.5}{4} = -3.625 $$

