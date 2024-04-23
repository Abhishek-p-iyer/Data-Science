___
## Introduction to Numpy and Pandas!
### Pandas
Pandas is a very popular and powerful library that data analysts and scientists use on a day to day basis due to its in-built functions that give us the ability to manipulate rows and columns, transform data and handle missing data with ease. A lot of SQL functions can be performed using pandas. 

## Numpy
The Numpy library facilitates efficient numerical operations on large quantities of data. The most important part about NumPy is that pandas is built on top of it. So, NumPy is a dependency of Pandas.
### Numpy Arrays 
The `numpy` arrays are more flexible than normal Python lists. They allow for faster elemental access and efficient data manipulation. There are called nd arrays since they can have any number (n) of dimensions (d). They hold a collection of items of any one data type and can either be a vector or a matrix. 
Mathematical operators can be performed on all values in a nd array at one time rather than having to loop through the values. 
Many operations can be performed on a Numpy array which makes them very helpful for manipulating data. 
- Selecting array elements 
- Slicing arrays 
- Reshaping arrays 
- Splitting arrays 
- Combining arrays 
- Applying numerical operators (min, max, mean, etc)


## Pandas - Series and Dataframes
### Series in Pandas 
Just how the nd arrays forms the foundation of numpy, series forms a basis for Pandas. A series is similar to a one-dimensional NumPy array with some additional functionalities which allows the index to be labeled. ==It contains only one datatype.== This is very convenient if we have a piece of information such as Age of an employee, it will be easier to link the age with the id of the employee. 
```Python
ages = [20, 27, 32]
series = pd.Series(ages, index=[123, 124, 125])
```

### Dataframe in Python
A dataframe is similar to a matrix of an nd array. It consists of rows and columns, both of which can be indexed by strings of numerical values. ==One dataframe contains different types of data but a column can only have a single datatype.==  A column of a dataframe is essentially a series. 
There are different ways in which we can import a dataframe into python, we could write a SQL query, a CSV file or even a python list or dictionary can be converted into a dataframe. 
```Python 

df = pd.DataFrame(["John","Petrucci",23],["John", "Callus", 34],["Jade","Winslet", 35], columns= ["first name", "last name", "Age"])
```

By default, the row indices range from 0 to the `len(df)-1`. If we want to change the indices from the default numerical values to a column name, we use the function `set_index()`. 
```Python 
df.set_index("first_name")
```
DataFrames are useful because they make it much easier to select, manipulate, and summarize data. Their tabular format (a table with rows and columns) also makes it easier to label, simpler to read, and easier to export data to and from a spreadsheet.

## Hands on with Pandas
Now that we have understood what a dataframe is, we will now learn how to create, load, select data with pandas. The [[Introduction to a DataFrame]] will work with the basics of manipulating single tables in pandas, creating a table from scratch, loading data from another file and selecting certain rows or columns of a table. We will also look into [[Modifying a DataFrame]] where we add columns, use lambda functions and rename columns. 
[[Aggregates in Pandas]] goes in depth into different ways we can describe our dataframe. Common aggregate statistics include mean, median, and standard deviation. An important topic of pivoting a table is also discussed. 
In order to store information efficiently, we spread data across multiple tables. [[Multiple Tables in Python]] will deal with merging, concatenating different tables. 


#Python
