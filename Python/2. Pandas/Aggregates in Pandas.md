___
## Calculating a Column Statistic 
The general syntax for calculating column statistic are:  `df.col_name.command()`. There are several commands build-into pandas. 
- `mean`: calculations the mean of the column
- `std`: calculates the standard deviation of the column
- `median`: calculates the median of the column
- `min`:  returns the minimum value in the column
- `max`: returns the maximum value in the column 
- `count`: calculates the number of values in the column 
- `nunique`: calculates the number of unique values in the column 
- `unique`: lists the unique values in the column 
#### Group by in Python 
There general syntax for using the group by clause: `df.groupby('col1').col2.measurment()`. 
Suppose we have a table consisting of `Student`, `assignment_name`, `grade`.  We want to calculate the average grade for each student. 
![[Pasted image 20240102185952.png]]

```Python 
grades = df.groupby('student').grade.mean().reset_index()
```

![[Pasted image 20240102190624.png]]

Sometimes, we want to group by more than one column. We can easily do this by passing a list of column names into the `groupby` method. 
Imagine that we run a chain of stores and have data about the number of sales at different locations on different days:
We suspect that sales are different at different locations on different days of the week. In order to test this hypothesis, we could calculate the average sales for each store on each day of the week across multiple months. The code would look like this:
```Python 
df.groupby(['Location', 'Day of Week'])['Total Sales'].mean().reset_index()
```
The results might look something like this:
![[Pasted image 20240102190859.png]]


## Pivot Tables 
Reorganizing a table in this way is called **pivoting**. The new table is called a **pivot table**.

When we perform a `groupby` across multiple columns, we often want to change how our data is stored. For instance, recall the example where we are running a chain of stores and have data about the number of sales at different locations on different days:
![[Pasted image 20240102191140.png]]
We suspected that there might be different sales on different days of the week at different stores, so we performed a `groupby` across two different columns (`Location` and `Day of Week`). This gave us results that looked like this:
![[Pasted image 20240102191201.png]]
In order to test our hypothesis, it would be more useful if the tables were formatted like this: 
![[Pasted image 20240102191248.png]]

In Pandas, the command for pivot is:
```Python 
df.pivot(column = 'Columns to pivot',
		index = 'Columns to be rows', 
		values = 'Columns to be values')
```

For our specific example, we would write the command like this:
```Python 
pivoted = unpivoted.pivot(  
    columns='Day of Week',  
    index='Location',  
    values='Total Sales')
```

#Python