_Deletion_ is, quite simply, when we remove some aspect of our missing data so that our resulting dataset is as complete as possible, leading to accurate analytics. Since missing data does not provide a complete picture of what happened in our observations, we can’t rely on it for analytics, hence why deleting data can be a good solution.
## When is it safe to use Deletion

There are some scenarios where data can be deleted without looking back at the deleted data and there are other scenarios where the data that is being deleted may add bias to the analysis. It would lead to the data not being representative of the whole population. Hence it is vital to know when we can delete the missing data and we should not. 
The data that are either MAR or MACR can be deleted because we are assuming that they are missing due to random reasons. Before we delete the missing data, we also need to consider the amount of data being deleted. We cannot delete a major chunk of our data.


## Types of Deletion
There are two kinds of ways we can delete data from our dataframe:
- Listwise Deletion
- Pairwise Deletion 

### Listwise Deletion 
We employ the listwise deletion when we want to delete the entire row of data. Entire rows of data missing from any columns are deleted.  This particular technique is usually employed when the missing variable(s) will directly impact the analysis we are trying to perform, usually with respect to MAR or MCAR missing data.
```Python 
df = df.dropna(inplace=True)
df.head()
```
In general, we should be cautious when using listwise deletion, as we lose a lot of information when we remove an entire row. When we remove rows like this, we decrease the amount of data that we can use for analysis. This means we would have less confidence in the accuracy of any conclusions we draw from the resulting dataset.

>As a best practice, we should only use listwise deletion when the number of rows with missing data is relatively small to avoid significant bias. Every dataset will have a different context for how much data is safe to remove. A safe place to start is assuming that if less than 5% of data is missing, then we are safe to use listwise deletion.

## Pairwise Deletion
Pairwise deletion is the alternative to listwise deletion and we use this when we want to delete data from certain columns. Suppose we have missing data on three columns, but we're analyzing only two of the three columns, hence we will drop the missing values from only these two columns. This will help us retain more data in general and increase the confidence of our analysis. 
```Python 
df = df.dropna(subset["col1","col2"], inplace="True", how='any')
```

 In general, pairwise deletion is the preferred technique to use.
## Dropping Variables
There is another tactic we could employ: to drop a variable entirely. If a variable is missing enough data, then we really don’t know enough about that variable to use it in our analysis, so we can’t be confident in any conclusions we draw from it.

While this may sound easier than the other solutions mentioned and possibly effective, we generally don’t want to drop entire variables. Why? In most contexts, having more data is always a good idea and we should work to retain it if possible. The more data we have, the more confidence we can have that our conclusions are actually happening, and not due to random chance. We should only drop a variable as a last resort, and if that variable is missing a very significant amount of data (at least 60%).


#Python 
