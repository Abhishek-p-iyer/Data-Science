____
Before fitting any model, it is often important to conduct an exploratory data analysis (EDA) in order to check assumptions, inspect the data for anomalies (such as missing, duplicated, or mis-coded data), and inform feature selection/transformation.

## The data
For our example analysis, we’ve downloaded a dataset from [Kaggle](https://www.kaggle.com/cyaris/2016-mlb-season?select=baseball_reference_2016_clean.csv) which contains data on Major League Baseball (MLB) games from the 2016 season. We’ve saved this data as a DataFrame named `bb`. Suppose that we want to fit a linear regression to predict `attendance` using the following predictors:

- `game_type` — is the game during the day or at night?
- `day_of_week` — what day of the week did the game occur?
- `temperature` — average game temperature (Fahrenheit).
- `sky` — description of sky condition at the time of the game.
- `total_runs` — total runs scored in the game.

### Preview the dataset
Any EDA process will probably begin by inspecting a subset of data. For a pandas `DataFrame`, this can be done by using the `.head()` method:
```Python 
bb.head()
```

![[Pasted image 20240129081541.png]]

By looking at the first few rows of the data, we can often figure out what kind of data we have (eg., discrete or continuous) and get a sense of how they are coded. For example, we can see that `attendance`, `temperature`, and `total_runs` are numbers, while `game_type`, `day_of_week`, and `sky` appear to be text.

After our initial inspection, we’ll want to dig deeper to investigate the following:

- The data type of each variable.
- How discrete/categorical data is coded (and whether we need to make any changes).
- How the data are scaled.
- Whether there is missing data and how it is coded.
- Whether there are outliers.
- The distributions of continuous features.
- The relationships between pairs of features.

### Datatypes
We use the `df.dtypes` command to check the datatypes of the dataframe. If we see something out of context, where we expect a column to have a datatype of `int64` or `float`, but instead has a datatype of `object`, we will need to continue further analysis. This is usually cause due to the presence of a string in the column. We can use the `replace()` method to replace the string values by `np.nan`. 

### Categorical Encoding 
EDA is also important during the feature engineering process in order to inform decisions around categorical encoding. This is important because categorical features with many levels are “expensive” to include in a regression model (we need to calculate a separate slope for each level). If one of the levels has only a few observations, we might want to delete those records from the data before fitting the model. We can check this using `.value_counts()`:
```Python
bb['game_type'].value_counts(dropna=False)
```
```
Night Game    1664  
Day Game       799  
Name: game_type, dtype: int64
```

Based on the output, we can see here that there are two levels for `game_type`; about one-third of games are day games and two-thirds are night games.
The `.value_counts()` accessor can also illuminate other issues. For example, in the following output, we notice that one instance of `'Tuesday'` was miscoded as `Tuesda`. This can either be corrected or removed before proceeding with a regression model.
```Python
bb['day_of_week'].value_counts(dropna=False)
```
```
Saturday     396  
Friday       394  
Sunday       392  
Wednesday    379  
Tuesday      375  
Monday       278  
Thursday     248  
Tuesda         1  
Name: day_of_week, dtype: int64
```

There are a few different options for how we might want to code the `day_of_week` variable. If attendance increases approximately linearly throughout the week, we might argue that `day_of_week` is ordinal and code it as an `int` in our model. However, attendance goes up and down throughout the week, we’re better off leaving it as an unordered category (`str`). Finally, if we see that games on Friday-Sunday simply have higher attendance than other days of the week, we might re-code this feature to only have two levels: `Weekend` and `Weekday`. We can check this by using boxplots:

![[Pasted image 20240129082109.png]]

We can see here that attendance on Friday, Saturday, and Sunday is on average higher than the other days of the week. Therefore it may be beneficial to re-code this feature to either `Weekend` or `Weekday`.

### Scaling 
For quantitative features, it is important to think about how each feature is scaled. Some features will be on vastly different scales than others just based on the nature of what the feature is measuring. For example, let’s look at `temperature` and `total_runs` using the `.describe()` method:
```Python
bb.describe()
```

The output will look like this:
```
         attendance   temperature   total_runs  
count   2457.000000  2457.000000    2457.000000  
mean     30380.462352   73.834959     8.949187  
std   9874.626652    10.567219    4.579542  
min   8766.000000    31.000000    1.000000  
25%   22437.000000  67.000000     6.000000  
50%   30628.000000  74.000000     8.000000  
75%   38412.000000  81.000000     12.000000  
max   54449.000000  101.000000   60.000000
```

These two features are on different scales because what they are measuring are different (`temperature` is in degrees Fahrenheit, `total_runs` is the number of runs scored in a game). Because of this, the ranges of values and the standard deviations for each are very different from one another. We can see here that `temperature` has a standard deviation of about 10.57, while `total_runs` has a standard deviation of about 4.58.

When working with features with largely differing scales, it is often a good idea to _standardize_ the features so that they all have a mean of 0 and a standard deviation of 1.

### Outliers
We use the scatterplot to plot outliers and we check if the outliers values have be correctly coded to the dataframe.

### Distributions and associations
Prior to fitting a linear regression model, it can be important to inspect the distributions of quantitative features and investigate the relationships between features. We can visually inspect both of these by using a pair plot:
![[Pasted image 20240129082618.png]]
Looking at the histograms along the diagonal, `total_runs` appears to be somewhat right-skewed. This indicates that we may want to transform this feature to make it more normally distributed.
We can explore the relationships between pairs of features by looking at the scatterplots off of the diagonal. This is useful for a few different reasons. For example, if we see non-linear associations between any of the predictors and the outcome variable, that might lead us to test out polynomial terms in our model. We can also get a sense for which features are most highly related to our outcome variable and check for collinearity. In this example, there appears to be a slight positive linear association between temperature and the total number of runs. We can further investigate this using a heat map of the correlation matrix:
![[Pasted image 20240129082821.png]]
There is a correlation of 0.35 between temperature and the total number of runs. This is not large enough to cause concern; however, if two or more predictors are highly correlated, we may consider leaving only one in our analysis. On the other hand, ==features that are highly correlated with our outcome variable are especially important to include in the model.==