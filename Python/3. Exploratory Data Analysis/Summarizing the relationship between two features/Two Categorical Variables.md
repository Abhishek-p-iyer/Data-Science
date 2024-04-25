___
## Contingency Tables: Frequency

Contingency tables are also called two-way tables or cross-tabulations, are useful for summarizing two variables at the same time. 
As an example, we’ll explore a sample of data from the Narcissistic Personality Inventory (NPI-40), a personality test with 40 questions about personal preferences and self-view.
There are two possible responses to each question. The sample we’ll be working with contains responses to the following:

- `influence`: `yes` = I have a natural talent for influencing people; `no` = I am not good at influencing people.
- `blend_in`: `yes` = I prefer to blend in with the crowd; `no` = I like to be the center of attention.
- `special`: `yes` = I think I am a special person; `no` = I am no better or worse than most people.
- `leader`: `yes` = I see myself as a good leader; `no` = I am not sure if I would make a good leader.
- `authority`: `yes` = I like to have authority over other people; `no` = I don’t mind following orders.
Suppose we are interested in understanding whether there is an association between `influence` (whether a person thinks they have a talent for influencing people) and `leader` (whether they see themself as a leader). We can use the `crosstab` function from pandas to create a contingency table: 
```Python 
cont_table = pd.crosstab(npi.influence, npi.leader)
print(cont_table)
```

```
leader       no      yes 
influence 
no           3015    1293 
yes          2360    4429
```

This table tells us the number of people who gave each possible combination of responses to these two questions. 
To assess whether there is an association between these two variables, we need to ask whether information about one variable gives us information about the other. In this example, we see that among people who **don’t** see themselves as a leader (the first column), a larger number (3015) **don’t** think they have a talent for influencing people. Meanwhile, among people who **do** see themselves as a leader (the second column), a larger number (4429) **do** think they have a talent for influencing people.
So, if we know how someone responded to the leadership question, we have some information about how they are likely to respond to the influence question. This suggests that the variables are associated.

## Contingency Tables: Proportions 
In the previous exercise, we looked at an association between the `influence` and `leader` questions using a contingency table of frequencies. However, sometimes it’s helpful to convert those frequencies to proportions. We can accomplish this simply by dividing the all the frequencies in a contingency table by the total number of observations (the sum of the frequencies):
```Python
influence_leader_freq = pd.crosstab(npi.influence, npi.leader)  
influence_leader_prop = influence_leader_freq/len(npi)  
print(influence_leader_prop)
```

```
leader           no          yes 
influence 
no               0.271695    0.116518 
yes              0.212670    0.399117
```

The resulting contingency table makes it slightly easier to compare the proportion of people in each category. 
We can also see that almost 40% of the surveyed population (by far the largest proportion) both see themselves as leaders and think they have a talent for influencing people.

## Marginal Proportions 
The proportion of respondents in each category of a single question is called a _marginal proportion_. We can calculate all the marginal proportions from the contingency table of proportions using row and column sums as follows: 
```Python 
leader_marginals = influence_leader_prop.sum(axis=0)  
print(leader_marginals)  
influence_marginals =  influence_leader_prop.sum(axis=1)  
print(influence_marginals)
```
```
leader 
no 0.484365 
yes 0.515635 
dtype: float64 

influence 
no 0.388213 
yes 0.611787 
dtype: float64
```

## Expected Contingency Tables
We calculate the expected contingency using the marginal proportions. To calculate these expected proportions, we need to multiply the marginal proportions for each combination of categories:

|      | leader = no     | leader = yes |
| --------|---------|-------|
| **influence = no**  | 0.484*0.388 = 0.188   | 0.516*0.388 = 0.200    |
| **influence = yes** | 0.484*0.612 = 0.296 | 0.516*0.612 = 0.315    |

These proportions can then be converted to frequencies by multiplying each one by the sample size (11097 for this data):

|  | leader = no | leader = yes |
| ---- | ---- | ---- |
| **influence = no** | 0.188*11097 = 2087 | 0.200*11097  =2221 |
| **influence = yes** | 0.296*11097 = 3288 | 0.315*11097 = 3501 |

This table tells us that **if** there were no association between the `leader` and `influence` questions, we would expect 2087 people to answer `no` to both.
In python, we can calculate this table using the `chi2_contingency()` function from SciPy, by passing in the observed frequency table. There are actually four outputs from this function, but for now, we’ll only look at the fourth one:
```Python 
from scipy.stats import chi2_contingency  
chi2, pval, dof, expected = chi2_contingency(influence_leader_freq)  
print(np.round(expected))
```

```
[[2087. 2221.] 
[3288. 3501.]]
```
Note that the ScyPy function returned the same expected frequencies as we calculated “by hand” above! Now that we have the expected contingency table if there’s no association, we can compare it to our observed contingency table:

```
leader           no      yes 
influence 
no               3015    1293 
yes              2360    4429
```
==The more that the expected and observed tables differ, the more sure we can be that the variables are associated.== In this example, we see some pretty big differences (eg., 3015 in the observed table compared to 2087 in the expected table). This provides additional evidence that these variables are associated.

## The Chi-Square Statistic
 many data scientists use the _Chi-Square statistic_ to summarize **how** different these two tables are. To calculate the Chi Square statistic, we simply find the squared difference between each value in the observed table and its corresponding value in the expected table, and then divide that number by the value from the expected table; finally add up those numbers:
 $$ ChiSquare = \sum \frac{(observerd - expected)^2}{expected} $$
 The Chi-Square statistic is also the first output of the SciPy function `chi2_contingency()`:
```Python 
from scipy.stats import chi2_contingency  
chi2, pval, dof, expected = chi2_contingency(influence_leader_freq)  
print(chi2)  
output: 1307.88
```

The interpretation of the Chi-Square statistic is dependent on the size of the contingency table. For a 2x2 table (like the one we’ve been investigating), a Chi-Square statistic larger than around 4 would strongly suggest an association between the variables. In this example, our Chi-Square statistic is much larger than that — 1307.88! This adds to our evidence that the variables are highly associated.


#Python
