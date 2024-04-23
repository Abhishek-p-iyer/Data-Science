___
# Data Wrangling
Let’s look at a subset of restaurant inspections from the [New York City Department of Health and Mental Hygiene](https://data.cityofnewyork.us/Health/DOHMH-New-York-City-Restaurant-Inspection-Results/43nn-pn8j) (NYC DOHMH) to work through some data wrangling processes. The data includes seven different columns with information about a restaurant’s location and health inspection. Here is a description of the dataset’s variables.
```
Pos.    Var. Name                 Var. Description  
-----   --------------------      --------------------------------------  
0       DBA                       Restaurant name  
1       BORO                      Borough  
2       CUISINE DESCRIPTION       Type of cuisine  
3       GRADE                     Letter grade  
4       LATITUDE                  Latitude coordinates of restaurant  
5       LONGITUDE                 Longitude coordinates of restaurant  
6       URL                       URL link to restaurant's website
```

Let’s use the `read_csv()` function in pandas to load our dataset as a pandas dataframe and take a look at the first 10 rows out of the 27 total.

```Python
import pandas as pd  
restaurants = pd.read_csv("DOHMH_restaurant_inspections.csv")  
  
# the .head(10) function will show us the first 10 rows in our dataset  
print(restaurants.head(10))
```

![[Pasted image 20240116064129.png]]

```Python 
# the .shape method in pandas identifies the number of rows and columns in our dataset as (rows, columns)  
restaurants.shape
```

```
(27, 7)
```

When we look closely at the table, we see some missing data. In both `GRADE` and `URL` columns, we have missing values marked as NaNs. In the `Latitude` and `Longitude` columns, we have a missing set of coordinates marked as (0.000, 0.000) for IHOP. (0.000, 0.000) is the label for missing coordinates because no restaurants in New York City are at the equator. Other common indicators used for missing values are values that are NA and -.

## Preliminary Data Cleaning 

### Dropping Duplicates
We first look into the duplicates, we drop any duplicates in the table if they're present. 
We use the `.drop_duplicates()` function. 
```Python 
restaurants = restaurants.drop_duplicates()
print(restaurants.shape())
```

```
(25, 7)
```

### Renaming and Formatting Columns 
We can see that five out of seven of the names of the columns are in upper case whereas two of the sever are in lower case, we will use the `map()` and `lower()` functions to make sure there is consistency in our naming convention.  
```Python 
restaurants.columns = map(str.lower(), restaurants.columns)
print(restaurants.head())
```

![[Pasted image 20240116072256.png]]

You may have noticed that the first column of the dataset is called `DBA`, but we know it is a column with restaurant names. We can use the `rename()` function and a dictionary to relabel our columns. While we are renaming our columns, we might also want to shorten the `cuisine description` column to just `cuisine`.

```Python 
restaurants = restaurants.rename({"dba":"name", "cuisine description":"cuisine"})
```

## Data Types
```Python 
restaurants.dtypes
```

```
name         object  
boro         object  
cuisine      object  
grade        object  
latitude     float64  
longitude    float64  
url          object  
dtype: object
```

We have two types of variables: object and float64. `object` can consist of both strings or mixed types (both numeric and non-numeric), and `float64` are numbers with a _floating point_ (ie. numbers with decimals). There are other data types such as `int64` (integer numbers), `bool` (True/False values), and `datetime64` (date and/or time values).

Since we have both continuous (float64) and categorical (object) variables in our data, it might be informative to look at the number of unique values in each column using the `nunique()` function.
```Python 
# .nunique() counts the number of unique values in each column  
restaurants.nunique()
```

```
name         25  
boro          4  
cuisine      15  
grade         1  
latitude     24  
longitude    24  
url          16
```

We see that our data consists of 4 boroughs in New York and 15 cuisine types. We know that we also have missing data in `url` from our initial inspection of the data, so the unique number of values in `url` might not be super informative. Additionally, we have corrected for duplicate restaurants, so the restaurant name, latitude, longitude, and url should be unique to each restaurant (unless, of course, there are some restaurant chains at different locations).

## Missing Data
From our initial inspection of the data, we know we have missing data in `grade`, `url`, `latitude`, and `longitude`. Let’s take a look at how the data is missing, also referred to as _missingness_. To do this we can use `isna()` to identify if the value is missing. This will give us a boolean and indicate if the observation in that column is missing (True) or not (False). We will also use `sum()` to count the number of missing values, where `isna()` returns True.

```Python
print(restaurants.isna().sum())
```

```
name          0  
boro          0  
cuisine       0  
grade        15  
latitude      0  
longitude     0  
url           9
```

We see that there are missing values in `grade` and `url`, but no missing values in `latitude` and `longitude`. However, we cannot have coordinates at (0.000, 0.000) for any of the restaurants in our dataset, and we saw that these exist in our initial analysis. Let’s replace the (0.000,0.000) coordinates with NaN values to account for this. We will use the `where()` function to replace the coordinates 0.000 with `np.nan`.

```Python 
restaurants['latitude'] = restaurants['latitude'].where(restaurants['latitude'] < 40, np.nan)
restaurants['longitude'] = restaurants['longitude'].where(restaurants['longitude']>-70, np.nan)
restaurants.isna().sum()
```

```
name          0  
boro          0  
cuisine       0  
grade        15  
latitude      2  
longitude     2  
url           9
```


#### Characterizing missingness with crosstab
Let’s try to understand the missingness in the `url` column by counting the missing values across each borough. We will use the `crosstab()` function in pandas to do this.

The `crosstab()` computes the frequency of two or more variables. To look at the missingness in the `url` column we can add `isna()` to the column to identify if there is an NaN in that column. This will return a boolean, True if there is a NaN and False if there is not. In our crosstab, we will look at all the boroughs present in our data and whether or not they have missing url links.
```Python
pd.crosstab(  
  
        # tabulates the boroughs as the index  
        restaurants['boro'],    
  
        # tabulates the number of missing values in the url column as columns  
        restaurants['url'].isna(),  
  
        # names the rows  
        rownames = ['boro'],  
  
        # names the columns  
        colnames = ['url is na'])
```

```
url is na   False   True  
boro          
Bronx         1   1  
Brooklyn       2      4  
Manhattan     11      2  
Queens       2    2
```

## Removing prefixes

It might be easier to read what url links are by removing the prefixes of the websites, such as "https://www". We use the `str.lstrip()` to remove the prefixes. Similar to when we were working with our column names, we need to make sure to include the `str` function to identify that we are working with strings and `lstrip` to remove parts of the string from the left side.
```Python
restaurants['url'] = restaurants.url.str.lstrip('https://')
restaurants['url'] = restaurants.url.str.lstrip('www.')
```

![[Pasted image 20240116074000.png]]

Amazing! Our dataset is now much easier to read and use. We have identifiable columns and variables that are easy to work with thanks to our data wrangling process. We also corrected illogical data values and made the strings a little easier to read.


# Tidy Data
Let’s take a look at a dataset that has information about the average annual wage for restaurant workers across New York City boroughs and New York City as a whole from the years 2000 and 2007. The data is from the [New York State Department of Labor](https://labor.ny.gov/stats/ins.asp), Quarterly Census of Employment and Wages,and only contains six total rows.

```Python
annual_wage = pd.read_csv("annual_wage_restaurant_boro.csv")  
print(annual_wage)
```

![[Pasted image 20240116074230.png]]

There are three variables in this dataset: `borough`, `year`, and `average annual income`. However, we have values (2000 and 2007) in the column headers rather than variable names (`year` and `average annual income`). This is not ideal to work with, so let’s fix it! We will use the `melt()` function in pandas to turn the current values (2000 and 2007) in the column headers into row values and add `year` and `avg_annual_wage` as our column labels.

```Python
annual_wage=annual_wage.melt(  
  
      # which column to use as identifier variables  
      id_vars=["boro"],  
        
      # column name to use for “variable” names/column headers (ie. 2000 and 2007)  
      var_name=["year"],  
  
      # column name for the values originally in the columns 2000 and 2007  
      value_name="avg_annual_wage")  
  
print(annual_wage)
```

![[Pasted image 20240116074319.png]]

Now we have a tidy dataset where each column is a variable (borough, year, or average annual wage), and each row is an observation! This dataset will be much easier to work with moving forward!

#Python 