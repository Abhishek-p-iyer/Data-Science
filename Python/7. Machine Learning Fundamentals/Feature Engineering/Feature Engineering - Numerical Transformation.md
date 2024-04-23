____
This process is called _numerical transformation_, when we take our numerical data and change it into another numerical value. This is meant to change the scale of our values or even adjust the skewness of our data. 
We will focus on
- Centering our data
- Standard scaler
- Min and Max Scaler
- Binning
- Log transformations

## 1. Centering Data
Data centering involves subtracting the mean of a data set from each data point so that the new mean is 0. This process helps us understand how far above or below each of our data points is from the mean. To center our data, we first find the mean of the data and subtract each value from the mean. 
```Python
distance = coffee['nearest_starbucks']
#get the mean of your feature  
mean_dis = np.mean(distance)  
  
#take our distance array and subtract the mean_dis, this will create a new series with the results  
centered_dis = distance - mean_dis  
  
#visualize your new list  
plt.hist(centered_dis, bins = 5, color = 'g')  
  
#label our visual  
plt.title('Starbucks Distance Data Centered')  
plt.xlabel('Distance from Mean')  
plt.ylabel('Count')  
plt.show();
```

![[Pasted image 20240129124302.png]]

## 2. Standardizing Data
Standardization or Z-score normalization is used when we want the mean to be 0 and the standard deviation to be 1. We first center our data and divide the data by its standard deviation. This allows all of our features to be on the same scale.
This step is critical because some machine learning models will treat all features equally regardless of their scale. You’ll definitely want to standardize your data in the following situations:

- Before Principal Component Analysis
- Before using any clustering or distance based algorithm (think KMeans or DBSCAN)
- Before KNN
- Before performing regularization methods like LASSO and Ridge

$$ z = \frac{(value - mean)}{standard \ deviation} $$

#### Manually Standardizing Data
```Python
distance = coffee['nearest_starbucks']  
#find the mean of our feature  
distance_mean = np.mean(distance)  
#find the standard deviation of our feature  
distance_std_dev = np.std(distance)     
#this will take each data point in distance subtract the mean, then divide by the standard deviation  
distance_standardized = (distance - distance_mean) / distance_std_dev
# print what type distance_standardized is  
print(type(distance_standardized))  
#output = <class 'pandas.core.series.Series'>  
#print the mean  
print(np.mean(distance_standardized))  
#output = 7.644158530205996e-17  
#print the standard deviation  
print(np.std(distance_standardized))  
#output = 1.0000000000000013
```


#### Standardizing our Data with `Sklearn`
```Python
from sklearn.preprocessing import StandardScaler  
scaler = StandardScaler()
reshaped_distance = np.array(distance).reshape(-1,1)  
distance_scaler = scaler.fit_transform(reshaped_distance)
print(np.mean(distance_scaler))  
#output = -9.464196275493137e-17  
print(np.std(distance_scaler))  
#output = 0.9999999999999997
```
We instantiate the StandardScaler by setting it to a variable called `scaler` which we can then use to transform our feature. The next step is to reshape our distance array. StandardScaler must take in our array as 1 column, so we’ll reshape our distance array using the `.reshape(-1,1)` method. This numpy method says to take our data and give it back to us as 1 column, represented in the second value. The -1 asks numpy to figure out the exact number of rows to create based on our data.

## 3. Min-Max Normalization
Another form of scaling your data is to use a min-max normalization process. The name says it all, we find the minimum and maximum data point in our entire data set and set each of those to 0 and 1, respectively. Then the rest of the data points will transform to a number between 0 and 1, depending on its distance between the minimum and maximum number. We find that transformed number by taking the data point subtracting it from the minimum point, then dividing by the value of our maximum minus minimum.
$$ X_n = \frac{X - X_(min)}{X_(max) - X_(min)} $$
>[!info] One thing to note about min-max normalization is that this transformation does not work well with data that has extreme outliers. You will want to perform a min-max normalization if the range between your min and max point is not too drastic.

```Python
from sklearn.preprocessing import MinMaxScaler  
mmscaler = MinMaxScaler()
#get our distance feature  
distance = coffee['nearest_starbucks']  
#reshape our array to prepare it for the mmscaler  
reshaped_distance = np.array(distance).reshape(-1,1)  
#.fit_transform our reshaped data  
distance_norm = mmscaler.fit_transform(reshaped_distance)  
#see unique values  
print(set(np.unique(distance_norm)))
```

## 4. Binning our Data
Binning data is the process of taking numerical or categorical data and breaking it up into groups. We could decide to bin our data to help capture patterns in noisy data. There isn’t a clean and fast rule about how to bin your data, but like so many things in machine learning, you need to be aware of the trade-offs.
You want to make sure that your bin ranges aren’t so small that your model is still seeing it as noisy data. Then you also want to make sure that the bin ranges are not so large that your model is unable to pick up on any pattern. It is a delicate decision to make and will depend on the data you are working with.

![[Pasted image 20240129130310.png]]

We can easily see that a lot of customers who completed this survey live fairly close to a Starbucks, and our data has a range of 0 km to 8km. I wonder how our data would transform if we were to bin our data in the following way:

- distance < 1km
- 1.1km <= distance < 3km
- 3.1km <= distance < 5km
- 5.1km <= distance

First, we’ll set the upper boundaries of what we listed above.
```Python
bins = [0, 1, 3, 5, 8.1]
coffee['binned_distance'] = pd.cut(coffee['nearest_starbucks'], bins, right = False)  
  
print(coffee[['binned_distance', 'nearest_starbucks']].head(3))  
  
#output  
#  binned_distance  nearest_starbucks  
#0      [5.0, 8.1)                  8  
#1      [5.0, 8.1)                  8  
#2      [5.0, 8.1)                  8
```

We can see that those who marked 8 km now live in the \[5.0, 8.1) bucket. The bracket `[` tells us 5.0 is included, and the parenthesis `)` tells us that 8.1 is excluded. We could write it as an inequality statement like this: `5 <= distance < 8.1` this allows our customers who marked 8 to belong to the ‘Lives greater than 5.1 km’ bin. Now let’s have a look at our newly binned data.
```Python
# Plot the bar graph of binned distances  
coffee['binned_distance'].value_counts().plot(kind='bar')  
  
# Label the bar graph  
plt.title('Starbucks Distance Distribution')  
plt.xlabel('Distance')  
plt.ylabel('Count')  
  
# Show the bar graph  
plt.show()
```

![[Pasted image 20240129130444.png]]

## 5. Natural Log transform
Logarithms are an essential tool in statistical analysis and machine learning preparation. This transformation works well for right-skewed data and data with large outliers. After we log transform our data, one large benefit is that it will allow the data to be closer to a “normal” distribution. It also changes the scale so our data points will drastically reduce the range of their values.

For example, let’s explore a whole new data set from Kaggle around used car prices. Take a look at this histogram plot of 100,000 used car odometers.
```Python
import pandas as pd  
  
#import our dataframe  
cars = pd.read_csv('cars.csv')  
  
#set our variable  
odometer = cars['odometer']  
  
#graph our odometer readings  
plt.hist(odometer, bins = 200, color = 'g')  
  
#add labels  
plt.xticks(rotation = 45)  
plt.title('Number of Cars by Odometer Reading')  
plt.ylabel('Number of Cars')  
plt.xlabel('Odometer')  
plt.show()
```

![[Pasted image 20240129130633.png]]

This histogram is right-skewed, where the majority of our data is located on the left side of our graph. If we were to provide this feature to our machine learning model it will see a lot of different cars with odometer readings off on the left of our graph. It will not see a lot of examples with very high odometer readings. This may cause issues with our model, as it may struggle to pick up on patterns that are within those examples off on the right side of our histogram.
We’ll perform a log transformation using numpy to see how our data will transform.
```Python
import numpy as np  
#perform the log transformation  
log_car = np.log(cars['odometer'])  
#graph our transformation  
plt.hist(log_car, bins = 200, color = 'g')  
#rotate the x labels so we can read it easily  
plt.xticks(rotation = 45)  
#provide a title  
plt.title('Logarithm of Car Odometers')  
plt.show();
```

![[Pasted image 20240129130723.png]]


1. Using a log transformation in a machine learning model will require some extra interpretation. For example, if you were to log transform your data in a linear regression model, our independent variable has a multiplication relationship with our dependent variable instead of the usual additive relationship we would have if our data was not log-transformed.
2. Keep in mind, just because your data is skewed does not mean that a log transformation is the best answer. You would not want to log transform your feature if:
3. You have values less than 0. The natural logarithm (which is what we’ve been talking about) of a negative number is undefined.    
4. You have left-skewed data. That data may call for a square or cube transformation.
5. You have non-parametric data


