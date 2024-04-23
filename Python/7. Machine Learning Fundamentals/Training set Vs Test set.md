___
## Introduction
Before we even begin to think about writing the code we will use to fit our machine learning model to the data, we must perform the split of our dataset to create our training set, validation set, and test set.
- The _training set_ is the data that the model will learn how to make predictions from. Learning looks different depending on which algorithm we are using. With Linear Regression, the observations in the training set are used to fit the parameters of a line of best fit. For choose different model (classification), the observations in the training set are used to fit the parameters being fit.
- The _validation set_ is the data that will be used during the training phase to evaluate the interim performance of the model, guide the tuning of hyper-parameters, and assist in other model improvement capacities (for example, feature selection). Some common metrics used to calculate the performance of machine learning models are
- The _test set_ is the data that will determine the performance of our final model so we can estimate how our model will fare in the real world. To avoid introducing any bias to the final measurements of performance, we do not want the test set anywhere near the model training or tuning processes. That is why the test set is often referred to as the holdout set.
## Evaluating the Model
During model fitting, both the features (X) and the true labels (y) of the training set (`Xtrain`, `ytrain`) are used to learn. When evaluating the performance of the model with the validation (`Xval, yval`) or test (`Xtest, ytest`) set, we are going to temporarily pretend like we do not know the true label of every observation. If we use the observation features in our validation (`Xval`) or test (`Xtest`) sets as inputs to the trained model, we can receive a prediction as output for each observation (`ypred`). We can now compare each of the true labels (`yval or ytest`) with each of the predicted labels (`ypred`) and get a quantitative evaluation on the performance of the model.

## How to Split?
If our training set is too small, then the model might not have enough data to effectively learn. On the other hand, if our validation and test sets are too small, then the performance metrics could have a large variance. In general, putting 70% of the data into the training set and 15% of the data into each of the validation and test sets is a good place to start.

## N-Fold Cross-Validation
Sometimes our dataset is so small that splitting it into training, validation, and test sets that are appropriate sizes is unfeasible. A potential solution is to perform N-Fold Cross-Validation. While we still first split the dataset into a training and test set, we are going to further split the training set into N chunks. In each iteration (or fold), N-1 of the chunks are treated as the training set and 1 of the chunks is treated as the validation set over which the evaluation metrics are calculated.

This process is repeated N times cycling through each chunk acting as the validation set and the evaluation metrics from each fold are averaged. For example, in 10-fold cross-validation, we’ll make the validation set the first 10% of the training set and calculate our evaluation metrics. We’ll then make the validation set the second 10% of the data and calculate these statistics once again. We can do this process 10 times, and every time the validation set will be a different chunk of the data. If we then average all of the accuracies, we will have a better sense of how our model does on average.
![[Pasted image 20240203120255.png]]

## The basic train-test split in `sklearn`
As with most machine learning algorithms, we have to split our dataset into:
- **Training set**: the data used to fit the model
- **Test set**: the data partitioned away at the very start of the experiment (to provide an unbiased evaluation of the model)
In general, putting 80% of your data in the training set and 20% of your data in the test set is a good place to start.
Suppose you have some values in `x` and some values in `y`:
```Python
from sklearn.model_selection import train_test_split 
x_train, x_test, y_train, y_test = train_test_split(X,y,train_size=0.8)
```

Here are the parameters:

- `train_size`: the proportion of the dataset to include in the train split (between 0.0 and 1.0)
- `test_size`: the proportion of the dataset to include in the test split (between 0.0 and 1.0)
- `random_state`: the seed used by the random number generator \[optional]
