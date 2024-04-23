____
## Initial Data Inspection 
Before analysis or cleaning, it is always useful to print a few rows of data. This helps us to compare the data with the data dictionary and make sure that the data is properly loaded. 
```Python 
import pandas as pd  
heart = pd.read_csv('processed.cleveland.data.csv')  
print(heart.head())
```

## Data Information 
Once we've taken a first look at the data, a common step to address is to ask a couple of questions such as:
- How many unique columns and features do we have in our dataset?
- What are the number of columns that contain missing data? 
- What are the data types of each column?

Using the pandas `.info()` method we can easily address these questions. 
```Python 
print(heart.info())
```

```
<class 'pandas.core.frame.DataFrame'>  
RangeIndex: 303 entries, 0 to 302  
Data columns (total 14 columns):  
#   Column         Non-Null Count  Dtype    
---  ------         --------------  -----    
0   age            303 non-null    float64  
1   sex            303 non-null    float64  
2   cp             303 non-null    float64  
3   trestbps       303 non-null    float64  
4   chol           303 non-null    float64  
5   fbs            303 non-null    float64  
6   restecg        303 non-null    float64  
7   thalach        303 non-null    float64  
8   exang          303 non-null    float64  
9   oldpeak        303 non-null    float64  
10  slope          303 non-null    float64  
11  ca             303 non-null    object  
12  thal           303 non-null    object  
13  heart_disease  303 non-null    int64    
dtypes: float64(11), int64(1), object(2)  
memory usage: 33.3+ KB
```

Sometimes, numerical values can be assigned a datatype of string or an object, a couple of common reasons why this may occur is due to the fact that there are mis-coded data in that particular column. We can use the `.replace()` method to replace all the mid-coded data values into  `np.NaN` value. 


## Inspecting Missing Data 
After identifying the columns that contain missing data, we have certain methods that we use to find out why the data is missing, based on which we can either proceed to delete the missing values or we could either use the average or a particular value to replace all of them. 
```Python 
heart[heart.isnull().any(axis=1)]
```


More resources: (Summary Statistics and Visualization)[https://www.youtube.com/watch?v=YwadRm2sfpQ&t=454s]


#Python