___
In predicting the price of a home, one factor to consider is the size of the home. The relationship between those two variables, price and size, is important, but there are other variables that factor in to pricing a home: location, air quality, demographics, parking, and more. When making predictions for price, our _dependent variable_, we’ll want to use multiple _independent variables_
Multiple Linear regression uses two or more independent variables to predict the values of the dependent variable. It is based on the equation given below:
$$ y = b + m_1x_1 + m_2x_2+....+m_nx_n$$

### Multiple Linear Regression with scikit-learn
We first split our data into training set and testing set, using the `train_test_split` method in the `selection_model` module in the `sklearn` library.
Now we have the training set and the test set, let’s use scikit-learn to build the linear regression model!
The steps for multiple linear regression in scikit-learn are identical to the steps for simple linear regression. Just like simple linear regression, we need to import `LinearRegression` from the `linear_model` module:

```Python 
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split


steeteasy = pd.read_csv("<insert csv file>")
print(df.head())
X = df[['bedrooms', 'bathrooms', 'size_sqft', 'min_to_subway', 'floor', 'building_age_yrs', 'no_fee', 'has_roofdeck', 'has_washer_dryer', 'has_doorman', 'has_elevator', 'has_dishwasher', 'has_patio', 'has_gym']]
print(X.head())

X_train, X_test, y_train, y_test = train_test_spit(X,y, test_size=0.8, random_state=1)
mlr = LinearRegression()
mlr.fit(X_train, y_train)

y_predict = mlr.predict(X_test)
coeff = mlr.coef_
intercept = mlr.intercept_
```

> We can also look into the coefficients and the intercept values, a greater `m` value would mean that the variable plays a  significant role in determining the relationship with the dependent variable. A negative or a positive sign would indicate a positive or a negative correlation. 


### Evaluating the Model's accuracy
- When trying to evaluate the accuracy of our multiple linear regression model, one technique we can use is **Residual Analysis**. The difference between the actual value _y_, and the predicted value _y' is the **residual _e_**. The equation is:
$$ e = y - y'$$
- The `sklearn`'s `linear_model.LinearRegression` comes with `.score()` method that returns the coefficient $R^2$  of the prediction. 
$$  R^2 = 1 - \frac{\sum(y-y')^2}{\sum (y - mean(y))^2}$$

R² is the percentage variation in y explained by all the x variables together.
For example, say we are trying to predict `rent` based on the `size_sqft` and the `bedrooms` in the apartment and the R² for our model is 0.72 — that means that all the x variables (square feet and number of bedrooms) together explain 72% variation in y (`rent`).
Now let’s say we add another x variable, building’s age, to our model. By adding this third relevant x variable, the R² is expected to go up. Let say the new R² is 0.95. This means that square feet, number of bedrooms and age of the building _together_ explain 95% of the variation in the rent.
==The best possible R² is 1.00 (and it can be negative because the model can be arbitrarily worse). Usually, a R² of 0.70 is considered good.==

