___

## Law of Large Numbers 
Before we get into the rules of probability, lets look into a law that states that, if a particular experiment is conducted a large number of times, the probability that an event A can occur will converge to its true probability. 

## Rules of probability 

### Addition Rule 
If we want to find the probability of one or both the events, we use the addition rule in probability that states that if P(A) is the probability of even A occurring and P(B) is the probability of event B occurring, then the probability of one or both of these events occurring would be - 
$$ P(A\  or\  B) = P(A) + P(B) - P(A \ and \ B )$$

> If these events are mutually exclusive, then 
$$ P(A \ or \ B) = P(A) + P(B)$$


### Multiplication Rule 
When we want to calculate the probability of both events occurring simultaneously, then the probability is 
$$ P(A \ and \ B) = P(A).P(B|A)$$

If they are independent variables, then 
$$ P(A \ and \ B) = P(A).P(B)$$

### Bayes Theorem
The Bayes theorem describes the probability of an event  based on prior knowledge of conditions that might be related to the event. 
$$ P(B \ |\  A) = \frac{P(A\ | \ B) . P(B)}{P(A)}$$

#Python