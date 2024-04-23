___
## 1. Creating a DataFrame
There are a couple of ways from which we can create a DataFrame in Python 
- Using a dictionary
``` Python
import pandas as pd 

df = pd.DataFrame({"Name": ["Abhi", "John", "Jack"], 
				   "Age":[12, 34, 16], 
				   "Color":["Blue", "Red", "Yellow"]})
```
- Using a List
```Python
import pandas as pd 

df = pd.DataFrame([
				   ["Abhi", "John", "Jack"],
				   [12, 34, 16],
				   ["Blue", "Red", "Yellow"]
				   ],
				   columns = ["name", "Age", "Color"]
)
```

## 2. Loading and saving a CSV file 
- Loading a CSV file as a dataframe
```Python
import pandas as pd
pd.read_csv('<filename.csv>')
```

- Converting a dataframe to a csv file 
```Python
import pandas as pd
df.to_csv('<filename.csv>')
```

## 3. Inspecting a DataFrame
We can inspect the first few rows of the dataframe by using the `.head(5)` function in pandas. 
```Python
import pandas as pd 
df.head(5)
df.head(10)
```

## 4. Selecting Columns 
There are two ways in which we can select columns in a dataframe. 
- We can select the column like we were selecting a value from a dictionary. The sytanx for which would be `df_name['<col_name>']`.
- We can use another method to select a column if the column name does not contain any special characters, spaces or doesn't start with a number. We can this use `df.columnName` to select columns. 
```Python
Col1 = df["col_name"]
Col2 = df.ColName
# Selecting Multiple Columns 
Cols = df[["col1", "col2", "col3"]]
```
>[!info] Selecting Multiple Columns 
>When we have a larger dataframe and just want to select a couple of rows into the new dataframe we enclose the names of the column in double brackets. 

## 5. Selecting rows 
To select rows from a dataframe we could i) either use the `.iloc()` function; ii) we could use logical statements, the syntax for which is `df[df.col == desired_col_value]`. ; iii)We can use the `isin` statement to get the rows we desire. 
> When we select a single row, the result is a _Series_ (just like when we select a single column).
```Python
df = pd.DataFrame([
  ['January', 100, 100, 23, 100],
  ['February', 51, 45, 145, 45],
  ['March', 81, 96, 65, 96],
  ['April', 80, 80, 54, 180],
  ['May', 51, 54, 54, 154],
  ['June', 112, 109, 79, 129]],
  columns=['month', 'clinic_east',
           'clinic_north', 'clinic_south',
           'clinic_west'])
# Using the iloc function
march = df.iloc[2]
jan_feb_march = df.iloc[:4]
March_April_May = df.iloc[3:6]

# Using logic
march = df[df.month == "March"]
# Using multiple logical statements
March_Or_June = df[(df.month == "March") | (df.month == "June")]

# Using isin command
Jan_Feb_March = df[df.name.is(["January", "February", "March"])]
```



> When we copy the rows of the dataframe into another dataframe, the index of the previous dataframe is also copied with it. If we want to reset the index our new dataframe, we use the `.reset_index()` function. This will create a new separate column in our new dataframe containing the previous indices. If we do not require the new column to be added, we can add a parameter called drop and initialize it to True. `.reset_index(drop=True)` 

#Python