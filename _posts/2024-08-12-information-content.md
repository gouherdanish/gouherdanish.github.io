## Information Content : Intuition, and Uses

### Intuition
- Information content quantifies how much information is gained (or how much surprise there is) when a specific event happens
- It is also known as self-information
- Surprising Events Carry More Information
    - Lower Probability => Higher Information
    - Higher Probability => Lower Information
- Example:
    - **Coin Flip** - If you flip a fair coin, and it lands on heads, the outcome isn't very surprising because the probability of heads is 0.5. The information content here is relatively low
    - **Die Roll** - If you roll a die and it lands on 6, the outcome is more surprising because the probability of rolling a 6 is only $1/6$. The information content here is higher

### Definition
- The information content of an event $x$ with probability $p(x)$ is defined as:

$$ 
I(x) = -log(p(x)) 
$$

- This is based on the idea that rare events carry more information when they occur, while common events carry less
    - If an event is very likely i.e. high $p(x)$, the information content is low (because it's not surprising).
    - If an event is unlikely i.e. low $p(x)$, the information content is high (because it's surprising).

### Properties
- Information content is always positive

$$ 0 <= p(x) <= 1 $$

$$ log(p(x)) <= 0 $$

$$ -log(p(x)) >= 0 $$

$$ I(x) >= 0 $$




