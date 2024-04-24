___
We've seen that decision trees can be powerful supervised ML algorithms but they are not without their weakness. Decision trees are often prone to overfitting. We’ve discussed some strategies to minimize this problem, like pruning, but sometimes that isn’t enough. We need to find another way to generalize our trees. This is where the concept of a random forest comes in handy.

A random forest is an _ensemble machine learning technique_. A random forest contains many decision trees that all work together to classify new points. When a random forest is asked to classify a new point, the random forest gives that point to each of the decision trees. Each of those trees reports their classification and the random forest returns the most popular classification. It’s like every tree gets a vote, and the most popular classification wins. Some of the trees in the random forest may be overfit, but by making the prediction based on a large number of trees, overfitting will have less of an impact.
![[Pasted image 20240208183631.png]]


Decision trees are said to be deterministic, that is for a given training set, the same decision tree will be made. To make a random forest, we will use a technique called bagging, which is short for bootstrap aggregating. Bootstrapping is a kind of sampling method done with replacement. Every time a decision tree is made, it is created using a different subset of the points in the training set. For example, if our training set had `1000` rows in it, we could make a decision tree by picking `100` of those rows at random to build the tree. This way, every tree is different, but all trees will still be created from a portion of the training data.

The process of Bagging  starts with creating a single decision tree on a bootstrapped sample of data points in the training set. Then after many trees have been made, the results are “aggregated” together. In the case of a classification task, often the aggregation is taking the majority vote of the individual classifiers. For regression tasks, often the aggregation is the average of the individual regressors.

```Python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn import tree
from sklearn.metrics import accuracy_score

df = pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/car/car.data', names=['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'accep'])
df['accep'] = ~(df['accep']=='unacc') #1 is acceptable, 0 if not acceptable
X = pd.get_dummies(df.iloc[:,0:6], drop_first=True)
y = df['accep']
x_train, x_test, y_train, y_test = train_test_split(X,y, random_state=0, test_size=0.25)


#1.Decision tree trained on  training set
dt = DecisionTreeClassifier(max_depth = 5)
dt.fit(x_train, y_train)
print(f'Accuracy score of DT on test set (trained using full set): {dt.score(x_test, y_test).round(4)}')

#2. New decision tree trained on bootstrapped sample
dt2 = DecisionTreeClassifier(max_depth=5)
#ids are the indices of the bootstrapped sample
ids = x_train.sample(x_train.shape[0], replace=True, random_state=0).index
dt2.fit(x_train.loc[ids],y_train[ids])
print(f'Accuracy score of DT on test set (trained using bootstrapped sample): {dt2.score(x_test, y_test).round(4)}')

  

## 3. Bootstapping ten samples and aggregating the results:
preds = []
random_state= 0
#Write for loop:
for i in range(10):
    ids = x_train.sample(x_train.shape[0], replace=True, random_state=random_state+i).index
    dt2.fit(x_train.loc[ids], y_train[ids])
    preds.append(dt2.predict(x_test))  
ba_pred = np.array(preds).mean(0)

# 4. Calculate accuracy of the bagged sample
ba_accuracy = accuracy_score(ba_pred>=0.5, y_test)
print(f'Accuracy score of aggregated 10 bootstrapped samples:{ba_accuracy.round(4)}')
```

```
Accuracy score of DT on test set (trained using full set): 0.8588
Accuracy score of DT on test set (trained using bootstrapped sample): 0.8912
Accuracy score of aggregated 10 bootstrapped samples:0.9097
```

## Bagging using `sckit-learn`
```Python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier, BaggingClassifier, RandomForestRegressor
from sklearn.metrics import accuracy_score

df = pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/car/car.data', names=['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'accep'])
df['accep'] = ~(df['accep']=='unacc') #1 is acceptable, 0 if not acceptable
X = pd.get_dummies(df.iloc[:,0:6], drop_first=True)
y = df['accep']
x_train, x_test, y_train, y_test = train_test_split(X,y, random_state=0, test_size=0.25)

# 1. Bagging classifier with 10 Decision Tree base estimators
bag_dt = BaggingClassifier(DecisionTreeClassifier(max_depth = 5), n_estimators=10 )
bag_dt.fit(x_train, y_train)
bag_accuracy = bag_dt.score(x_test, y_test)
print('Accuracy score of Bagged Classifier, 10 estimators: {}'.format(bag_accuracy))
# 2.Set `max_features` to 10.
bag_dt_10 = BaggingClassifier(DecisionTreeClassifier(max_depth = 5), n_estimators =10, max_features=10)
bag_dt_10.fit(x_train,y_train)
bag_accuracy_10 = bag_dt_10.score(x_test, y_test)
print('Accuracy score of Bagged Classifier, 10 estimators, 10 max features: {}'.format(bag_accuracy_10))


# 3. Change base estimator to Logistic Regression
from sklearn.linear_model import LogisticRegression
bag_lr =  BaggingClassifier(LogisticRegression(), n_estimators=10, max_features=10)
bag_lr.fit(x_train, y_train)
bag_accuracy_lr = bag_lr.score(x_test, y_test)
print('Accuracy score of Logistic Regression, 10 estimators: {}'.format(bag_accuracy_lr))
```

## Implementing Random Forest using `scikit-learn`

### Classifier

```Python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import confusion_matrix, classification_report, accuracy_score, precision_score, recall_score, f1_score
  

df = pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/car/car.data', names=['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'accep'])
df['accep'] = ~(df['accep']=='unacc') #1 is acceptable, 0 if not acceptable
X = pd.get_dummies(df.iloc[:,0:6], drop_first=True)
y = df['accep']
x_train, x_test, y_train, y_test = train_test_split(X,y, random_state=0, test_size=0.25)

  

# 1. Create a Random Forest Classifier and print its parameters
rf = RandomForestClassifier()
rf_params = rf.get_params()
print('Random Forest parameters: {}'.format(rf_params))
#print(rf_params)

# 2. Fit the Random Forest Classifier to training data and calculate accuracy score on the test data
rf.fit(x_train, y_train)
y_pred = rf.predict(x_test)
rf_accuracy = rf.score(x_test, y_test)
print('Test set accuracy:')
print(rf_accuracy)

# 3. Calculate Precision and Recall scores and the Confusion Matrix

rf_precision = precision_score(y_test,y_pred)
print(f'Test set precision: {rf_precision}')
rf_recall = recall_score(y_test, y_pred)
print(f'Test set recall: {rf_recall}')
rf_confusion_matrix = confusion_matrix(y_test,y_pred)
print(f'Test set confusion matrix:\n{rf_confusion_matrix}')
```

```
##   
Output-only Terminal

Output:

Random Forest parameters: {'bootstrap': True, 'ccp_alpha': 0.0, 'class_weight': None, 'criterion': 'gini', 'max_depth': None, 'max_features': 'auto', 'max_leaf_nodes': None, 'max_samples': None, 'min_impurity_decrease': 0.0, 'min_impurity_split': None, 'min_samples_leaf': 1, 'min_samples_split': 2, 'min_weight_fraction_leaf': 0.0, 'n_estimators': 100, 'n_jobs': None, 'oob_score': False, 'random_state': None, 'verbose': 0, 'warm_start': False}
Test set accuracy:
0.9490740740740741
Test set precision: 0.9384615384615385
Test set recall: 0.8970588235294118
Test set confusion matrix:
[[288   8]
 [ 14 122]]
```


### Regressor
```Python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error

  
  

df = pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/car/car.data', names=['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'accep'])
df['accep'] = ~(df['accep']=='unacc') #1 is acceptable, 0 if not acceptable
X = pd.get_dummies(df.iloc[:,0:6], drop_first=True)

## Generating some fake prices for regression! :)
fake_prices = (15000 + 25*df.index.values)+np.random.normal(size=df.shape[0])*5000
df['price'] = fake_prices
print(df.price.describe())
y = df['price']
x_train, x_test, y_train, y_test = train_test_split(X,y, random_state=0, test_size=0.25)

# 1. Create a Random Regressor and print `R^2` scores on training and test data
rfr = RandomForestRegressor()
rfr.fit(x_train,y_train)
r_squared_train = rfr.score(x_train, y_train)
print(f'Train set R^2: {r_squared_train}')
r_squared_test = rfr.score(x_test, y_test)
print(f'Test set R^2: {r_squared_test}')

  

# 2. Print Mean Absolute Error on training and test data
avg_price = y.mean()
print(f'Avg Price Train/Test: {avg_price}')
train_y_predict = rfr.predict(x_train)
test_y_predict = rfr.predict(x_test)
mae_train = mean_absolute_error(y_train, train_y_predict)
print(f'Train set MAE: {mae_train}')
mae_test = mean_absolute_error(y_test, test_y_predict)
print(f'Test set MAE: {mae_test}')
```

