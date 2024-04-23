___
When we receive raw data, we have to do a number of things before we’re ready to analyze it, possibly including:

- diagnosing the “tidiness” of the data — how much data cleaning we will have to do
- reshaping the data — getting right rows and columns for effective analysis
- combining multiple files
- changing the types of values — how we fix a column where numerical values are stored as strings, for example
- dropping or filling missing values - how we deal with data that is incomplete or missing
- manipulating strings to represent the data better

## Diagnose the Data 
We often describe data that is easy to analyze and visualize as “tidy data”. What does it mean to have tidy data?
For data to be tidy, it must have:
- Each variable as a separate column
- Each row as a separate observation

We have seen some of the useful functions to diagnose the data, some of them are: 
- `.head()` — display the first 5 rows of the table
- `.info()` — display a summary of the table
- `.describe()` — display the summary statistics of the table
- `.columns` — display the column names of the table
- `.value_counts()` — display the distinct values for a column

## Dealing with Multiple files
Often, you have the same data separated out into multiple files.

Let’s say that we have a ton of files following the filename structure: `'file1.csv'`, `'file2.csv'`, `'file3.csv'`, and so on. The power of pandas is mainly in being able to manipulate large amounts of structured data, so we want to be able to get all of the relevant information into one table so that we can analyze the aggregate data.
We can combine the use of `glob`, a Python library for working with files, with pandas to organize this data better. `glob` can open multiple files by using regex matching to get the filenames:
```Python
import glob  
files = glob.glob("file*.csv")  
df_list = []  
for filename in files:  
  data = pd.read_csv(filename)  
  df_list.append(data)  
df = pd.concat(df_list)  
print(files)
```

## Reshaping the data
We would need to reshape our data in such a way that each variable has a separate column and each row has a separate observation. We can use `pd.melt()` to transform our data.  


We would want to reshape a table like:
![[Pasted image 20240117082815.png]]

Into a table that looks more like:
![[Pasted image 20240117082835.png]]

We can use `pd.melt()` to do this transformation. `.melt()` takes in a DataFrame, and the columns to unpack:
```Python 
df = pd.melt(id_vars = "Account", value_vars = ["Checking", "Saving"], value_name = "Amount", var_name="Account Type")
```

The parameters you provide are:

- `frame`: the DataFrame you want to `melt`
- `id_vars`: the column(s) of the old DataFrame to preserve
- `value_vars`: the column(s) of the old DataFrame that you want to turn into variables
- `value_name`: what to call the column of the new DataFrame that stores the values
- `var_name`: what to call the column of the new DataFrame that stores the variables

## Dealing with Duplicates 
Often we see duplicated rows of data in the DataFrames we are working with. This could happen due to errors in data collection or in saving and loading the data.
To check for duplicates, we can use the pandas function `.duplicated()`, which will return a Series telling us which rows are duplicate rows.

We can use the pandas `.drop_duplicates()` function to remove all rows that are duplicates of another row. This will remove all the duplicates but will not remove every row with a duplicate value, for ex: Lets consider a fruit seller dataframe containing two values of "apple" with the same price and two values of "peach" with different prices. Now if we want to drop our duplicate values only, i.e: the second occurrence apple but not peach, then we use `.drop_duplicates()` but if we want to drop both apple and peach, we use `.drop_duplicates(subset=[item])`.

## Splitting by index
Let’s say we have a column “birthday” with data formatted in MMDDYYYY format. In other words, “11011993” represents a birthday of November 1, 1993. We want to split this data into day, month, and year so that we can use these columns as separate features.

In this case, we know the exact structure of these strings. The first two characters will always correspond to the month, the second two to the day, and the rest of the string will always correspond to year. We can easily break the data into three separate columns by splitting the strings using `.str`:
```Python
# Create the 'month' column  
df['month'] = df.birthday.str[0:2]  
  
# Create the 'day' column  
df['day'] = df.birthday.str[2:4]  
  
# Create the 'year' column  
df['year'] = df.birthday.str[4:]
```

This would transform a table like:
![[Pasted image 20240117083830.png]]
into a table like:
![[Pasted image 20240117083845.png]]

## Splitting by Character
Let’s say we have a column called “type” with data entries in the format `"admin_US"` or `"user_Kenya"`. Just like we saw before, this column actually contains two types of data. One seems to be the user type (with values like “admin” or “user”) and one seems to be the country this user is in (with values like “US” or “Kenya”).
We can no longer just split along the first 4 characters because `admin` and `user` are of different lengths. Instead, we know that we want to split along the `"_"`. Using that, we can split this column into two separate, cleaner columns:

```Python
# Split the string and save it as `string_split`  
string_split = df['type'].str.split('_')  
  
# Create the 'usertype' column  
df['usertype'] = string_split.str.get(0)  
  
# Create the 'country' column  
df['country'] = string_split.str.get(1)
```
![[Pasted image 20240117084110.png]]

## String Parsing 
Sometimes we need to modify strings in our DataFrames to help us transform them into more meaningful metrics. For example, in our fruits table from before:

![[Pasted image 20240117084757.png]]

We can see that the `'price'` column is actually composed of strings representing dollar amounts. This column could be much better represented in floats, so that we could take the mean, calculate other aggregate statistics, or compare different fruits to one another in terms of price.
First, we can use what we know of regex to get rid of all of the dollar signs:
```Python
fruit.price = fruit.price.replace('[\$,]','',regex=True)
```

Then, we can use the pandas function `.to_numeric()` to convert strings containing numerical values to integers or floats:
```Python
fruit.price = pd.to_numeric(fruit.price)
```

![[Pasted image 20240117084810.png]]

## More String Parsing
Sometimes we want to do analysis on numbers that are hidden within string values. We can use regex to extract this numerical data from the strings they are trapped in. Suppose we had this DataFrame `df` representing a workout regimen:
![[Pasted image 20240117085048.png]]

It would be helpful to separate out data like `"30 lunges"` into 2 columns with the number of reps, `"30"`, and the type of exercise, `"lunges"`. Then, we could compare the increase in the number of lunges done over time, for example.
To extract the numbers from the string we can use pandas’ `.str.split()` function:
```Python
split_df = df['exerciseDescription'].str.split('(\d+)', expand=True)
```

which would result in this DataFrame `split_df`:
![[Pasted image 20240117085131.png]]

Then, we can assign columns from this DataFrame to the original `df`:
```Python
df.reps = pd.to_numeric(split_df[1])  
df.exercise = split_df[0].replace('[\- ]', '', regex=True)
```

Now, our `df` looks like this:
![[Pasted image 20240117085210.png]]

### Missing Values
There are two ways in which we can handle missing values in Python
- Drop all the rows with missing values: We can either drop all the incomplete rows or we could just drop the incomplete rows of a specific column. The function for which is `df.dropna(subset=<col_name>)`
- Fill the missing values with the aggregate value: We can use `df.fillna()` to fill the missing values with an aggregate of the column. It could be mean, median or any other aggregate.

#Python 