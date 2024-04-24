___

## Introduction

Stacking fundamentally operates differently from bagging and boosting in two ways:
- The base estimators used in the ensemble need not be weak learners. This is in contrast to boosting where we use a sequence of weak learners that improve performance each iteration.
- Unlike bagging, there are no subsampling processes used. The stacking model effectively uses a full training set.
Consider the scenario in which we are handling a classification problem where we are exploring different models. When using a decision tree we find that our model has poor generalization performance as a result of overfitting. Additionally, when using a logistic regression classification model, we discover our model parameters are hard to tune due to highly-correlated features. We might discover that while each model has a unique advantage over the others, each may also have a distinct drawback such as poor generalization accuracy or the inability to predict a specific class. As it turns out, there’s no rule stating we can’t take the best of both worlds!

Stacking can be thought of as a democracy of machine learning models, where different models are trained and subsequently cast their vote through their predictions. A majority-rules approach can be used for determining the final model prediction if we weighted each estimator equally. In practice, each base estimator may need to be weighted differently, so we have a later-stage model to learn how to appropriately weigh the predictions of all the prior base estimators.

## Training Base Estimators
We can select from combinations of different base estimators, such as a logistic regression model in combination with a decision tree. We could additionally select models of the same learning algorithms, but with different parameters, such as multiple decision trees with varying depths. The number of estimators is arbitrary, so it’s good practice to explore how different combinations behave.
This introduces a problem, however. The estimators would be making predictions on data used in training. This puts our model at risk of overfitting. To avoid this, we use K-Fold Cross Validation
![[Pasted image 20240208234650.png]]

## K-Fold cross validation
Consider 10 segments (or folds) and a stacking model that uses a logistic regression model and a decision tree model. Each estimator can be trained using data from 9 of the segments, and make predictions on the excluded 10th segment. We then append the predictions as new features to that 10th segment. Now 1/10th of the training data has two new features: one is the prediction made by the logistic regression model and the other is the prediction made by the decision tree model.

We want to do the same with the other 9 segments, so we rotate the excluded segment and repeat this process until all training data points are augmented with new features. The end result is a prediction made on each training sample, without having seen the sample during the training process.

##### Feature Augmentation
In our stacking setup, the base estimators need to be trained to make predictions on our training data. The prediction of each estimator will be appended to the corresponding data sample as a new feature. We thus augment the training data set with this additional information. The augmented training set is used by our later-stage stacking model to make the final prediction.
So, in summary, say our training dataset has 10,000 samples, 10 features, and we select 3 base estimators. We would train each base estimator on the training set and make predictions on the training set. Each estimator would make a prediction on each sample, therefore each sample will have 3 predictions. These 3 predictions are appended to the pre-existing 10 features. This leaves us with 10,000 training samples with 13 features each.

#### Training the stacking model
With our augmented training set, we’re ready to prepare the final model that will make the official prediction from our stacking setup.

This later-stage model is a learning algorithm that we select just like the base estimators. We may even reuse an algorithm from an earlier stage here. The purpose of this model is to learn the proper weighting of the earlier estimator given our training samples now include a data point from each estimator. Some estimators may perform better than others, so our overall model should account for this.

The only difference in the training process is that the model will be designed to accept samples of the augmented size rather than the given size. This means if the given data set has n features and m base estimators, this model will require n + m features on each sample.

And that’s it! Once the later-stage model is trained, we feed testing data samples into the base estimators, which will append their predictions to the data sample. This sample is then given to the later-stage model for our final prediction.

## Implementing stacking with `scikit-learn`
```Python
import pandas as pd  
df = pd.read_csv('water_potability')  
print(df.columns, df.shape)
```

```
Index(['ph', 'Hardness', 'Solids', 'Chloramines', 'Sulfate', 'Conductivity',  
       'Organic_carbon', 'Trihalomethanes', 'Turbidity', 'Potability'],  
      dtype='object') (2011, 10)
```

```Python
X = water_potability.drop(['Potability'], axis=1)  
y = water_potability['Potability']  
  
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.20, random_state=rand_state)
```

To assemble our ensemble, we’ll make a dictionary of base estimators. This will be contained within the `level_0_estimators` dict. Also, our final estimator will be a Random Forest as represented with `level_1_estimator`. Notice also how we prepare to add new features to our training dataset (as columns) in `level_0_columns`.
```Python
level_0_estimators = dict()  
level_0_estimators["logreg"] = LogisticRegression(random_state=rand_state)  
level_0_estimators["forest"] = RandomForestClassifier(random_state=rand_state)  
  
level_0_columns = [f"{name}_prediction" for name in level_0_estimators.keys()]  
  
level_1_estimator = RandomForestClassifier(random_state=rand_state)
```

Handling our k-fold cross-validation is fairly straightforward using `sklearn.model_selection.StratifiedKFold` from the `scikit-learn` library. The kfold is then given to the instantiated `StackingClassifier`
```Python
kfold = StratifiedKFold(n_splits=10, shuffle=True, random_state=rand_state)  
stacking_clf = StackingClassifier(estimators=list(level_0_estimators.items()),  
                                    final_estimator=level_1_estimator,  
                                    passthrough=True, cv=kfold, stack_method="predict_proba")
```

Calling `fit_transform` on our classifier manages a lot of the heavy lifting. It will handle the training of our `level_0` base estimators along with the `level_1_estimator`, make cross-validated predictions on the training set, and augment the training set with predictions from each estimator. Let’s see how the resulting training dataset looks

```Python
df = pd.DataFrame(stacking_clf.fit_transform(X_train, y_train), columns=level_0_columns + list(X_train.columns))
```
![[Pasted image 20240209000001.png]]
Finally, with our full model trained, we can make predictions. Let’s compare how our Stacking classifier performed with how a lone linear model and a lone decision tree model perform!

```Python
y_val_pred = stacking_clf.predict(X_test)  
stacking_accuracy = accuracy_score(y_test, y_val_pred)  
  
vanilla_logistic_regression = LogisticRegression(random_state=rand_state).fit(X_train, y_train)  
lr_accuracy = accuracy_score(y_test, vanilla_logistic_regression.predict(X_test))  
                                     
vanilla_decision_tree = RandomForestClassifier(random_state=rand_state).fit(X_train, y_train)  
dt_accuracy =  accuracy_score(y_test, vanilla_decision_tree.predict(X_test))  
  
print(f'Stacking accuracy: {stacking_accuracy:.4f}')  
print(f'Logistic Regression accuracy: {lr_accuracy:.4f}')  
print(f'Decision Tree accuracy: {dt_accuracy:.4f}')
```

```
Stacking accuracy: 0.6576  
Logistic Regression accuracy: 0.5732  
Decision Tree accuracy: 0.6526
```

## Limitations of stacking 
Stacking is very powerful in that we remove the occasionally difficult choice of which learning algorithm to use for our problem. Depending on the use case, this benefit does come with some limitations worth noting:

- Because we have an arbitrary number of learning algorithms in use, training an entire stacking model is computationally expensive. This is also true for deployed inference models.
    
- Such a large model with many parameters means that a plethora of data is needed for proper training. Small datasets won’t see significant gains with stacking. Stacking models typically yields marginal gains over the best single estimator used for the same problem. When successful, a stacked model may reduce error by 2% or less.
### Additional considerations
Stacking offers some creativity and openendedness in how we want to build our model. A vanilla stacking model may have one tier of models to contribute to the final prediction. We could alternatively construct a multi-tier approach in which one level of models feeds into a later stage of models before making a final prediction.

As with other machine learning models, we also have many parameters to tune and select from. We can enhance model diversity by using different training algorithms, different training sets, different feature subsets, and different hyperparameters.