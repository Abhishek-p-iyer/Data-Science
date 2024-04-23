___
## Sampling Distribution 
When we want to calculate the average weight of a population of an animal species, it would be impossible for us to measure each and every animal in this world. To work around this, we take a sample of this population and then take the mean of the population and repeat the process several times. 
```Python 
salman_population = population['Salmon_Weight']

sample_size = 50
sample_mean = []

for i in range(500):
	samp = np.random.choice(salman_population, sample_siz, replace=False)
	sample_m = samp.mean()
	sample_mean.append(sample_m)

sns.histplot(sample_mean, stat='density')
plt.title("Sampling Distribution of the Mean")
```


![[Pasted image 20240111075711.png]]

## Central Limit Theorem 
The central limit theorem states that the sampling distribution of the mean of a population will be normally distributed if the sample size is large enough and the population is not skewed. 
We also will be able to calculate the sample mean and standard deviation using the true mean and the true standard deviation of the sample. 
The CLT states that: 
- The mean of the sample will be similar to the true mean 
- The standard deviation will be the standard deviation divided by the square root of the population size 

$$ Sampling \ distribution \ STD = \frac{\sigma}{\sqrt{n}} $$
>The standard deviation of the distribution is also called standard error of the estimated mean.

Two important things to note here is:
- As the population increases, the standard deviation decreases 
- As the standard deviation increases, so does the standard error

>[!info] 95% of normally distributed values are within about 1.96 standard deviations of the mean.



## Biased Estimators
According to the CLT, when the mean of the sampling distribution of the statistic is equal to the population statistic, it is an _unbiased estimator_. For eg: The mean of the sampling distribution of the mean is equal to the population mean, which would make it an unbiased estimator. 
There are different statistics that are biased, for eg: The mean of the sampling distribution of the variance or the maximum value is not equal to the population variance or the population maximum. 

### Calculating probabilities 
Suppose we want to transport salmon in a crate that withstands a maximum weight of 750lbs and we would want to transport 10 fish in a crate. Now we will need to calculate the probability that the average weight of each salmon fish is less than 75lbs. With a mean of 60 and standard deviation of 40lbs
```Python 
import scipy.stats as stats
x = 75
mean = 60
sample_size = 10
std_error = 40/sqrt(10)
print(stats.norm.cdf(x,mean,std_error))

# OUTPUT => 0.882
```

There is an 88% chance that the average weight of the salmon fish will be below 75lbs. 


#Python

