___
When we have two quantitative variables, information about the first variable gives us information about the second variable. Now measuring the strength of the relationship between these two variables is what we'll be doing. 
We can plot a scatter plot to access the relationship between these two variables, we look for any kind of pattern that is visible with respect to these two variables, we check if there is a general upward trend or a general downward trend in the data. 

### Scatter Plot 
```Python 

plt.scatter(x=housing.price, y=housing.sqfeet) 
plt.xlabel('Rental Price (USD)')  
plt.ylabel('Area (Square Feet)')
plt.show()
```

![[Pasted image 20240105134602.png]]

While there are several data points in this graph, we can see that there is a general upward trend in the visual which would indicate that there is in fact a relationship between these two variables. We will explore this relationship further with the help of summary statistics like covariance or correlation which would give us a better idea of the strength of the linearity in two variables. 

## Covariance 
The covariance measures the strength of linearity between two variables. It ranges from -infinity to +infinity, a larger positive value of covariance would indicate that there is a strong positive relationship which simply put means that a larger value of one variable would mean a larger value of another.
We can calculate the covariance with the help of the `cov()` function in Numpy.  The covariance matrix would look like 

| cov-table | **variable 1** | **variable 2** |
| ---- | ---- | ---- |
| **variable 1** | variance  | covariance  |
| **variable 2** | covariance | variance  |
```Python 

cov_matrix = np.cov(housing.price, housing.sqfeet)
print(cov_matrix)

# OUTPUT
# [[184332.9  57336.2]  
# [ 57336.2 122045.2]]
```

### Correlation 
Correlation is the scaled down version of covariance, it ranges from -1 to +1. Highly associated variables with a positive linear relationship will have a correlation close to 1. Highly associated variables with a negative linear relationship will have a correlation close to -1. Variables that do not have a linear association (or a linear association with a slope of zero) will have correlations close to 0.
The `pearsonr()` function from `scipy.stats` can be used to calculate the correlation
```Python 
from scipy.stats import pearsonr
corr, p = pearsonr(housing.price, housing.sqfeet)
print(corr)

# OUTPUT: 0.507
```

Generally, a correlation larger than about .3 indicates a linear association. A correlation greater than about .6 suggestions a strong linear association.

> [!info] It’s important to note that there are some limitations to using correlation or covariance as a way of assessing whether there is an association between two variables. Because correlation and covariance both measure the strength of **linear** relationships with non-zero slopes, but not other kinds of relationships, correlation can be misleading.

For example, the four scatter plots below all show pairs of variables with near-zero correlations.
![[Pasted image 20240105140513.png]]


#Python