___

## 1. Inner merge
- When we want to merge two dataframes over a column, we use the `.merge()` method. 
- In addition to the `.merge()` method, each dataframe has its own `.merge()` method which we use while trying to merge more than two dataframes.

> While dealing with inner merge, the new dataframe only includes the matched columns. The mismatched row values are ignored.

```Python
# Inner merge I
new_df = pd.merge(df1, df2)

# Inner merge II
new_df = df1.merge(df2).merge(df3)
```

## 2. Outer merge
An outer merge would include all kinds of data, the data values that match as well as the data values that do not. 
```Python 
new_df = pd.merge(df1,df2, how='outer')
```

## 3. Left Merge 
Left merge copies every data point from the left table and only the matched rows from the right table. 
```Python 
new_df = pd.merge(df1, df2, how='left')
```
## 4. Right Merge 
The right merge is similar to the left merge, but instead copies all the values from the right table and the matched data points of the left table. 
```Python 
new_df = pd.merge(df1, df2, how='right')
```

## 5. Concatenate Dataframe
When we want to reconstruct a dataframe that has been broken into several parts, we use the `concat()` method of Pandas. 
```Python 
new_df = pd.concat([df1, df2, df3,...])
```
>The `concat()` method works only if the columns are the same in all of the DataFrames

#Python