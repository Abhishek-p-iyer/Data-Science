____

## Introduction
A _hyperparameter_ of a machine learning model is a value that determines part of the learning process and is not affected by training unlike a _parameter_, which is learned during the model training process.

In the k-nearest neighbors algorithm, k is a hyperparameter. k determines how many neighbors will be used, and training data does not change k. Decision trees also use hyperparameters. Before training a decision tree classifier, you might want to specify how deep the tree can go (i.e., how many splits are allowed before arriving at a leaf). You might also want to specify the minimum number of samples that are present in a node in order to split that node. Both of those values are hyperparameters. They determine the structure of the model, but they are not learned during training.

## Why is it vital?
Hyperparameters can make or break a machine learning model. Take the k-nearest neighbors algorithm as an example. Recall that in k-nn classification, the class of a sample is determined by the classes of its k nearest neighbors from the training data. If k is too large, classes with a small number of samples might be missed, and the model could underfit the data. If k is too small, it’s possible that outliers in the training data could misclassify their neighbors. The model could be overfit to those outliers.


This balance between overfitting and underfitting is called the _bias-variance tradeoff_. It’s an important consideration for supervised machine learning.
- _Bias_ is the difference between a model’s predictions and the correct values. Models with a lot of bias underfit the data and will perform poorly on both testing and training data. In biased models, the structure of the model overpowers the training data.
- _Variance_ refers to the dependence of a model on training data. A model has high variance if different training data results in widely varying outcomes. This often leads to overfitting a model to training data. Overfit, high-variance models generally perform well on training data, but poorly on testing data. In this case, the training data overpowers the structure of the model.
We want hyperparameters that make a good compromise between bias and variance. This can be done by training a model multiple times, each time with different hyperparameter values, and then using the best ones. This process is called _hyperparameter tuning_.

## Common Hyperparameter tuning methods
- The _grid search_ algorithm for hyperparameter tuning works by training a model on predetermined lists of hyperparameter values. This method tries every hyperparameter value on the list, and then uses the one that makes the model perform best.
- The _random search_ algorithm works similarly, but instead of using a predetermined list of hyperparameter values, the values are randomly chosen. As with grid search, it selects the hyperparameter that performed the best.
- _Bayesian optimization_ is another approach to hyperparameter tuning. It uses ideas from the field of Bayesian statistics to iterate through different hyperparameter values. Each time the Bayesian optimization algorithm evaluates a new hyperparameter value, it gains more information about where it should look for the best hyperparameter value.
- _Genetic algorithms_ are another possible hyperparameter tuning method. These work by going through several generations of hyperparameter values. Within each generation, the fittest (i.e., best-performing) hyperparameter values are slightly mutated (i.e., changed) in order to produce the next generation.

>When tuning hyperparameters, it’s important to split the data into training, testing, and validation data. Training data is used to train the model. Validation data is used to evaluate hyperparameters. After a hyperparameter is tuned, the model can be tested on testing data. This data allows for an estimate of model performance that isn’t affected by the hyperparameter tuning process.

![[Pasted image 20240208072551.png]]

## Implementation
To understand the implementation of different methods of hyperparameter tuning, we need to choose a dataset, a classification or regression problem we’d like to solve, and a machine learning model to solve it with. The image on the right-hand side lists some of the commonly used machine learning models and their corresponding hyperparameters. Our choices for the rest of the lesson are as follows:

- _Dataset and Model_: We’re going to work with the commonly used [breast cancer data set](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_breast_cancer.html) that is available with `scikit-learn`. The prediction task is to classify tumors as benign or malignant and the data has 30 numeric predictor variables. We’re going to use [logistic regression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html) to perform this task.
    
- _Hyperparameters_: So, which hyperparameters do we tune? There are many arguments to `scikit-learn`‘s logistic regression function and many of them can be treated as hyperparameters and tuned.

### Introduction to Grid Search Method
 Grid search works by testing a model on a list of hyperparameter values decided upon beforehand. Suppose we had two hyperparameters we wanted to tune and we wanted to choose between 6 values for the first one and 5 values of the second, we’d be searching a grid of thirty values as shown below. Grid search would fit the model and evaluate its performance for each of the values represented by these points. We can then choose the hyperparameter values corresponding to the best performance and conclude our hyperparameter search!
 ![[Pasted image 20240208073055.png]]
In our case, we want to make a decision on the following:
- The type of logistic regression being using, either Lassos (L1) or Ridge (L2) Regression. 
- The penalty value, `c` which is the inverse of regularization strength. We would like to test three possible values 1, 10, 100.

We will now have 6 cases to test. `GridSearchCV` in `sklearn` lets us do this. The parameters we give for the function is a dictionary with the name of the parameter and the possible values as a list.

```Python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import GridSearchCV

# Load the data set
cancer = load_breast_cancer()

# Split into training and testing data
X_train, X_test, y_train, y_test = train_test_split(cancer.data, cancer.target)
lr = LogisticRegression(solver='liblinear', max_iter=1000)

#check output
parameters = {'penalty': ['l1', 'l2'], 'C': [1, 10, 100]}


clf = GridSearchCV(estimator=lr, param_grid=parameters)
#Scheck output
print(clf.get_params())

## OUTPUT
{'cv': None, 'error_score': nan, 'estimator__C': 1.0, 'estimator__class_weight': None, 'estimator__dual': False, 'estimator__fit_intercept': True, 'estimator__intercept_scaling': 1, 'estimator__l1_ratio': None, 'estimator__max_iter': 1000, 'estimator__multi_class': 'auto', 'estimator__n_jobs': None, 'estimator__penalty': 'l2', 'estimator__random_state': None, 'estimator__solver': 'liblinear', 'estimator__tol': 0.0001, 'estimator__verbose': 0, 'estimator__warm_start': False, 'estimator': LogisticRegression(max_iter=1000, solver='liblinear'), 'n_jobs': None, 'param_grid': {'penalty': ['l1', 'l2'], 'C': [1, 10, 100]}, 'pre_dispatch': '2*n_jobs', 'refit': True, 'return_train_score': False, 'scoring': None, 'verbose': 0}

```

### Evaluating the Results of `GridSearchCV`
After fitting a `GridSearchCV` model we can find out the results using the following attributes of the `clf` argument:

- `.best_estimator_` gives us the best estimator
	- `.best_score_` gives us the mean cross-validated score corresponding to the best estimator
- `.best_params_` gives us the set of hyperparameters that correspond to the best estimator

Additionally, the `.cv_results_` attribute gives us the scores for each hyperparamter combination in the grid. We’re now ready to evaluate the grid search we set up earlier and we’ve preloaded the code from the previous exercise in the Setup cell.

```Python
clf.fit(X_train, y_train)
best_model = clf.best_estimator_
print(best_model)
print(clf.best_params_)
#OUTPUT
LogisticRegression(C=10, max_iter=1000, penalty='l1', solver='liblinear')
{'C': 10, 'penalty': 'l1'}

best_score = clf.best_score_
test_score = clf.score(X_test, y_test)
print(best_score)
print(test_score)
# OUTPUT
0.9647606019151846
0.951048951048951

hyperparameter_grid = pd.DataFrame(clf.cv_results_['params'])
grid_scores = clf.cv_results_['mean_test_score']
grid_scores = pd.DataFrame(grid_scores, columns=['score'])
df = pd.concat([hyperparameter_grid, grid_scores], axis = 1)
print(df)
```

```
     C penalty     score
0    1      l1  0.955349
1    1      l2  0.952996
2   10      l1  0.964761
3   10      l2  0.957702
4  100      l1  0.957674
5  100      l2  0.960027
```

### Introduction to Random Search Method
Instead of using a list of pre-determined values in the case of the grid search method, we select some `n` number of random values. For example, for hyperparameter `c` in logistic regression, instead of selecting a pre-determined list of 3 values we select 3 random values from 1-100. 
In `scikit-learn`, the `RandomSearchCV` function implements random searches with cross-validation. There are three arguments that need to be specified. 
- `estimator`: The ML model whose hyperparameters we are going to tune 
- `param_distribution`: A dictionary which specifies the parameters as keys and the corresponding distribution to draw a list of values
- `n_iter`: The number of times the algorithm needs to randomly draw from the distribution. The default value is 10. 
When we used grid search, we made a list of different hyperparameter values. With random search, we don’t have to make a list, but we still have to provide some information about how we want to select random numbers. Do we want random numbers between 0 and 100? Between -10 and 10? Do we want the same chance of picking small numbers and picking large numbers?
We can do this by specifying a probability distribution for each hyperparameter.
```Python
from scipy.stats import uniform  
distributions = {'penalty': ['l1', 'l2'], 'C': uniform(loc=0, scale=100)}
```

Let’s take a closer look at each distribution:

- The `penalty` hyperparameter of scikit-learn’s `LogisticRegression` model has only two possible values: `l1` and `l2`. We list them both. `RandomizedSearchCV` will treat this as a _discrete uniform distribution_. This just means that every item in the list has an equal chance of being selected. In this case, there’s a 50% chance of drawing `l1` and a 50% chance of drawing `l2`.
- The hyperparameter `C` is the inverse of regularization strength. It can be any positive number, so we have to specify a probability distribution that allows us to randomly select a positive number. The `scipy` library has many probability distributions to choose from ( [here](https://docs.scipy.org/doc/scipy/reference/stats.html)).  For this example, we’re using the _uniform distribution_. This allows us to randomly select numbers between `loc` and `loc + scale` (in this case, between 0 and 100).
```Python
import pandas as pd
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import RandomizedSearchCV
from scipy.stats import uniform

# Load the data set
cancer = load_breast_cancer()

# Split the data into training and testing sets
X = cancer.data
y = cancer.target
X_train, X_test, y_train, y_test = train_test_split(X, y)
distributions = {'penalty':['l1','l2'], 'C': uniform(loc=0, scale=100)}
lr = LogisticRegression(solver = 'liblinear', max_iter=1000)
clf = RandomizedSearchCV(lr, distributions,n_iter=8
```

#### Evaluating the Results of `RandomizedSearchCV`
We can now follow a similar process to what we did with `GridSearchCV` to evaluate the results of `RandomizedSearchCV`.
After fitting a `RandomizedSearchCV` model we can find out the results using the following attributes of the `clf` argument:
- `.best_estimator_` gives us the best estimator
- `.best_score_` gives us the mean cross-validated score corresponding to the best estimator
- `.best_params_` gives us the set of hyperparameters that correspond to the best estimator
Additionally, the `.cv_results_` attribute gives us the scores for each hyperparameter combination in the grid. We’re now ready to evaluate the random search we set up earlier and we’ve preloaded the code from the previous exercise in the Setup cell.

```Python
clf.fit(X_train, y_train)
best_model = clf.best_estimator_
print(best_model)
print(clf.best_params_)

#OUTPUT
LogisticRegression(C=68.15310608970896, max_iter=1000, penalty='l1',
                   solver='liblinear')
{'C': 68.15310608970896, 'penalty': 'l1'}

best_score = clf.best_score_
test_score = clf.score(X_test, y_test)
print(best_score)
print(test_score)
#OUTPUT
0.9600547195622434
0.958041958041958


hyperparameter_values = pd.DataFrame(clf.cv_results_['params'])
randomsearch_scores = pd.DataFrame(clf.cv_results_['mean_test_score'])
df = pd.concat([hyperparameter_values, randomsearch_scores], axis = 1)
print(df)
```

```
C penalty 0 
0 83.146052 l1 0.957729 
1 68.153106 l1 0.960055 
2 28.791332 l1 0.953023 
3 24.544303 l1 0.957729 
4 63.729938 l2 0.957729 
5 37.020544 l2 0.955349 
6 76.028097 l1 0.960055 
7 22.335915 l2 0.953023
```



