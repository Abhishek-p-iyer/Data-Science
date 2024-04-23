____

## Introduction

There are many reasons why we might be interested in calculating feature importances as part of our machine learning workflow. For example:

- Feature importance is often used for dimensionality reduction.
    - We can use it as a filter method to remove irrelevant features from our model and only retain the ones that are most highly associated with our outcome of interest.
    - Wrapper methods such as recursive feature elimination use feature importance to more efficiently search the feature space for a model.
- Feature importance may also be used for model inspection and communication. For example, stakeholders may be interested in understanding which features are most important for prediction. Feature importance can help us answer this question.

## Calculating feature importance 
There are many different ways to calculate feature importance for different kinds of machine learning models. In this section, we’ll investigate one tree-based method in a little more detail: **Gini impurity**.
### Gini impurity
Imagine, for a moment, that you’re interested in building a model to screen candidates for a particular job. In order to build this model, you’ve collected some data about candidates who you’ve hired and rejected in the past. For each of these candidates, suppose that you have data on years of experience and certification status. Consider the following two simple decision trees that use these features to predict whether the candidate was hired:
![[Pasted image 20240207173839.png]]
Which of these features seems to be more important for predicting whether a candidate will be hired? In the first example, we saw that _most_ candidates who had >5 years of experience were hired and _most_ candidates with <5 years were rejected; however, _all_ candidates with certifications were hired and _all_ candidates without them were rejected.

Gini impurity is related to the extent to which observations are well separated based on the outcome variable at each node of the decision tree. For example, in the two trees above, the Gini impurity is higher in the node with all candidates (where there are an equal number of rejected and hired candidates) and lower in the nodes after the split (where most or all of the candidates in each grouping have the same outcome — either hired or rejected).
To estimate feature importance, we can calculate the Gini gain: the amount of Gini impurity that was eliminated at each branch of the decision tree. In this example, certification status has a higher Gini gain and is therefore considered to be more important based on this metric.

#### Gini importance in scikit-learn
To demonstrate how we can estimate feature importance using Gini impurity, we’ll use the breast cancer dataset from `sklearn`. This dataset contains features related to breast tumors. The outcome variable is the diagnosis: either malignant or benign. To start, we’ll load the dataset and split it into a training and test set:
```Python
import pandas as pd  
from sklearn.model_selection import train_test_split  
from sklearn import datasets  
  
dataset = datasets.load_breast_cancer()  
X = pd.DataFrame(dataset.data, columns=dataset.feature_names)  
y = dataset.target  
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25)
```

Next, we’ll fit a decision tree to predict the diagnosis using `sklearn.tree.DecisionTreeClassifier()`. Note that we’re setting `criterion= 'gini'`. This actually tells the function to build the decision tree by splitting each node based on the feature that has the highest Gini gain. By building the tree in this way, we’ll be able to access the Gini importances later.
```Python
from sklearn.tree import DecisionTreeClassifier  
clf = DecisionTreeClassifier(criterion='gini')  
# Fit the decision tree classifier  
clf = clf.fit(X_train, y_train)
# Print the feature importances  
feature_importances = clf.feature_importances_
```

Finally, we’ll visualize these values using a bar chart:
```Python
import seaborn as sns  
  
# Sort the feature importances from greatest to least using the sorted indices  
sorted_indices = feature_importances.argsort()[::-1]  
sorted_feature_names = data.feature_names[sorted_indices]  
sorted_importances = feature_importances[sorted_indices]  
  
# Create a bar plot of the feature importances  
sns.set(rc={'figure.figsize':(11.7,8.27)})  
sns.barplot(sorted_importances, sorted_feature_names)
```
![[Pasted image 20240207174148.png]]
Based on this output, we could conclude that the features `worst concave points`, `worst radius` and `mean texture` are most predictive of a malignant tumor. There are also many features with importances close to zero which we may want to exclude from our model.

#### Pros and cons of using Gini importance
Because Gini impurity is used to train the decision tree itself, it is computationally inexpensive to calculate. However, Gini impurity is somewhat biased toward selecting numerical features (rather than categorical features). It also does not take into account the correlation between features. For example, if two highly correlated features are both equally important for predicting the outcome variable, one of those features may have low Gini-based importance because all of it’s explanatory power was ascribed to the other feature. This issue can be mediated by removing redundant features before fitting the decision tree.

### Other measures of feature importance

#### Aggregate methods
Random forests are an ensemble-based machine learning algorithm that utilize many decision trees (each with a subset of features) to predict the outcome variable. Just as we can calculate Gini importance for a single tree, we can calculate average Gini importance across an entire random forest to get a more robust estimate.

#### Permutation-based methods
Another way to test the importance of particular features is to essentially remove them from the model (one at a time) and see how much predictive accuracy suffers. One way to “remove” a feature is to randomly permute the values for that feature, then refit the model. This can be implemented with any machine learning model, including non-tree-based- methods. However, one potential drawback is that it is computationally expensive because it requires us to refit the model many times.

#### Coefficients
When we fit a general(ized) linear model (for example, a linear or logistic regression), we estimate coefficients for each predictor. If the original features were standardized, these coefficients can be used to estimate relative feature importance; larger absolute value coefficients are more important. This method is computationally inexpensive because coefficients are calculated when we fit the model. It is also useful for both classification and regression problems (i.e., categorical and continuous outcomes). However, similar to the other methods described above, these coefficients do not take highly correlated features into account.

