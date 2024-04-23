___
Before we dive into single imputation, we need to understand what kind of data can be imputed. We need to make sure that the data that we're going to impute are MNAR. 
### Is it MNAR?
we must verify that our missing data can be categorized as MNAR — these techniques assume that to be the case. It can be tricky to understand that the data is missing in a non-random manner. How exactly can we verify this? There are two key aspects to be able to accurately describe missing data as MNAR:

1. **Use domain knowledge**: Probably the quickest way to identify MNAR is by having extensive knowledge of the data and industry we are working in. Missing data that falls into MNAR will look and feel different from “normal” data in our dataset.  
      
    With domain knowledge, either from ourselves or someone on our team, we could identify the cause for missing data. For example, someone might know that data in a survey is missing in a particular column because the participant was either too embarrassed to answer, or didn’t know the answer. This would let us know that the data is MNAR.
2. **Analyze the dataset to find patterns**: As we explore, filter, and separate our data, we should ultimately be able to identify something about our missing data that doesn’t line up with everything else. For example, if we have some survey data, we might find that our missing data almost exclusively comes from men older than 55 years old. If we see a pattern like this emerge, we can confidently say it is MNAR.


When our dataset has a value for every minute, and we are missing one value. The best way to handle this missing data is by using the data around it to “fill in the blanks”. This is called _single imputation_, and there are many ways that we can tackle this problem.

## Last Observation Carried Forward 
The first imputation we're going to deal with is the _Last Observation Carried Forward_. This deals with filling the missing data with a preceding value in the table. LOCF is used often when we see a relatively consistent pattern that has continued to either increase or decrease over time.

In Python, there are a variety of methods we can employ:
1. If your data is in a pandas DataFrame, we can use the `.ffil()` method on a particular column:
```Python
df["col_name"].ffill(axis=0, inplace=True)
# Applying Forward Fill (another name for LOCF) on the comfort column
```

2. If our data is in a NumPy array, there is a commonly used library called [impyute](https://impyute.readthedocs.io/en/master/index.html) that has a variety of time-series functions. To use LOCF, we can write the following code:
```Python
impyute.imputation.ts.locf(data, axis=0)
# Applying LOCF to the dataset
```

## Next Observation Carried Forward 
The _Next observation carried forward_ is another imputation which is similar to LOCF but fills the missing value with the next observation. NOCB is usually used when we have more recent data, and we know enough about the past to fill in the blanks that way.

Similarly to LOCF, there are a couple common techniques we can use:

1. If your data is in a pandas DataFrame, when we can use the `.bfil()` method on a particular column. By “back-filling” (another name for NOCB) our data, we will take data from the next data point in the time series and carry it backwards.
```Python
df['comfort'].bfill(axis=0, inplace=True)
```
2.  To use `impyute` to perform NOCB, we can write the following code
```Python 
impyute.imputation.ts.nocb(data, axis=0)
```

## Baseline Observation Carried Forward 
In this approach, the initial values for a given variable are applied to missing values. This is a common approach in medical studies, particularly in drug studies. For example, we could assume that missing data for a reported pain level could return to the baseline value. This would happen if a drug were not working as well, so it could be a safe assumption to make if the data is trending in that direction.

Since we are trying to measure the effect of a drug on something in the body, we don’t want to assume that LOCF or NOCB will apply here, as that could introduce significant error in our analysis. Instead, we could employ BOCF, which could look something like this:
```Python
# Isolate the first (baseline) value for our data  
baseline = df['concentration'][0]  
# Replace missing values with our baseline value  
df['concentration'].fillna(value=baseline, inplace=True)
```
## Worst Case Carried Forward 
Another alternative for single imputation is WOCF, or _Worst Observation Carried Forward_. With this kind of imputation, we want to assume that the data is the worst possible value. This would be useful if the purpose of our analysis was to record improvement in some value (for example, if we wanted to study if a treatment was helping a particular patient’s condition). By filling in data with the worst value, then we can reduce potentially biased results that didn’t actually happen.
```Python
# Isolate worst pain value (in this case, the highest)  
worst = df['pain'].max()  
# Replace all missing values with the worst value  
df['pain'].fillna(value=worst, inplace=True)
```

>[!info] There are disadvantages to using single imputation
>- This process adds bias to our dataset. The data will often change in unexpected ways.
>- Single imputation will also ignore the potential changes and will smooth out our data, which could lead to inaccurate results .


#Python 

