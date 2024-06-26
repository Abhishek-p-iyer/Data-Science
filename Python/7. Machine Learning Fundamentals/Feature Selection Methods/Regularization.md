___
Machine learning algorithms are rarely deployed to production without using some form of regularization. The reason for this is as follows: In practice, every model has to deal with the question of how well it can generalize from known to unknown data. We can train, test and tune models on known data and make them as accurate as possible. However, in deploying models, we are applying them on new data. Regularization makes sure that our model is still accurate.

## What is overfitting?
If a model is able to represent a particular set of data points effectively but is not able to fit new data well, it is overfitting the data. Such a model has one or more of the following attributes:

- It fits the training data well but performs significantly worse on test data
- It typically has more parameters than necessary, i.e., it has _high model complexity_
- It might be fitting for features that are multi-collinear (i.e., features that are highly negatively or positively correlated)
- It might be fitting the noise in the data and likely mistaking the noise for features
In practice, we often catch overfitting by comparing the model performance on the training data versus test data. For instance if the R-squared score is high for training data but the model performs poorly on test data, it’s a strong indicator of overfitting.

Lets take an example of a multiple linear regression model. A dataset has been loaded from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Student+Performance) that describes the performance in mathematics of students from two Portuguese schools. We’re going to implement a multiple linear regression model to predict the final grade of the students based on a number of features in the dataset.
```Python
import pandas as pd
import numpy as np
import codecademylib3
import matplotlib.pyplot as plt

df = pd.read_csv("./student_math.csv")
print(df.head())

#setting target and predictor variables
y = df['Final_Grade']
X = df.drop(columns = ['Final_Grade'])

# 1. Number of features
num_features = len(X.columns)
print("Number of features: ",num_features)

#Performing a Train-Test split
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)
#Fitting a Linear Regression Model
from sklearn.linear_model import LinearRegression
model = LinearRegression()
model.fit(X_train, y_train)

#Training Error
pred_train = model.predict(X_train)
MSE_train = np.mean((pred_train - y_train)**2)
print("Training Error: ", MSE_train)

  

# 2. Testing Error
pred_test = model.predict(X_test)
MSE_test = np.mean((pred_test - y_test)**2)
print("Testing Error: ", MSE_test)

#Calculating the regression coefficients
predictors = X.columns
coef = pd.Series(model.coef_,predictors).sort_values()
# 3. Plotting the Coefficients

plt.figure(figsize = (15,10))
coef.plot(kind='bar', fontsize = 20)
plt.title ("Regression Coefficients", fontsize = 30)
plt.show()
```

## The loss function
Let’s revisit how the coefficients (or parameters) of a linear regression model are obtained. Suppose we want to fit a linear regression with two features, x1 and x2:
$$ y = b_0 + b_1x_1 + b_2x_2 + e $$

We solve for the intercept b0 and coefficients b1 and b2 by minimizing the mean squared error. This is also referred to as the **loss function** (or alternately, as the _objective function_ or _cost function_) in machine learning:
$$ Loss function = \frac{1}{n}\sum (y_i - (b_0 + b_1x_1 + b_2x_2))^2 $$

The loss function is minimized by the method of Ordinary Least Squares (OLS) and for larger datasets, the gradient descent algorithm is used to arrive at the minimum. As we have seen already however, the “best fit” as obtained by minimizing the loss function might not be the fit that generalizes well to new data. Regularization modifies the loss function in a way that might help with this.
Before going into more detail, we’re going to plot the loss function as a function of the coefficients b1 and b2. For this, we’re going to generate a dataset for two-feature linear regression using the `datasets` module from `scikit-learn`. The default data generated by this module is centered, meaning that our intercept, b0 is already set to zero.
```Python
import numpy as np
import matplotlib.pyplot as plt
from sklearn import datasets
data, y, coefficients = datasets.make_regression(n_samples = 100, n_features = 2, coef = True, random_state = 23)
x1 = data[:,0]
x2 = data[:,1] 

# 1. Print the coefficients
print(coefficients)

# 2. Loss Function
def loss_function(b1,b2,y,x1,x2):
    error = y - b1*x1 - b2*x2
    loss = np.mean(error**2)
    return loss
# 3. Plot loss function for data
from plot_loss import plot_loss_function
b1 = np.linspace(-150, 150, 501)
b2 = np.linspace(-150, 150, 501)
contour_plot = plot_loss_function(b1,b2,y,x1,x2)
plt.show()


# 4. Plot the best fit coefficients
best_fit_b1 = coefficients[0]
best_fit_b2 = coefficients[1]
plt.scatter(best_fit_b1, best_fit_b2, s = 50, color = 'green')
plt.show()
```

![[Pasted image 20240207101434.png]]

## The regularization term
Regularization penalizes models for overfitting by adding a “penalty term” to the loss function. The two most commonly used ways of doing this are known as _Lasso (or L1)_ and _Ridge (or L2)_ regularization. Both of these rely on penalizing overfitting by controlling how large the coefficients can get in the first place. The penalty term or regularization term is multiplied by a factor `alpha` and added to the old loss function as follows:
$$ New \ loss \ function = Old \ loss \ function \ + \ \alpha*Regularization \ term$$

Because of the additive term, minimizing the new loss function will necessarily mean “true loss” goes up, i.e., the scores on this will be lower on the training data than regression without regularization! But this is what _we want_ when we are regularizing. Remember that the reason we’re regularizing is because our model is overfitted to our data
![[Pasted image 20240207101909.png]]

The regularization term is the sum of the absolute values of the coefficients in the case of L1 regularization and the sum of the squares of the coefficients in the case of L2.
$$ L1 \ Regularization \ term : |b_1| + |b2|$$
$$ L2 \ Regularization \ term: b_1^2 + b_2^2$$
## L1 or Lasso Regularization  - two variables
In the case of the two-feature linear regression scenario we were looking at in the previous exercise, our new loss function for L1 regularization looks as follows:
$$ L1 \ Loss = \frac{1}{n}\sum (y_i - (b_0 + b_1x_1 + b_2x_2))^2 + \alpha*(|b_1|+|b_2|) $$

Minimizing this new loss function essentially means restricting the size of the regularization term while minimizing the old loss function. Let’s say that our regularization term can take a maximum value, `s`:
$$ |b_1| + |b_2| < s$$

This is equivalent to confining our coefficients to a surface around the origin bounded by 4 lines : `b1+b2 = s`, `b1-b2 = s`, `b2-b1 = s` and `-b1-b2 = s`.

![[Pasted image 20240207102324.png]]

The figure to the right replicates the elliptical contours for the loss function that we’d plotted earlier in the lesson. If we plot these four lines as with b1 and b2 as X and Y axes respectively, we get the diamond shaped blue shaded region that we’ve shown here. We have chosen a value of `s = 50` for this figure - this means that either coefficient can have a maximum value of 50. The choice of `s` is deeply tied to the choice of `alpha` as they are inversely related.

The value of (b1,b2) that satisfies the regularization constraint while minimizing the loss function is denoted by the white dot. Notice that it is exactly at the tip of the diamond where it intersects with a contour from our loss function. It also happens to fall exactly on the X axis, thus setting the value of b2 to 0!

Why is this the point that minimizes the new loss function? Remember that the goal here is to minimize the original loss function _while_ meeting the regularization constraint. We still want to be as close to the center of the contours (the original loss function minimum) as possible. The point that’s closest to the center of the contours _while_ simultaneously lying within the regularization surface boundary is the white dot!

## L1 or Lassos Regularization - Multiple variables
 The loss function for a multiple linear regression case with `m` features looks as follows: 
 $$L1 \ Loss = \frac{1}{n}\sum (y_i - (b_0 + b_1x_1 + b_2x_2 +...+b_mx_m))^2 + \alpha*(|b_1|+|b_2|+....+|b_m|)  $$
We’re going to examine this by reapplying a multiple linear regression model to the student performance dataset we were looking at earlier on in the lesson — only this time, we are going to do this with L1 regularization. A quick reminder that our original unregularized coefficients look as shown in the image below:![[Pasted image 20240207102855.png]]
We’ve loaded a file, `script.py` here containing the analysis we did earlier on. Some of the things we’d noted earlier on were:

- too many features possibly (from the number of columns in the DataFrame)
- testing error is higher than training error (by looking at the `MSE`)
- highly negatively correlated features (inferred from the plot of the coefficients)
```python
import pandas as pd
import numpy as np
import codecademylib3
import matplotlib.pyplot as plt
import helpers

df = pd.read_csv("./student_math.csv")
y = df['Final_Grade']
X = df.drop(columns = ['Final_Grade'])

# 1. Train-test split and fitting an l1-regularized regression model
from sklearn.model_selection import train_test_split
from sklearn.linear_model import Lasso
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)
lasso = Lasso(alpha = 0.1)
lasso.fit(X_train, y_train)

l1_pred_train = lasso.predict(X_train)
l1_mse_train = np.mean((l1_pred_train - y_train)**2)
print("Lasso (L1) Training Error: ", l1_mse_train)

# 2. Calculate testing error
l1_pred_test = lasso.predict(X_test)
l1_mse_test = np.mean((l1_pred_test - y_test)**2)
print("Lasso (L1) Test Error: ",l1_mse_test)

# 3. Plotting the Coefficients
predictors = X.columns
coef = pd.Series(lasso.coef_,predictors).sort_values()
plt.figure(figsize = (12,8))
plt.ylim(-1.0,1.0)
coef.plot(kind='bar', title='Regression Coefficients with Lasso (L1) Regularization')
plt.show()
```
![[Pasted image 20240207102756.png]]

Lasso has shrunk more than half our coefficients to zero! Additionally, while the value of all the coefficients have shrunk, Lasso has increased the relative importance of some. An important thing to note here is that we manually set the value of `alpha` to 0.1. The number of features that get eliminated due to Lasso is definitely tied to the value of `alpha`

## L2 or Ridge Regularization
L2 regularization is also called “Ridge” regularization as the mathematical formulation of it belongs to a class of analysis methods known as “ridge analysis”. The modified loss function in the case of L2 regularization looks as follows:
$$ L1 \ Loss = \frac{1}{n}\sum (y_i - (b_0 + b_1x_1 + b_2x_2))^2 + \alpha*(b_1^2+b_2^2) $$
Similar to L1, this would mean constraining the regularization term to the following surface:
$$ b_1^2 + b_2^2 < s^2$$

![[Pasted image 20240207103513.png]]

The key difference here is that instead of a diamond-shaped area, we’re constraining the coefficients to live within a circle of radius `s` as shown in the figure here. The general goal is still similar though, we want to minimize the old loss while restricting the values of the coefficients to the boundary of this circle. Once again we want our new coefficient values to be as close to the unregularized best fit solution (i.e., the center of the contours) as possible while falling within the circle.

The value of (b1,b2) that minimizes this new loss function almost never lies on either axes. The solution here is **not** the pink dot that lies on the X axis like in L1, but rather the white dot that we have shown. The reason for this is as follows: The circle that contains the white and pink dots represents the smallest value of the old loss function that satisfies the regularization constraint, but while the pink dot makes the regularization term’s value `s^2` exactly, the white dot makes it even smaller as it lies _inside_ the circle!

>[!info] Unlike Lasso, our Ridge coefficients can never be exactly zero! L2 regularization is therefore not a feature elimination method like L1. The coefficients of L2 get arbitrarily small but never zero. This is particularly useful when we don’t want to get rid of features during modeling but nonetheless want their relative importances emphasized.

We’re now going to apply L2 regularization on the student performance dataset we’ve been working with to see how it works in a many-feature scenario. The loss function for a Multiple Linear Regression model with L2 regularization with `m` features looks as follows: (You can use the horizontal scroll bar here to view the entire equation!)
$$ L1 \ Loss = \frac{1}{n}\sum (y_i - (b_0 + b_1x_1 + b_2x_2 +...+b_mx_m))^2 + \alpha*(b_1^2+b_2^2+....+b_m^2)  $$

We’re going to examine this by reapplying a multiple linear regression model to the student performance dataset we were looking at earlier on in the lesson — only this time, we are going to do this with L2 regularization. A quick reminder that our original unregularized coefficients look as shown in the image below:
![[Pasted image 20240207103648.png]]

```Python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import helpers

df = pd.read_csv("./student_math.csv")
y = df['Final_Grade']
X = df.drop(columns = ['Final_Grade'])

# 1. Train-test split and fitting an l2-regularized regression model
from sklearn.model_selection import train_test_split
from sklearn.linear_model import Ridge
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)
ridge = Ridge(alpha = 100)
ridge.fit(X_train, y_train)

#Training error
l2_pred_train = ridge.predict(X_train)
l2_mse_train = np.mean((l2_pred_train - y_train)**2)
print("Ridge (L2) Training Error: ", l2_mse_train)

# 2. Calculate testing error
l2_pred_test = ridge.predict(X_test)
l2_mse_test = np.mean((l2_pred_test - y_test)**2)
print("Ridge (L2) Test Error: ", l2_mse_test)
#print("Ridge (L2) Testing Error: ", l2_mse_test)
 
# 3. Plotting the Coefficients
predictors = X.columns
coef = pd.Series(ridge.coef_,predictors).sort_values()
plt.figure(figsize = (12,8))
plt.ylim(-1.0,1.0)
coef.plot(kind='bar', title='Regression Coefficients with Lasso (L1) Regularization')
plt.show()
```

![[Pasted image 20240207103746.png]]

## Hyperparameter tuning 
`alpha` is what is known as a _hyperparameter_ in machine learning. It is not learned during the model fitting step the way model parameters are. Rather, it’s chosen prior to fitting the model and is used to control the learning process. In regularization, the choice of `alpha` determines how much we want to control for overfitting during the model fitting step.

We’re able to do this by using `alpha` to control the size of the constraint surface and consequently the size of the coefficients themselves. The larger the `alpha`, the smaller the size of the allowed coefficients. In essence, `alpha` is **inversely proportional** to `s` in the case of L1 regularization or `s^2` in the case of L2 regularization.

- If `s` is very large (i.e., `alpha` is very small), the unregularized loss function minimum can easily fall within this large boundary and thus make it similar to regression without regularization.
    
- If `s` is very small (i.e., `alpha` is very large), the regression coefficients become very small and the loss value for the best fit becomes large making the regression over-regularized.
To avoid either of these extreme scenarios, we need to find the _right amount of regularization_ by tuning `alpha` and this process is known as **hyperparameter tuning**.

## Bias-Variance trade off
When we add the regularization term to our loss function, we are in essence introducing **bias** into our problem, i.e., we are biasing our model to have coefficients within the regularization boundary. The greater the `alpha`, the smaller the coefficients and the more biased the model.  For very high values of `alpha`, while Ridge begins to make most of the coefficients very small, Lasso ends up eliminating all but one feature!

Recall that the reason we wanted to perform regularization was to prevent our model from overfitting the data. Such a model is said to have high **variance** as it is likely fitting for random errors or noise within the data. We introduced the bias term to minimize the variance, but if we’re not careful and allow it to get arbitrarily large, we run the risk of underfitting the model!
Ideally we want a machine learning model to have low bias and low variance, i.e., we want it to perform well on training data as well as test data. However, trying to minimize bias and variance simultaneously is a bit of a conundrum as lowering one raises the other! This dilemma in machine learning models is known as the **bias-variance tradeoff**.
Hyperparameter tuning helps us find a sweet spot in this tradeoff to ensure that neither bias nor variance get too high. Typically, a portion of the data is set aside that is known as the “validation set” or “holdout set” (over and above the usual test-train split) and this is used to perform hyperparameter tuning on.


![[Pasted image 20240207104335.png]]