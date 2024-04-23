____

### Introduction
A linear regression model tries to fit a line when we're given with two different quantitative data such that the loss of each of the data points to the line is minimum. 
A line is determined by its slope and intercept value, when we take the line formula 
$$ y = mx \ + \ b$$

where `m` is the slope, and `b` is the intercept. `y` is a given point on the y-axis, and it corresponds to a given `x` on the x-axis.
The slope is a measure of how steep the line is, while the intercept is a measure of where the line hits the y-axis.
When we perform Linear Regression, the goal is to get the “best” `m` and `b` for our data. We will determine what “best” means in the next exercises.

### Loss
We calculate the distance between each data point and the line and we take the sum of all the error values, if we find a error value that is less than that of the current error value, we consider that line to be better than the earlier one. The goal of a linear regression model is to find the slope and intercept pair that minimizes loss on average across all of the data.

## Gradient Descent:
As we are trying to minimize the loss, we take each parameter that we are changing and we move it as long as our loss decreases the most, once it stops decreasing we can use this value as our loss is minimized the most for this value. This process is called ==gradient descent==
### Gradient Descent for Intercept 
The equation for gradient descent for the intercept is:
$$ min\_intercept = \frac{-2}{N}\sum(y_i \ - (mx_i+b))$$

- `N` is the number of points we have in our dataset
- `m` is the current gradient guess
- `b` is the current intercept guess
### Gradient Descent for Slope
We have a function to find the gradient of `b` at every point. To find the `m` gradient, or the way the loss changes as the slope of our line changes, we can use this formula:
$$ m_min = \frac{-2}{N}\sum x_i(y_i - (mx_i+b))$$

Once we have a way to calculate both the `m` gradient and the `b` gradient, we’ll be able to follow both of those gradients downwards to the point of lowest loss for both the `m` value and the `b` value. Then, we’ll have the best `m` and the best `b` to fit our data!
##### Updating our current slop and intercept values
Now that we have our gradient, we need to take a step in that direction, hence, we will use a learning rate and update our current b and m values. 

```Python
new_b = current_b - (learning_rate * b_gradient)
new_m = current_m - (learning_rate * m_gradient)
```

## Convergence 
We stop changing our `m` and `b` values when we know that our model is reaching convergence. Convergence is reached when the loss stops changing when the parameters are changed. 
![[Pasted image 20240202102152.png]]


### Learning Rate
We have to choose a **learning rate**, which will determine how far down the loss curve we go.
A small learning rate will take a long time to converge — you might run out of time or cycles before getting an answer. A large learning rate might skip over the best value. It might _never_ converge!

> Finding the absolute best learning rate is not necessary for training a model. You just have to find a learning rate large enough that gradient descent converges with the efficiency you need, and not so large that convergence never happens.

## Putting it all together 
```Python
import matplotlib.pyplot as plt
def get_gradient_at_b(x, y, b, m):
  N = len(x)
  diff = 0
  for i in range(N):
    x_val = x[i]
    y_val = y[i]
    diff += (y_val - ((m * x_val) + b))
  b_gradient = -(2/N) * diff  
  return b_gradient

def get_gradient_at_m(x, y, b, m):
  N = len(x)
  diff = 0
  for i in range(N):
      x_val = x[i]
      y_val = y[i]
      diff += x_val * (y_val - ((m * x_val) + b))
  m_gradient = -(2/N) * diff  
  return m_gradient

#Your step_gradient function here
def step_gradient(b_current, m_current, x, y, learning_rate):
    b_gradient = get_gradient_at_b(x, y, b_current, m_current)
    m_gradient = get_gradient_at_m(x, y, b_current, m_current)
    b = b_current - (learning_rate * b_gradient)
    m = m_current - (learning_rate * m_gradient)
    return [b, m]

#Your gradient_descent function here:  

months = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
revenue = [52, 74, 79, 95, 115, 110, 129, 126, 147, 146, 156, 184]

def gradient_descent(x,y,learning_rate, num_iterations):
  b = 0
  m = 0
  for i in range(num_iterations):
    b,m = step_gradient(b,m,x,y,learning_rate)
  return b,m

#Uncomment the line below to run your gradient_descent function
b, m = gradient_descent(months, revenue, 0.01, 1000)
#Uncomment the lines below to see the line you've settled upon!
y = [m*x + b for x in months]
plt.plot(months, revenue, "o")
plt.plot(months, y)
plt.show()
```


## Linear Regression with Scikit - Learn
 We can use Python’s scikit-learn library. Scikit-learn, or `sklearn`, is used specifically for Machine Learning. Inside the `linear_model` module, there is a `LinearRegression()` function we can use. We can then use the `LinearRegression` model and fit the data. 
 The `fit()` method gives us two variables that are useful:
 1. The `line_fitter.coef_` which contains the slope
 2. The `line_fitter.interept_` which contains the intercept
We can also use the `predict()` function to pass x-values and receive the y-values.
```Python
from sklearn.linear_model import LinearRegression
line_fitter = LinearRegression()
line_fitter.fit(X,y)
y_predicted = line_fitter.predict(X)
```

>[!info] We would need to reshape our data sometimes. The syntax for which is `X_value.reshape(-1,1)`


## Project - Honey production 

```Python
import codecademylib3_seaborn
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from sklearn import linear_model

df = pd.read_csv("https://content.codecademy.com/programs/data-science-path/linear_regression/honeyproduction.csv"
print(df.head())
prod_per_year = df.groupby('year').totalprod.mean().reset_index()
X = prod_per_year.year
X = X.values.reshape(-1,1)
y = prod_per_year.totalprod
regr = linear_model.LinearRegression()
regr.fit(X,y)
print(regr.coef_[0])

  

X_future = np.array(range(2013,2051))
X_future = X_future.reshape(-1,1)
y_predict = regr.predict(X)
future_predict = regr.predict(X_future)
plt.scatter(X,y)
plt.plot(X,y_predict)
plt.show()
plt.clf()
plt.plot(X_future,future_predict)
plt.show()
x_2050 = 2050
x_2050 = np.array(x_2050)
x_2050 = x_2050.reshape(-1,1)
y_2050 = regr.predict(x_2050)
print(y_2050)
```
