## Information Content and Entropy : Intuition, and Uses

Information content and Entropy are very important concept in Information theory and are widely used in Machine Learning. Below we try to learn important points related to this in detail

---

### Information Content (Self-Information)
- Information content quantifies how much information is gained (or how much surprise there is) when a specific event happens.
- Surprising Events Carry More Information
    - Lower Probability => Higher Information
    - Higher Probability => Lower Information
- Example:
    - **Coin Flip** - If you flip a fair coin, and it lands on heads, the outcome isn't very surprising because the probability of heads is 0.5. The information content here is relatively low
    - **Die Roll** - If you roll a die and it lands on 6, the outcome is more surprising because the probability of rolling a 6 is only `1/6`. The information content here is higher

**Definition of Information:**
- The information content of an event `x` with probability `p(x)` is defined as:

$$ 
I(x) = -log(p(x)) 
$$

- This is based on the idea that rare events carry more information when they occur, while common events carry less
    - If an event is very likely (high `p(x)`), the information content is low (because it's not surprising).
    - If an event is unlikely (low `p(x)`), the information content is high (because it's surprising).

- Why the Negative Log? 
    - The negative sign ensures that information content is positive

$$ 0 <= p(x) <= 1 $$

$$ log(p(x)) <= 0 $$

$$ -log(p(x)) >= 0 $$

$$ I(x) >= 0 $$

### Entropy

- Entropy is a concept from information theory introduced by Claude Shannon in 1948. 
- It measures the uncertainty or randomness in a probability distribution. 
- In the context of machine learning and data science, entropy quantifies the unpredictability or impurity in a dataset.

**Definition of Entropy:**
For discrete random variable X with possible outcomes $`\left( x_1, x_2, ..., x_n \right)`$ and a probability distribution `p(x)`, entropy `H(X)` is defined as the expected value of the information content across all possible outcomes of a random variable.

$$
H\left( X \right) = -\sum_{i=1}^n p\left( x_i \right) log_2 p\left( x_i \right) 
$$


