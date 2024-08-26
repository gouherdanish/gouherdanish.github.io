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
## Concepts

### Linear Model

Each example is fed into a linear model characteized by weight `w` and bias `b`

$$ z = w_1 x_1 + w_2 x_2 + b $$

In this case, $ w_1,w_2,b \in {\mathbb{R}} $ i.e. these are scalar values

So, z will also be scalar $ z \in {\mathbb{R}} $

But, logistic regression requires the output as probabilities in the range (0,1)

---

### Sigmoid Function

When binary classification is involved, we can use Sigmoid function to limit the prediction in the range (0,1). 
Since, its output range is limited to (0,1), the Sigmoid function is very much suitable to represent probabilities

Sigmoid function can be given by,

$$ \hat{y} = \sigma(z) = \frac {1}{1+e^{-z}} $$

where, 

$$ z \in {\mathbb{R}} $$

$$ \sigma(z) \in (0,1) $$

---
### Cross Entropy Loss

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

### Gradient of Loss Function

From above, we have derived the Binary cross entropy loss for $i^{th}$ example,

$$ \ell_i = - y_i \log\hat{y_i} - (1 - y_i) \log(1 - \hat{y_i}) $$

We can calculate required partial derivatives as follows,

#### 1. Calculate $ \frac{\partial \ell_i}{\partial \hat{y_i}} $

$$ \frac{\partial \ell_i}{\partial \hat{y_i}} = -\frac{y_i}{\hat{y_i}} + \frac{1 - y_i}{1 - \hat{y_i}} $$

$$ \Rightarrow \frac{\partial \ell_i}{\partial \hat{y_i}} = \frac{-y_i(1-\hat{y_i})+\hat{y_i}(1-y_i)}{\hat{y_i}(1-\hat{y_i})} $$

$$ \Rightarrow \frac{\partial \ell_i}{\partial \hat{y_i}} = \frac{\hat{y_i}-y_i}{\hat{y_i}(1-\hat{y_i})} $$

#### 2. Calculate $ \frac{\partial \hat{y_i}}{\partial z_i} $

$$ \hat{y_i} = \sigma(z_i) = \frac {1}{1+e^{-z_i}} $$

$$ \Rightarrow \frac{\partial \hat{y_i}}{\partial z_i} = \frac{-1}{(1+e^{-z_i})^2} \left({-e^{-z_i}} \right) $$

$$ \Rightarrow \frac{\partial \hat{y_i}}{\partial z_i} = \left( \frac{1}{1+e^{-z_i}} \right) \left( \frac{e^{-z_i}}{1+e^{-z_i}} \right) $$

$$ \Rightarrow \frac{\partial \hat{y_i}}{\partial z_i} = \hat{y_i}(1 - \hat{y_i}) $$

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

## Hand Calculations

### Problem

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

### Data Preparation

From the problem statement, 

- Features (Predictor Variables)
    - rating $\rightarrow x_1$
    - released date $\rightarrow x_2$
- Label (Target Variable)
    - watched $\rightarrow y$

|   i   | $x_1$ | $x_2$ | $y$ |
| ----- | ----- | ----- | --- |
|   1   |  6.2  | 2024  |  1  |
|   2   |  7.8  | 2018  |  1  |
|   3   |  8.1  | 1990  |  0  |
|   4   |  4.5  | 2023  |  0  |

### Model Training - Epoch 1 

#### Step 1 - Linear Model

if `w` and `b` are weights

$$ z = w_1x_1 + w_2x_2 + b $$

Initially, $ w_1 = w_2 = b = 0 $

$$ z = \left( \begin{array}{cc} 1 & 6.2 & 2024 \\ 1 & 7.8 & 2018 \\ 1 & 8.1 & 1990 \\ 1 & 4.5 & 2023 \end{array} \right) \left( \begin{array}{cc} 0 \\ 0 \\ 0 \end{array} \right) $$

$$ \Rightarrow z = \left( \begin{array}{cc} 0 \\ 0 \\ 0 \\ 0 \end{array} \right) $$

#### Step 2 - Sigmoid Function

$$ \hat{y} = \frac{1}{1+e^{-z}} $$

$$ \Rightarrow \hat{y} = 0.5 \; since \; z=0 $$ 

$$ \Rightarrow \hat{y} = \left( \begin{array}{cc} 0.5 \\ 0.5 \\ 0.5 \\ 0.5 \end{array} \right) $$

|   i   | $x_1$ | $x_2$ | $y$ | $\hat{y}$ |
| ----- | ----- | ----- | --- | --------- |
|   1   |  6.2  | 2024  |  1  |    0.5    |
|   2   |  7.8  | 2018  |  1  |    0.5    |
|   3   |  8.1  | 1990  |  0  |    0.5    |
|   4   |  4.5  | 2023  |  0  |    0.5    |

#### Step 3 - Residuals (Prediction Error)

$$ residual = \hat{y} - y $$

$$ \Rightarrow \hat{y} - y = \left( \begin{array}{cc} -0.5 \\ -0.5 \\ 0.5 \\ 0.5 \end{array} \right) $$

|  $i$  | $x_1$ | $x_2$ | $y$ | $\hat{y}$ | $\hat{y} - y$ |
| ----- | ----- | ----- | --- | --------- | ------------- |
|   1   |  6.2  | 2024  |  1  |    0.5    |     -0.5      |
|   2   |  7.8  | 2018  |  1  |    0.5    |     -0.5      |
|   3   |  8.1  | 1990  |  0  |    0.5    |      0.5      |
|   4   |  4.5  | 2023  |  0  |    0.5    |      0.5      |

#### Step 4 - Cross Entropy Loss

$$ \ell = - y \log \hat{y} - (1-y) \log (1-\hat{y}) $$

$$ \Rightarrow \ell = -\left( \begin{array}{cc} 1 \\ 1 \\ 0 \\ 0 \end{array} \right) \log \left( \begin{array}{cc} 0.5 \\ 0.5 \\ 0.5 \\ 0.5 \end{array} \right) -\left( \begin{array}{cc} 0 \\ 0 \\ 1 \\ 1 \end{array} \right) \log \left( \begin{array}{cc} 0.5 \\ 0.5 \\ 0.5 \\ 0.5 \end{array} \right) $$

$$ \Rightarrow \ell = \left( \begin{array}{cc} 0.693 \\ 0.693 \\ 0.693 \\ 0.693 \end{array} \right) $$

|   i   | $x_1$ | $x_2$ | $y$ | $\hat{y}$ | $\hat{y} - y$ |  $\ell$  |
| ----- | ----- | ----- | --- | --------- | ------------- | -------- |
|   1   |  6.2  | 2024  |  1  |    0.5    |     -0.5      |   0.693  |
|   2   |  7.8  | 2018  |  1  |    0.5    |     -0.5      |   0.693  |
|   3   |  8.1  | 1990  |  0  |    0.5    |      0.5      |   0.693  |
|   4   |  4.5  | 2023  |  0  |    0.5    |      0.5      |   0.693  |

Averaging over all $m=4$ examples,

$$ L = \frac{1}{m} \sum_{i=1}^m \ell_i = \frac{0.693*4}{4} = 0.693 $$

#### Step 5 - Gradient Computation

$$ \frac{\partial \ell}{\partial w} = (\hat{y} - y)x $$

$$ \Rightarrow \frac{\partial \ell}{\partial w} = -\left( \begin{array}{cc} -0.5 \\ -0.5 \\ 0.5 \\ 0.5 \end{array} \right) \left( \begin{array}{cc} 1 & 6.2 & 2024 \\ 1 & 7.8 & 2018 \\ 1 & 8.1 & 1990 \\ 1 & 4.5 & 2023 \end{array} \right) $$

$$ \Rightarrow \frac{\partial \ell}{\partial w} = \left( \begin{array}{cc} -0.5 & -3.1 & -1012 \\ -0.5 & -3.9 & -1009 \\ 0.5 & 4.05 & 995 \\ 0.5 & 2.25 & 1011.5 \end{array} \right) $$

|   i   | $x_1$ | $x_2$ | $y$ | $\hat{y}$ | $\hat{y} - y$ | $\ell$ | $\frac{\partial \ell}{\partial b}$ | $\frac{\partial \ell}{\partial w_1}$ | $\frac{\partial \ell}{\partial w_2}$ |
| ----- | ----- | ----- | --- | --------- | -------- | -------- | ---------- | ----------- | --------- |
|   1   |  6.2  | 2024  |  1  |    0.5    |   -0.5   |   0.693  |    -0.5    |    -3.1     |   -1012   |
|   2   |  7.8  | 2018  |  1  |    0.5    |   -0.5   |   0.693  |    -0.5    |    -3.9     |   -1009   |
|   3   |  8.1  | 1990  |  0  |    0.5    |    0.5   |   0.693  |     0.5    |    4.05     |    995    |
|   4   |  4.5  | 2023  |  0  |    0.5    |    0.5   |   0.693  |     0.5    |    2.25     |   1011.5  |

Averaging over all $m=4$ examples, the gradients are,

$$ \frac{\partial L}{\partial w} = \frac{1}{m} \sum_{i=1}^m \left( \begin{array}{cc} \frac{\partial \ell_i}{\partial b} \\ \frac{\partial \ell_i}{\partial w_1} \\ \frac{\partial \ell_i}{\partial w_2} \end{array} \right) $$

$$ \frac{\partial L}{\partial w} = \frac{1}{m} \left( \begin{array}{cc} -0.5 - 0.5 + 0.5 + 0.5 \\ -3.1 - 3.9 + 4.05 + 2.25 \\ -1012 - 1009 + 995 + 1011.5 \end{array} \right) $$

$$ \frac{\partial L}{\partial w} = \left( \begin{array}{cc} 0 \\ -0.175 \\ -3.625 \end{array} \right) $$

---
#### Step 6 - Updating Weights

Assuming the learning rate, $\alpha = 0.01$

$$ w := w - \alpha \frac{\partial L}{\partial w} $$

$$ w := \left( \begin{array}{cc} 0 \\ 0 \\ 0 \end{array} \right) -0.01 \left( \begin{array}{cc} 0 \\ -0.175 \\ -3.625 \end{array} \right)$$

$$ w := \left( \begin{array}{cc} 0 \\ 0.00175 \\ 0.03625 \end{array} \right)$$

#### Step 7 - Repeat

- Steps 1 - 6 is called one epoch where the model sees all the training examples once and finds the error in its prediction and updates its parameters.

- These steps are repeated for certain number of epochs such that the training error is minimized and the model becomes confident about the training data 

---

## Formulation

```
m = len(X)
_bias = 0
_weights = np.zeros(m)

def linear_model(X):
    return _bias + np.dot(X,_weights)

def sigmoid(X):
    return 1/(1 + np.exp(-linear_model(X)))

def predict_proba(X):
    return sigmoid(X)

def predict(z):
    y_prob = predict_proba(X)
    return np.where(y_prob > 0.5,1,0)

def fit(X,y,iterations=100,learning_rate=0.01):
    for i in range(iterations):
        # Model predicts
        y_pred = predict_proba(X)
        residual = y_pred - y
        # Compute Gradients
        dLdb = (1/m) * sum(residual)
        dLdw = (1/m) * np.dot(residual,X)
        # Update Model parameters
        _bias += (-1 * learning_rate * dLdb)
        _weights += (-1 * learning_rate * dLdw)

```

---

## Implementation

```
from logistic_regression import LogisticRegression

# Data Gathering
df = pd.DataFrame({
    'movie':['Kalki','Tumbbad','Indiana Jones','Tiger 3'],
    'rating':[6.2,7.8,8.1,4.5],
    'released_date':[2024,2018,1990,2023],
    'watched':[1,1,0,0]
})

# Feature Engg
df['recency'] = pd.Timestamp.today().year - df['released_date']

# Training Data Prep
X = df[['rating','recency']].to_numpy()
y = df['watched'].to_numpy()

# Model Training
model = LogisticRegression(iterations=1,learning_rate=0.01)
model.fit(X,y)
print(model.history())      # [0.6931471805599453] -> gives loss history in 1 epoch of training
print(model.gradients())    # [0.0, -0.175,  3.625] -> loss gradient relevant to each model parameter 
print(model.parameters())   # [0.0, 0.00175, -0.03625] -> updated model parameters
```

All the values match with the Hand Calculation done for 1 epoch of training

For full implementation, refer following repository

Github - [ML Concepts](https://github.com/gouherdanish/ml_concepts/blob/main/logistic_regression.py)

---

## Verification

We verify our own implementation against the state-of-the-art sklearn implementation.

### SOTA Implementation Using Sklearn

```
import sklearn

model = sklearn.linear_model.SGDClassifier(loss='log_loss',penalty=None,max_iter=100,learning_rate='constant',eta0=0.01,random_state=42)
model.fit(X, y)
print(model.coef_)                      # [[ 0.24185039 -0.17151609]] 
print(model.itercept_)                  # [-0.02785408]

y_val_prob = model.predict_proba(X)
y_val = model.predict(X)
print([p2 for p1,p2 in y_val_prob])     # [0.8133032438552896, 0.696245481408132, 0.019832274960378975, 0.7086748039455667]
print(y_val)                            # [1 1 0 1]
```

### Scratch Implementation

```
from logistic_regression import LogisticRegression

model = LogisticRegression(iterations=100,learning_rate=0.01)
model.fit(X, y)
print(model.parameters()[0])            # [ 0.23113958, -0.15335232]
print(model.parameters()[1])            # -0.026008310204226472

y_val_prob = model.predict_proba(X)
y_val = model.predict(X)
print(y_val_prob)                       # [0.80330136 0.70198464 0.03331849 0.70282098]
print(y_val)                            # [1 1 0 1]
```

Note:
- The final predicted classes match perfectly with Sklearn.
- Individual probability values and model weights don't match exactly since Sklearn uses additional parameters while training the model
```
print(model.get_params())
{'alpha': 0.0001, 'average': False, 'class_weight': None, 'early_stopping': False, 'epsilon': 0.1, 'eta0': 0.01, 'fit_intercept': True, 'l1_ratio': 0.15, 'learning_rate': 'constant', 'loss': 'log_loss', 'max_iter': 100, 'n_iter_no_change': 5, 'n_jobs': None, 'penalty': None, 'power_t': 0.5, 'random_state': 42, 'shuffle': True, 'tol': 0.001, 'validation_fraction': 0.1, 'verbose': 0, 'warm_start': False}
```
