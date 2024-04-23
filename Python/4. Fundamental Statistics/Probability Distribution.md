___
## 1. Random Variables 
Random variables in its simplest form is a function and we use random variables to represent random events. These random variables are always numeric and does not represent a characteristics or a qualities. If we want them to represent anything other than numeric values, we can assign integer values to these non-numeric values. For example, When a coin is flipped, we either get heads or tails, now we can represent heads as 0 and tails as 1. 
In python we use the syntax : `np.random.choice(a, size = size, replace = True/False)`, 
where 
- `a` is a list or other object that has values we're sampling from, 
- `size` is a number that represents how many values to choose
- `replace` can either be equal to `True` or `False`, we can either replace the chosen value from the sample or not. 
```Python 
import numpy as np  
# 7 is not included in the range function  
die_6 = range(1, 7)  
rolls = np.random.choice(die_6, size = 2, replace = True)   
print(rolls)

# OUTPUT = >[2, 5]
```

There are two kinds of random variables:
- Discrete Random Variables 
- Continuous Random Variables
#### Discrete Random Variables 
The random variables that can be counted, or are discrete are called Discrete random variables. The rolling of a die is considered to be a discrete event where the variables can range from 1-6. 

#### Continuous Random Variables 
When the possible values of the random variables are said to be uncountable, it is called a continuous random variable. These are generally measurement variables. for example, the temperature of a certain place is said to be a continuous random variable. 


## 2.Probability Mass Function
The probability Mass Function or the PMF is a type of distribution that defines the probability of observing a particular value of a discrete random variable. 
To understand PMF a little better, lets first understand a binomial distribution using an example. 
Suppose that we flip a fair coin some number of times and count the number of heads. The probability mass function that describes the likelihood of each possible outcome (eg., 0 heads, 1 head, 2 heads, etc.) is called the _binomial distribution_.
The parameters for the binomial distribution are:

- `n` for the number of trials (eg., n=10 if we flip a coin 10 times)
- `p` for the probability of success in each trial (probability of observing a particular outcome in each trial. In this example, p= 0.5 because the probability of observing heads on a fair coin flip is 0.5)
If we flip a fair coin 10 times, we say that the number of observed heads follows a `Binomial(n=10, p=0.5)` distribution.
The heights of the bars represent the probability of observing each possible outcome as calculated by the PMF.
![[Pasted image 20240110091105.png]]

Now, lets find the Probability Mass Function of a binomial distribution using the `scipy.stats` library. The `binom.pmf()` function takes in three parameters 
- `x`: the value of interest 
- `n`: the number of trials 
- `p`: the probability of success 
For ex: If we want to flip a coin 10 times and want to find the probability of observing heads 6 times, 
```Python 
import scipy.stats as stats
print(stats.binom.pmf(6,10,0.5))

# OUTPUT -> 0.205078
```

We can even calculate the probability mass function over a range of values, for example, to calculate the probability of getting any number between 1 and 3. 
$$ P(1<=X<=3) = P(X=1) + P(X=2) + P(X=3)$$
```Python
import scipy.stats as stats
print(stats.binom.pmf(1,10,0.5) + stats.binom.pmf(2,10,0.5) + stats.binom.pmf(3,10,0.5))
```

## 3. Cumulative Distribution Function 
The _cumulative distribution function_ for a discrete random variable can be derived from the probability mass function. However, instead of the probability of observing a specific value, the cumulative distribution function gives the probability of observing a specific value OR LESS.
As previously discussed, the probabilities for all possible values in a given probability distribution add up to 1. The value of a cumulative distribution function at a given value is equal to the sum of the probabilities lower than it, with a value of 1 for the largest possible number.

We can use a cumulative distribution function to calculate the probability of a specific range by taking the difference between two values from the cumulative distribution function. For example, to find the probability of observing between 3 and 6 heads, we can take the probability of observing 6 or fewer heads and subtracting the probability of observing 2 or fewer heads. This leaves a remnant of between 3 and 6 heads.
We can use the `binom.cdf()` method from the `scipy.stats` library to calculate the cumulative distribution function. This method takes 3 values:

- `x`: the value of interest, looking for the probability of this value or less
- `n`: the sample size
- `p`: the probability of success
Calculating the probability of observing 6 or fewer heads from 10 fair coin flips (0 to 6 heads) mathematically looks like the following:
$$ P(X<=6) = P(0-6 heads)$$
```Python 
import scipy.stats as stats

print(stats.binom.cdf(6,10,0.5))
# OUTPUT => 0.828125
```

## Probability Density Function

Similar to how discrete random variables relate to probability mass functions, continuous random variables relate to probability density functions. They define the probability distributions of continuous random variables and span across all possible values that the given random variable can take on.
When graphed, a probability density function is a curve across all possible values the random variable can take on, and the total area under this curve adds up to 1.

> In a probability density function, we cannot calculate the probability at a single point. This is because the area of the ==curve underneath a single point is always zero.==

Suppose we want to calculate the area under the cure, we can do this by calculating the cumulative distribution function. 
For example, heights fall under a type of probability distribution called a _normal distribution_. The parameters for the normal distribution are the mean and the standard deviation, and we use the form _Normal(mean, standard deviation)_ as shorthand.

We know that women’s heights have a mean of 167.64 cm with a standard deviation of 8 cm, which makes them fall under the Normal(167.64, 8) distribution.
Let’s say we want to know the probability that a randomly chosen woman is less than 158 cm tall. We can use the cumulative distribution function to calculate the area under the probability density function curve from 0 to 158 to find that probability.
We can calculate the area of the blue region in Python using the `norm.cdf()` method from the `scipy.stats` library. This method takes on 3 values:

- `x`: the value of interest
- `loc`: the mean of the probability distribution
- `scale`: the standard deviation of the probability distribution

```Python 
import scipy.stats as stats  
# stats.norm.cdf(x, loc, scale)  
print(stats.norm.cdf(158, 167.64, 8))

# OUTPUT => 0.1141
```


## Introduction to Poisson Distribution 
The Poisson distribution is another common distribution, and it is used to describe the number of times a certain event occurs within a fixed time or space interval. The Poisson distribution is defined by the rate parameter, symbolized by the Greek letter lambda, λ. Lambda represents the expected value — or the average value — of the distribution.

### Calculating the probability of exact value using PMF 
We can use the `poisson.pmf()` method in the `scipy.stats` library to evaluate the probability of observing a specific number given the parameter (expected value) of a distribution.
For example, suppose that we expect it to rain 10 times in the next 30 days. The number of times it rains in the next 30 days is “Poisson distributed” with lambda = 10. We can calculate the probability of exactly 6 times of rain as follows:
```Python
import scipy.stats as stats 
print(stats.poisson,pmf(6,10))
# OUTPUT =>0.06305545800345125
```

### Calculating the probability of a range using CDF 
We can use the `poisson.cdf()` method in the `scipy.stats` library to evaluate the probability of observing a specific number or less given the expected value of a distribution.
 For example, if we wanted to calculate the probability of observing 6 or fewer rain events in the next 30 days when we expected 10, we could do the following:

```Python
import scipy.stats as stats
print(stats.poisson.cdf(6,10))

#OUTPUT => 0.130141420882483
```

## Properties of Expectation
1. The expected value of two independent random variables is the sum of each expected value separately:
$$ E(X \ + \ Y) = E(X)+E(Y) $$
2. 1. Multiplying a random variable by a constant _a_ changes the expected value to be _a_ times the expected value of the random variable:
$$ E(aX) = a*E(x)$$
3. Adding a constant _a_ to the distribution changes the expected value by the value _a_:
$$ E(X+a) = E(X)+a$$
## Properties of Variance 
1. Increasing the values in a distribution by a constant _a_ does not change the variance:
$$ Var(X+a) = Var(X)$$
2. Scaling the values of a random variable by a constant _a_ scales the variance by the constant squared:
$$Var(aX) = a^2Var(X) $$
4. The variance of the sum of two random variables is the sum of the individual variances:
$$ Var(X+Y) = Var(X) + Var(Y)$$

#Python