# Feature Selection Methods
## Filter Methods 
[[Filter methods]] are a type of feature selection method that works by selecting features based on some criteria prior to building the model. Because they don’t involve actually testing the subset-ed features using a model, they are computationally inexpensive and flexible to use for any type of machine learning algorithm. This makes filter methods an efficient initial step for narrowing down the pool of features to only the most relevant, predictive ones.

## Wrapper Methods
Machine learning problems often involve datasets with many features. Some of those features might be very important for a specific machine learning model. Other features might be irrelevant. Given a feature set and a model, we would like to be able to distinguish between important and unimportant features. [[Wrapper Methods]] are feature selection algorithms that evaluate the model based on the subset of features and select the best features based on the performance of the model. 

## Regularization
[[Regularization]] plays a very important role in real world implementations of machine learning models. It is a statistical technique that minimizes overfitting and is executed during the model fitting step. It is also an embedded feature selection method because it is implemented while the parameters of the model are being calculated. 
Hands on code for regularization by [[Implementing Regularization]]

## Feature Importance 
[[Feature importance]]: When we fit a supervised machine learning (ML) model, we often want to understand which features are most associated with our outcome of interest. Features that are highly associated with the outcome are considered more “important.”

## Hyperparameter tuning 
A _hyperparameter_ of a machine learning model is a value that determines part of the learning process and is not affected by training unlike a _parameter_, which is learned during the model training process. The process of selecting an ideal hyperparameter is called [[Hyperparameter tuning]]
