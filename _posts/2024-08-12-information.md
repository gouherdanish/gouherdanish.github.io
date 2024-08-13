## Information : Measuring surprise level of an event

### Intuition
- Information content quantifies how much information is gained (or how much surprise there is) when a specific event happens
- It is also known as self-information
- Surprising events carry more information
    - Lower Probability => Rare event => Higher Information
    - Higher Probability => Common event => Lower Information
- Example:
    - **Coin Flip** - If you flip a fair coin, and it lands on heads, the outcome isn't very surprising because the probability of heads is 0.5. The information content here is relatively low
    - **Die Roll** - If you roll a die and it lands on 6, the outcome is more surprising because the probability of rolling a 6 is only $1/6$. The information content here is higher

### Definition
- The information content of an event $x$ with probability $p(x)$ is defined as:

$$ I(x) = -log(p(x)) $$

- This is based on the idea that rare events carry more information when they occur, while common events carry less
    - If an event is very likely i.e. high $p(x)$, the information content is low (because it's not surprising).
    - If an event is unlikely i.e. low $p(x)$, the information content is high (because it's surprising).

### Properties
- Information content is always positive

$$ 0 <= p(x) <= 1 $$

$$ log(p(x)) <= 0 $$

$$ -log(p(x)) >= 0 $$

$$ I(x) >= 0 $$

### Hand Calculation
Consider an experiment where a fair coin is flipped 3 times. Here are following outcomes

$$ Sample Space = \{HHH,HHT,HTH,THH,HTT,THT,TTH,TTT\} $$

Let $X$ denote a random variable which represents the number of heads in 3 consecutive coin flips

$$ X = \{0,1,2,3\} $$

The probability $p(X)$ associated with each $X$

$$ p(X=0) = p(TTT) = 1/8 $$

$$ p(X=1) = p(HTT or THT or TTH) = 3/8 $$

$$ p(X=2) = p(HHT or HTH or THH) = 3/8 $$

$$ p(X=3) = p(HHH) = 1/8 $$

Information $I(X)$ for each $X$ can be calculated as follows,

$$ I(X=0) = -log(p(X=0)) = -log(1/8) = 2.07944154 $$

$$ I(X=1) = -log(p(X=1)) = -log(3/8) = 0.98082925 $$

$$ I(X=2) = -log(p(X=2)) = -log(3/8) = 0.98082925 $$

$$ I(X=3) = -log(p(X=3)) = -log(1/8) = 2.07944154 $$

### Formulation
```
from numpy import log

def information(prob: float):
    return -log(prob)

>>> information(prob=3/8) = 0.98082925 # High probability has low information content
>>> information(prob=1/8) = 2.07944154 # Low probability has high information content
```

### OOPS Implementation
```
from random_event import RandomEvent

event = RandomEvent(x='Getting a six when a fair die is thrown', prob=1/6)
print(event.information) # 1.791759469228055
```
For full implementation, refer following repository

Github - [Stats Concepts](https://github.com/gouherdanish/stats_concepts/blob/main/random_event.py)





