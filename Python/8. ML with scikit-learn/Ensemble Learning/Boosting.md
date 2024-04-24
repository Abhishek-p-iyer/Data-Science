___

## Introduction
Boosted ensemble methods use weak learners as base models that are simple and tend to suffer from high bias. The weak learners underfit the data. Boosting is a sequential learning technique where each of the base models builds off of the previous model. Each subsequent model aims to improve the performance of the final ensemble model by attempting to fix the errors in the previous stage. There are two important decisions that need to be made to perform boosted ensembling:
1. Sequential Fitting Method
2. Aggregation Method

## Adaptive Boosting 
It is a sequential ensembling method that can be used for both classification and regression. It can use any base machine learning model, though it is most commonly used with decision trees.
For AdaBoost, the **Sequential Fitting Method** is accomplished by updating the weight attached to each of the training dataset observations as we proceed from one base model to the next. The **Aggregation Method** is a weighted sum of those base models where the model weight is dependent on the error of that particular estimator.
The training of an AdaBoost model is the process of determining the training dataset observation weights at each step as well as the final weight for each base model for aggregation.


## Working of AdaBoost

![[Pasted image 20240208232747.png]]
Let’s take a deeper look at how AdaBoost works! AdaBoost can be used for both regression and classification, but in this example we will be solving a classification problem. We begin with the full Training Dataset. You will see that it consists of green circles and red triangles. The goal of our AdaBoost classifier will be to form a decision boundary that separates these two classes. Initially, the training data instances are all given the same weight. This is indicated by the size of shapes all being the same.

Our first step is to fit an estimator, the 1st Base Model. While boosting can be applied to any base machine learning model, we will use decision trees. But aren’t decision trees prone to overfitting? We already said that the base models for boosting are supposed to be very simple and tend to underfit. That is correct, and for this reason we use the simplest version of a decision tree, known as a decision stump. A decision stump only makes a single decision, so the resultant estimator only has two leaf nodes.

Taking a look at the Result of the 1st Base Model, we see that the decision boundary, that is the border between the lighter green and lighter red regions, does a decent job of separating the green circles from the red triangles. However we do notice that there are two red triangles in the light green region. This indicates that they have been classified incorrectly by the decision stump.

Each of the base models will contribute a different amount to the final ensemble model. The influence that a particular base model contributes is going to be dependent on the number of errors it makes, or for regression, the magnitude of the errors it makes. We do not want a decision stump that does a terrible job of classifying the data to have the same say as a decision stump that does a great job. Once we are able to evaluate the Result of the 1st Base Model, we can Weight the Model and assign it a value, here indicated by `alpha_1`.

To prepare for the next stage of the sequential learning process, we need to Reweight the Data. The instances of the training data that were classified incorrectly by the 1st Base Model, the two red triangles in the middle right, are given a larger weight than the other data instances indicated by their larger size. By assigning those misclassified points a larger weight, we are asking the the 2nd Base Model to give them preferential treatment during the Model Fitting.

Taking a look at the Result of the 2nd Base Model, we see that is exactly what happens. The two larger red triangles are classified correctly by the 2nd Base Model. Once again we assign the base model a weight, `alpha_2` proportional to the errors it makes and prepare for the next stage of the sequential learning by reweighting the training data. The instances that were incorrectly classified by the 2nd Base Model, the two green circles in on the center right, are given a larger weight.

Once we have reached the predefined number of estimators for our AdaBoost model, the base models are ready to aggregate. In this example we have chosen `n_estimators = 3`. The influence of each base model in the final ensemble model will be proportional to the `alpha` it was assigned during the training process.


## Implementation
```Python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import AdaBoostClassifier
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, confusion_matrix

# Load dataset to a pandas DataFrame
path_to_data = 'https://archive.ics.uci.edu/ml/machine-learning-databases/car/car.data'
column_names = ['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'accep']
df = pd.read_csv(path_to_data, names=column_names)
target_column = 'accep'
raw_feature_columns = [col for col in column_names if col != target_column]

# Create dummy variables from the feature columns
X = pd.get_dummies(df[raw_feature_columns], drop_first=True)
# Convert target column to binary variable; 0 if 'unacc', 1 otherwise
df[target_column] = np.where(df[target_column] == 'unacc', 0, 1)
y = df[target_column]

# Split the full dataset into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=123, test_size=0.3)

# 1. Create a decision stump base model using the Decision Tree Classifier and print its parameters
decision_stump = DecisionTreeClassifier(max_depth=1)
print(decision_stump.get_params())
# 2. Create an Adaptive Boost Classifier and print its parameters
ada_classifier = AdaBoostClassifier(base_estimator = decision_stump, n_estimators=5)
print(ada_classifier.get_params())
# 3. Fit the Adaptive Boost Classifier to the training data and get the list of predictions
ada_classifier.fit(X_train,y_train)
y_pred = ada_classifier.predict(X_test)
# 4. Calculate the accuracy, precision, recall, and f1-score on the testing data
accuracy = accuracy_score(y_test, y_pred)
precision = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)
f1 = f1_score(y_test, y_pred)

# 5. Remove the comments from the following code block to print the confusion matrix
test_conf_matrix = pd.DataFrame(
     confusion_matrix(y_test, y_pred, labels=[1, 0]),
     index=['actual yes', 'actual no'],
     columns=['predicted yes', 'predicted no'])
print(f'Confusion Matrix:\n{test_conf_matrix.to_string()}')
```

```
{'ccp_alpha': 0.0, 'class_weight': None, 'criterion': 'gini', 'max_depth': 1, 'max_features': None, 'max_leaf_nodes': None, 'min_impurity_decrease': 0.0, 'min_samples_leaf': 1, 'min_samples_split': 2, 'min_weight_fraction_leaf': 0.0, 'random_state': None, 'splitter': 'best'}
{'algorithm': 'SAMME.R', 'base_estimator__ccp_alpha': 0.0, 'base_estimator__class_weight': None, 'base_estimator__criterion': 'gini', 'base_estimator__max_depth': 1, 'base_estimator__max_features': None, 'base_estimator__max_leaf_nodes': None, 'base_estimator__min_impurity_decrease': 0.0, 'base_estimator__min_samples_leaf': 1, 'base_estimator__min_samples_split': 2, 'base_estimator__min_weight_fraction_leaf': 0.0, 'base_estimator__random_state': None, 'base_estimator__splitter': 'best', 'base_estimator': DecisionTreeClassifier(max_depth=1), 'learning_rate': 1.0, 'n_estimators': 5, 'random_state': None}
Confusion Matrix:
            predicted yes  predicted no
actual yes            129            25
actual no              49           316
```

## Gradient boosting 
For Gradient Boost, the **Sequential Fitting Method** is accomplished by fitting a base model to the negative gradient of the error in the previous stage. The **Aggregation Method** is a weighted sum of those base models where the model weight is constant.

The training of a Gradient Boosted model is the process of determining the base model error at each step and using those to determine how to best formulate the subsequent base model.

## Working of Gradient Boosting

![[Pasted image 20240208233439.png]]

Now we will take a deeper look at how Gradient Boosting works. While Gradient Boosting can be applied to any base machine learning model, decision trees are commonly used in practice. In this example we will be focusing on a Gradient Boosted Trees model.

Our first step is to fit an estimator, the 1st Base Model. Recall that the base estimators for boosting algorithms tend to be simple and high bias. In contrast to AdaBoost which leveraged the simplest form of decision trees, the decision stump with only 1 level, gradient boosted trees can and actually do tend to include a few more decision branches. Often gradient boosted trees will have up to 32 leaf nodes, which corresponds to a tree depth of 5 levels. In this example, we are limiting the depth of the base estimators to 2, corresponding to 4 leaf nodes.

Once the 1st Base Model is trained, the residual errors (`h_1`), of the model given the training training data are determined. The residual error is the difference between the actual and predicted values for each of the training data instances.

$$ h_1 = y_a - y_p$$

The errors will be greater for the training data instances where the model did not do as good of a job with its prediction and will be lower on training data instances where the model fit the data well.
In the next stage of the sequential learning process, we fit the 2nd Base Model. Here is where the interesting part comes in. Instead of fitting the model to the target values `y_actual` as we are typically used to doing in machine learning, we actually fit the model on the errors of the previous stage, in this case `h_1`. The 2nd Base Model is literally learning from the mistakes of the 1st Base Model through those residuals that were calculated.
The results of the 2nd Base Model are multiplied by a constant learning rate, `alpha`, and added to the results of the 1st Base Model to give the set of updated predictions, The results of the second base model, which was tasked with fitting the errors of the first base model are multiplied by a constant learning rate, alpha and added to the results of the first base model to give us a set of updated predictions, `y_2(predicted)`.
The subsequent stages repeat the same steps. At stage `N`, the base model is fit on the errors calculated at the previous stage `h_(N-1)`. The new model that is fit is multiplied by the constant learning rate `alpha` and added to the predictions of the previous stage.
Once we have reached the predefined number of estimators for our Gradient Boosting model or the residual errors are not changing between iterations, the model will stop training and we end up with the resultant ensemble model.

## Implementation of Gradient Boosting
```Python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, confusion_matrix

# Load dataset to a pandas DataFrame
path_to_data = 'https://archive.ics.uci.edu/ml/machine-learning-databases/car/car.data'
column_names = ['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'accep']

df = pd.read_csv(path_to_data, names=column_names)
target_column = 'accep'
raw_feature_columns = [col for col in column_names if col != target_column]

# Create dummy variables from the feature columns
X = pd.get_dummies(df[raw_feature_columns], drop_first=True)
# Convert target column to binary variable; 0 if 'unacc', 1 otherwise
df[target_column] = np.where(df[target_column] == 'unacc', 0, 1)
y = df[target_column]

# Split the full dataset into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=123, test_size=0.3)
# 1. Create a Gradient Boosting Classifier and print its parameters
grad_classifier = GradientBoostingClassifier(n_estimators = 15)
print(grad_classifier.get_params())
# 2. Fit the Gradient Boosted Trees Classifier to the training data and get the list of predictions
grad_classifier.fit(X_train, y_train)
y_pred = grad_classifier.predict(X_test)

# 3. Calculate the accuracy, precision, recall, and f1-score on the testing data
accuracy = accuracy_score(y_pred, y_test)
precision = precision_score( y_test, y_pred)
recall = recall_score(y_test, y_pred)
f1= f1_score(y_pred, y_test)
print(f'Test set accuracy:\t{accuracy}')
print(f'Test set precision:\t{precision}')
print(f'Test set recall:\t{recall}')
print(f'Test set f1-score:\t{f1}')

# 4. Remove the comments from the following code block to print the confusion matrix
test_conf_matrix = pd.DataFrame(
    confusion_matrix(y_test, y_pred, labels=[1, 0]),
    index=['actual yes', 'actual no'],
    columns=['predicted yes', 'predicted no']
)

print(f'Confusion Matrix:\n{test_conf_matrix.to_string()}')
```
