___
## 1. Central Tendency for Quantitative Data
For quantitative variables, we often want to describe the _central tendency_, or the “typical” value of a variable. For example, what is the typical cost of rent in New York City?

There are several common measures of central tendency:

- **Mean**: The average value of the variable, calculated as the sum of all values divided by the number of values.
- **Median**: The middle value of the variable when sorted.
- **Mode**: The most frequent value of the variable.
- **Trimmed mean**: The mean excluding x percent of the lowest and highest data points.

```Python
# Mean  
rentals.rent.mean()  
  
# Median  
rentals.rent.median()  
  
# Mode  
rentals.rent.mode()  
  
# Trimmed mean  
from scipy.stats import trim_mean  
trim_mean(rentals.rent, proportiontocut=0.1)
```
## 2. Spread for Quantitative Data
The _spread_ of a quantitative variable describes the ==amount of variability.== This is important because it provides context for measures of central tendency.
There are several common measures of spread:

- **Range**: The difference between the maximum and minimum values of a variable.
- **Interquartile range (IQR)**: The difference between the 75th and 25th percentile values.
- **Variance**: The average of the squared distance from each data point to the mean.
- **Standard deviation (SD)**: The square root of the variance.
- **Mean absolute deviation (MAD)**: The mean absolute value of the distance between each data point and the mean.
```Python
# Range  
rentals.rent.max() - rentals.rent.min()  
  
# Interquartile range  
rentals.rent.quantile(0.75) - rentals.rent.quantile(0.25)  
  
from scipy.stats import iqr  
iqr(rentals.rent)  # alternative way  
  
# Variance  
rentals.rent.var()  
  
# Standard deviation  
rentals.rent.std()  
  
# Mean absolute deviation  
rentals.rent.mad()
```

## 3. Visualizing Quantitative Variables 
While summary statistics are certainly helpful for exploring and quantifying a feature, we might find it hard to wrap our minds around a bunch of numbers. This is why data visualization is such a powerful element of EDA.
For quantitative variables, _boxplots_ and _histograms_ are two common visualizations. These plots are useful because they simultaneously communicate information about minimum and maximum values, central location, and spread. Histograms can additionally illuminate patterns that can impact an analysis such as skew or multimodality. 

Python’s `seaborn` library, built on top of `matplotlib`, offers the `boxplot()` and `histplot()` functions to easily plot data from a pandas DataFrame. 

### Boxplot: 
```Python 
import matplotlib.pyplot as plt 
import seaborn as sns 

sns.boxpot(x='rent', data='rentals')
plt.show()
plt.close()
```

![[Pasted image 20240103200715.png]]


### Histogram:
```Python 
import matplotlib.pyplot as plt
import seaborn as sns 

sns.histplot(x='rent', data= rentals)
plt.show()
plt.close()
```

![[Pasted image 20240103200858.png]]

## 4. Value counts for Categorical Variables 
 a good way to summarize categorical variables is to generate a frequency table containing the count of each distinct value. For example, we may be interested to know how many of the New York City rental listings are from each borough. Related, we can also find which borough has the most listings.
 The `pandas` library offers the `.value_counts()` method for generating the counts of all values in a DataFrame column:
 
```Python
df.borough.value_counts()
```

```
Manhattan 3539 
Brooklyn 1013 
Queens 448
```

## 5. Value Proportions for Categorical Data
A counts table is one approach for exploring categorical variables, but sometimes it is useful to also look at the proportion of values in each category. For example, knowing that there are 3,539 rental listings in Manhattan is hard to interpret without any context about the counts in the other categories. On the other hand, knowing that Manhattan listings make up 71% of all New York City listings tells us a lot more about the relative frequency of this category.

We can calculate the proportion for each category by dividing its count by the total number of values for that variable:
```Python 
df.borough.value_counts(normalize = True)
```

## 6. Visualizing Categorical Variables
For categorical variables, bar charts and pie charts are common options for visualizing the count (or proportion) of values in each category. They can also convey the relative frequencies of each category.
Python’s `seaborn` library offers several functions that can create bar charts. The simplest for plotting the counts is `countplot()`:
```Python 
import seaborn as sns 
sns.countplot(x='borough', data=rentals)
plt.show()
plt.close()
```

![[Pasted image 20240103201602.png]]

here are currently no functions in the `seaborn` library for creating a pie chart, but the `pandas` library provides a convenient wrapper function around `matplotlib`‘s `pie()` function that can generate a pie chart from any column in a DataFrame:
```Python 
rentals.borough.value_counts().plot.pie()  
plt.show()  
plt.close()
```

![[Pasted image 20240103201722.png]]

In general, many data analysts avoid pie charts because people are better at visually comparing areas of rectangles than wedges of a pie. For a variable with a small number of categories a pie chart is a reasonable choice; however, for more complex data, a bar chart is usually preferable.

#Python