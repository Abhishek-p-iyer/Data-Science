___
## 1. Adding a Column
We can add a column to a dataframe in many different ways, 
- **Using a list**: We add a column similar to how we add a new value into  dictionary. The syntax for adding a column into a dataframe would be `df['<NewColName>'] = [<collection>]`. 
- **Using calculations**: We can use other columns in a dataframe to add a new column. 
```Python 
# Using a list 
df['Quantity'] = [100, 50, 50, 60, 20]

# The same row throughout the column
df['In Stock'] = True 

# Using calculations 
df['Tax'] = df.Price*0.075
```

## 2.Performing Column Operations on a DataFrame
Sometimes we want to manipulate the cells in a dataframe, for example, we might want to capitalize all the letters of a particular row. Here we use the apply function, that takes a particular column and applies the necessary function to each and very cell of a dataframe. We use the [[Lambda Functions]] to perform complex operations on a particular column in a dataframe. 
```Python 
df['Name'] = df.Name.apply(str.upper()) #Converts each cell to an uppercase value
```

- ### Applying Lambda function to a column
Example: Lets say we want to get the last name in a column, which contains the full name of each employee of a certain organization. 
```Python 
get_last_name = lambda x: x.split(' ')[-1]
df['last_name'] = df.Name.apply(get_last_name)
```


- ### Applying Lambda function to a row 
We can use a lambda function to calculate a column using other columns if we do not specify a column before the initializing the `apply()` and adding the argument `axis=1`. The input to the lambda function will be an entire row and not a column. To access a particular value of the row, we use a syntax `row.column_name` or `row[column_name]`. 
Lets consider the table below, 
![[Pasted image 20240102181415.png]]

We want to add a new column called `Price_with_Tax`, if the item is taxed, we add 7.5% to the price. 
```Python 
tax_funtion = lambda row: row.Price*1.075 if row['Is taxed'] == Yes else row.Price
df['Price_with_Tax'] = df.apply(tax_function, axis=1)
```

## 3. Renaming Columns 
We can either rename all the columns at once or we can rename specific columns. To rename all the columns at once using `.columns` property to a different list and we can rename specific columns using the `.rename()` method. 
```Python 
df = pd.DataFrame({  
    'name': ['John', 'Jane', 'Sue', 'Fred'],  
    'age': [23, 29, 21, 18]  
})  
# Using the .columns property
df.columns = ['First Name', 'Age']

# Using the .rename() method 
df.rename(columns={  
    'name': 'First Name',  
    'age': 'Age'},  
    inplace=True)
```

>Using `rename` with only the `columns` keyword will create a **new** DataFrame, leaving your original DataFrame unchanged. That’s why we also passed in the keyword argument **`inplace=True`**. Using `inplace=True` lets us edit the **original** DataFrame.

There are several reasons why `.rename` is preferable to `.columns`:

- You can rename just one column
- You can be specific about which column names are getting changed (with `.column` you can accidentally switch column names if you’re not careful)
#Python