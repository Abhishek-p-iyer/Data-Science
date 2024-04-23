___

## 1. One sample T-test
We are inspecting the purchase amount of an online platform with the following hypothesis 
- Null: The average cost of a BuyPie order is 1000 Rupees
- Alternative: The average cost of a BuyPie order is **not** 1000 Rupees.

SciPy has a function called `ttest_1samp()`, which performs a one-sample t-test for you. `ttest_1samp()` requires two inputs, a sample distribution (eg. the list of the 50 observed purchase prices) and a mean to test against (eg. `1000`):
```Python
tstat, pval = ttest_1samp(sample_distribution, expected_mean)
```

### Assumptions of a One Sample T-test
When running any hypothesis test, it is important to know and verify the assumptions of the test. The assumptions of a one-sample t-test are as follows:

##### 1. The sample was randomly selected from the population
- For example, if you only collect data for site visitors who agree to share their personal information, this subset of visitors was not randomly selected and may differ from the larger population.
##### 2. The individual observations were independent
 - For example, if one visitor to BuyPie loves the apple pie they bought so much that they convinced their friend to buy one too, those observations were not independent.
##### 3. The data is normally distributed without outliers OR the sample size is large (enough)
- There are no set rules on what a “large enough” sample size is, but a common threshold is around 40. For sample sizes smaller than 40, and really all samples in general, it’s a good idea to make sure to plot a histogram of your data and check for outliers, multi-modal distributions (with multiple humps), or skewed distributions. If you see any of those things for a small sample, a t-test is probably not appropriate.
## 2. Binomial Test with a function

```Python
import numpy as np
import pandas as pd
from scipy.stats import binom_test

def simulation_binomial_test(observed_successes, n, p):
  #initialize null_outcomes
  null_outcomes = []
  #generate the simulated null distribution
  for i in range(10000):
    simulated_monthly_visitors = np.random.choice(['y', 'n'], size=n, p=[p, 1-p])
    num_purchased = np.sum(simulated_monthly_visitors == 'y')
    null_outcomes.append(num_purchased)

  #calculate a 1-sided p-value
  null_outcomes = np.array(null_outcomes)
  p_value = np.sum(null_outcomes <= 41)/len(null_outcomes)
  #return the p-value
  return p_value

p_value1 = simulation_binomial_test(41, 500, 0.1)
print("simulation p-value: ", p_value1)
p_value2 = binom_test(41, 500, .1, alternative = 'less')
print("binom_test p-value: ", p_value2)
```

## 3. Binomial Test with SciPy
SciPy has a function called `binom_test()`, which performs a binomial test for you. The default alternative hypothesis for the `binom_test()` function is two-sided, but this can be changed using the `alternative` parameter (eg., `alternative = 'less'` will run a one-sided lower tail test).
`binom_test()` requires three inputs, the number of observed successes, the number of total trials, and an expected probability of success. For example, with 10 flips of a fair coin (trials), the expected probability of heads is 0.5. Let’s imagine we get 2 heads (observed successes) in 10 flips. Is the coin weighted? The function call for this binomial test would look like:
```Python
from scipy import binom_test  
p_value = binom_test(2, n=10, p=0.5)  
print(p_value) #output: 0.109
```

Now for our case study, lets try the function out 
```Python 
import numpy as np
import pandas as pd
from scipy.stats import binom_test
# calculate p_value_2sided here:
p_value_2sided = binom_test(41, n=500, p=0.1)
print(p_value_2sided)
# calculate p_value_1sided here:
p_value_1sided = binom_test(41, n=500, p=0.1, alternative='less')
print(p_value_1sided)
```

Now once we get out P-value, we will need to conclude weather the difference in the mean is actually significant or not, for which we use the [[Significance Threshold]]. The threshold value can be anywhere between 0 and 1, but we usually pre-set it to be 0.05. 

>[!info] One sample t-test and binomial t-test is conducted when we have a sample statistic and we are comparing it to a hypothesized population value]

______
___

## 1. Two sample T-Test
We are inspecting a website and we want to find out if the users spent more time on the new website or not
- Null: The time spent on both the websites are equal
- Alternative: The time spent on both the websites are not equal
```Python
from scipy.stats import ttest_ind
tstat, pval = ttest_ind(<version1>, <version2>) #here these are lists
```

Now if we find the significance to be lower than 0.05, we consider it to be significant.

#### Multiple Tests
In some circumstances, we might instead care about an association between a quantitative variable and a **non-binary** categorical variable. 
For example, suppose that we own a chain of stores that sell ants, called VeryAnts. There are three different locations: A, B, and C. We want to know whether customers are spending a significantly different amount per order at any of the locations.
There are three different comparisons we could make: A vs. B, B vs. C, and A vs. C. One way to answer our question is to simply run three separate 2-sample t-tests. This process will inflate our possibility of getting a type 1 error (explained in [[Significance Threshold]]). 
To overcome this, we use ANOVA (Analysis of Variance)
## 2. ANOVA 
In Python, we can use the SciPy function `f_oneway()` to perform an ANOVA. `f_oneway()` has two outputs: the F-statistic and the p-value. 
If we were comparing scores on a video-game for math majors, writing majors, and psychology majors, we could run an ANOVA test with this line:
```Python
from scipy.stats import f_oneway  
fstat, pval = f_oneway(scores_mathematicians, scores_writers, scores_psychologists)
```
If the p-value is below our significance threshold, we can conclude that at least one pair of our groups earned significantly different scores on average; ==however, we won’t know which pair until we investigate further!==

## 3. Tukey's range Test
Let’s say that we have performed an ANOVA to compare sales at the three VeryAnts stores. We calculated a p-value less than 0.05 and concluded that there is a significant difference between at least one pair of stores.
Now, we want to find out **which** pair of stores are different. This is where Tukey’s range test comes in handy!
In python, we can perform Tukey's range test using `statsmodel` functions `pairwise_tukeyhsd()`. For example, suppose we again are comparing the videogame scores for math majors, writing majors, and psychology majors. We have a dataset named `data` with two columns: `score` and `major`. We could run Tukey’s range test with a type I error rate of 0.05 as follows:
```Python
from statsmodel.stats.multicomp import pairwise_tukeyhsd
tukey_result = pairwise_tukeyhsd(data.score, data.major, 0.05)
print(tukey_results)
```
```
Multiple Comparison of Means - Tukey HSD,FWER=0.05 ========================================== group1 group2 meandiff lower upper reject ------------------------------------------ math psych 3.32 -0.11 6.74 False math write 5.23 2.03 8.43 True psych write -2.12 -5.25 1.01 False ------------------------------------------
```

Tukey’s range test is similar to running three separate 2-sample t-tests, except that it runs all of these tests simultaneously in order to preserve the type I error rate.
The function output is a table, with one row per pair-wise comparison. For every comparison where `reject` is `True`, we “reject the null hypothesis” and conclude there _is_ a significant difference between those two groups. For example, in the output above, we would conclude that there is a significant difference between scores for math and writing majors, but no significant difference in scores for the other

## Assumptions for T-test, ANOVA, Tukey's test

##### 1. The observations must be independently randomly sampled from the population 
- Lets take an example of the site visitors of website, Randomly sampling will help ensure that the sample is representative of the population. If we only sample visitors on Halloween, then the visitors must have acted differently. 
##### 2. The Standard Deviation of the groups should be equal
##### 3. The data should be normally distributed 
##### 4. The groups created by categorical variables must be independent

## Chi-Squared Test
If we want to understand whether the outcomes of two categorical variables are associated, we can use a Chi-Square test.
It is useful in situations like:

- An A/B test where half of users were shown a green submit button and the other half were shown a purple submit button. Was one group more likely to click the submit button?
- People under and over age 40 were given a survey asking “Which of the following three products is your favorite?” Did these age groups have significantly different preferences?

In `SciPy`, we can use the function `chi2_contingency()` to perform a Chi-Square test. The input to `chi2_contingency` is a contingency table, which can be created using the `pandas` `crosstab()` function as follows:
```Python
#create table:  
import pandas as pd  
table = pd.crosstab(variable_1, variable_2)  
  
#run the test:  
from scipy.stats import chi2_contingency  
chi2, pval, dof, expected = chi2_contingency(table)
```

## Assumptions of Chi-Squared Test
##### 1. The observations should be independently randomly sampled from the population
##### 2. The categories of both variables must be mutually exclusive
##### 3. The groups should be independent
## Binomial Test
In this lesson, we will walk through a simulation of a binomial hypothesis test in Python. Binomial tests are useful for comparing the frequency of some outcome in a sample to the expected probability of that outcome. For example, if we expect 90% of ticketed passengers to show up for their flight but only 80 of 100 ticketed passengers actually show up, we could use a binomial test to understand whether 80 is significantly different from 90.

Binomial tests are similar to one-sample t-tests in that they test a sample statistic against some population-level expectation. The difference is that:

- ==Binomial tests== are used for ==binary categorical data== to compare a sample frequency to an expected population-level probability
- ==One-sample t-tests== are used for ==quantitative data== to compare a sample mean to an expected population mean.

>[!info] We use two sample test, ANOVA, Tukeys range test, Chi-square test when we are testing the association between a categorical variable and quantitative, categorical variable respectively.


#Python