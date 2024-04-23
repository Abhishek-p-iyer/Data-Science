___
To build a logistic regression model, we apply a _**logit link function**_ to the left-hand side of our linear regression function. Remember the equation for a linear model looks like this:
$$ y = b_0 + b_1x_1 + b_2x_2 \ +  \ ....... + \ b_nx_n$$
When we apply the logit function, we get the following:
$$ ln(\frac{y}{1-y}) =  b_0 + b_1x_1 + b_2x_2 \ +  \ ....... + \ b_nx_n $$

![[Pasted image 20240202123441.png]]

### Log-Odds
So far, we’ve learned that the equation for a logistic regression model looks like this:
$$ ln(\frac{p}{1-p}) =  b_0 + b_1x_1 + b_2x_2 \ +  \ ....... + \ b_nx_n $$

Note that we’ve replaced _y_ with the letter _p_ because we are going to interpret it as a probability (eg., the probability of a student passing the exam). The whole left-hand side of this equation is called _**log-odds**_ because it is the natural logarithm (_ln_) of odds (_p/(1-p)_). The right-hand side of this equation looks exactly like regular linear regression!
In order to understand how this link function works, let’s dig into the interpretation of _**log-odds**_ a little more. The _odds_ of an event occurring is:
$$ Odds = \frac{p}{1-p} = \frac{P(event \ occuring)}{P(event \ not \ occuring)} $$

> Odds can only be a positive number. When we take the natural log of odds (the log odds), we transform the odds from a positive value to a number between negative and positive infinity — which is exactly what we need! The logit function (log odds) transforms a probability (which is a number between 0 and 1) into a continuous value that can be positive or negative.

## Sigmoid Function
Suppose that we want to fit a model that predicts whether a visitor to a website will make a purchase. We’ll use the number of minutes they spent on the site as a predictor. The following code fits the model:
```Python 
from sklearn.linear_model import LogisticRegression()
model = LogisticRegression()
model.fit(min_on_site, purchases)
log_odds = model.intercept_ + model.coef_+min_on_site
print(log_odds)
```
``` OUTPUT
[[-3.28394203]  
[-1.46465328]  
[-0.02039445]  
[ 1.22317391]  
[ 2.18476234]]
```
Notice that these predictions range from negative to positive infinity: these are log odds. In other words, for the first datapoint, we have:
$$ ln(\frac{p}{1-p}) = -3.28394203$$
Using numpy to solve this, we get
```Python
np.exp(log_odds)/(1+np.exp(log_odds))
```
```
array([[0.0361262 ],  
       [0.18775665],  
       [0.49490156],  
       [0.77262162],  
       [0.89887279]])
```
The calculation that we just did required us to use something called the _sigmoid function_, which is the inverse of the logit function. The sigmoid function produces the S-shaped curve we saw previously:
![[Pasted image 20240202124641.png]]

> An important thing to note about the Logistic regression coefficients are:
> - ==Large positive coefficient==: a one unit increase in that feature is associated with a ==large **increase**== in the log odds (and therefore probability) of a datapoint belonging to the positive class (the outcome group labeled as `1`)
> - ==Large negative coefficient:== a one unit increase in that feature is associated with a ==large **decrease** ==in the log odds/probability of belonging to the positive class.
>  - Coefficient of 0: The feature is not associated with the outcome

In summary, we've done the following:
- Call the model
- Fit the model
- Get the coefficients and intercept values
- Apply these to the equation
- Get the log odds values
- Solve for the p values

### Predictions in `sklearn`
After we've trained the data, we can try fitting the model in unseen data and the model will predict which class the datapoint belongs to. The input is a matrix of features and the output is a vector of predicted labels, `1` or `0`.

```Python
print(model.predict(features))  
# Sample output: [0 1 1 0 0]
```

If we are more interested in the predicted probability of group membership, we can use the `.predict_proba()` method. The input to `predict_proba()` is also a matrix of features and the output as an array of values ranging from 0 to 1
```Python
print(model.predict_proba(features)[:,1])  
# Sample output: [0.32 0.75  0.55 0.20 0.44]
```
By default, `.predict_proba()` returns the probability of class membership for both possible groups. In the example code above, we’ve only printed out the probability of belonging to the positive class. Notice that datapoints with predicted probabilities greater than 0.5 (the second and third datapoints in this example) were classified as `1`s by the `.predict()` method. This is a process known as thresholding. As we can see here, sklearn sets the default classification threshold probability as 0.5.

## Classification thresholding

As we’ve seen, logistic regression is used to predict the probability of group membership. Once we have this probability, we need to make a decision about what class a datapoint belongs to. This is where the _**classification threshold**_ comes in!
The default threshold for `sklearn` is `0.5`. If the predicted probability of an observation belonging to the positive class is greater than or equal to the threshold, `0.5`, the datapoint is assigned to the positive class.
![[Pasted image 20240202130216.png]]
We can choose to change the threshold of classification based on the use-case of our model. For example, if we are creating a logistic regression model that classifies whether or not an individual has cancer, we may want to be more sensitive to the positive cases. We wouldn’t want to tell someone they don’t have cancer when they actually do!
In order to ensure that most patients with cancer are identified, we can move the classification threshold down to `0.3` or `0.4`, increasing the sensitivity of our model to predicting a positive cancer classification. While this might result in more overall misclassifications, we are now missing fewer of the cases we are trying to detect: actual cancer patients.

  ![[Pasted image 20240202130230.png]]

## Confusion Matrix
When we fit a machine learning model, we need some way to evaluate it. Often, we do this by splitting our data into training and test datasets. We use the training data to fit the model; then we use the test set to see how well the model performs with new data.
For ex: suppose that the true and predicted classes for a logistic regression model are:
```Python
y_true = [0, 0, 1, 1, 1, 0, 0, 1, 0, 1]  
y_pred = [0, 1, 1, 0, 1, 0, 1, 1, 0, 1]
```

We can create a confusion matrix as follows:
```Python 
from sklearn.metrics import confusion_matrix 
print(confusion_matrix(y_true, y_pred))
```
Output:
```
array([[3, 2],  
       [1, 4]])
```
This output tells us that there are `3` true negatives, `1` false negative, `4` true positives, and `2` false positives. Ideally, we want the numbers on the main diagonal (in this case, `3` and `4`, which are the true negatives and true positives, respectively) to be as large as possible.

## Assumptions of Logistic Regression
- ==The target variable is binary:== The target variable NEEDS to be binary
- ==Independent observation:== The observations need to be independent, For ex: When we are considering the cancer prediction example, if a patient biopsied multiple times, the observation would not be independent. (repeated sampling of the same individual).
- ==Large enough sample size:== Often a rule of thumb is that there should be at least 10 samples per feature for the smallest class in the outcome variable. For example, if there were 100 samples and the outcome variable `diagnosis` had 60 benign tumors and 40 malignant tumors, then the max number of features allowed would be 4. To get 4 we took the smallest of the classes in the outcome variable, 40, and divided it by 10.
- ==No influence outliers==: Logistic regression is sensitive to outliers, so we must remove any extremely influential outliers for model building.
- ==No multicollinearity==: We cannot have two features that are highly correlated.
- ==Features are linearly related to log odds==  Similar to linear regression, the underlying assumption of logistic regression is that the features are linearly related to the logit of the outcome. To test this visually, we can use `seaborn`’s `regplot`, with the parameter `logistic= True` and the x value as our feature of interest. If this condition is met, the fit model will resemble a sigmoidal curve (as is the case when `x=radius_mean`).
#### To change the prediction threshold
We print the confusion matrix for the prediction threshold as 0.75 instead of 0.5
```Python
print("Confusion Matrix: Threshold 75%")
y_pred_class75 = (y_pred_prob[:,1]>0.75)*1.0
cm_75 = confusion_matrix(y_test, y_pred_class75)
print(cm_75)
```

## ROC curve and AUC
We have examined how changing the threshold can affect the logistic regression predictions. There is a continuum of predictions available in a single model by varying the threshold incrementally from zero to one. For each of these thresholds, the True Positive Rate (TPR) and the False Positive Rate (FPR) can be calculated and then plot. The resulting curve these points form is known as the Receiver Operating Characteristic (ROC) curve.
![[Pasted image 20240202135018.png]]
In the ROC curve plotted above, the True Positive Rate (`TPR = TP / TP + FN`) is on the y-axis and the False Positive Rate (`FPR = FP / TN + FP`) is on the x-axis. The ROC curve is the orange line and the dashed blue line is the Dummy Classification line, which is the equivalent of random guessing.

Notice there are three data points on the ROC curve, each labeled with their threshold values. The classification threshold of `.5` will give us a TPR of about `.65` with an FPR of about `.28`. For our specific data, we want a higher TPR so that we catch every malignant tumor. We might select a lower threshold of `.25` so that our TPR is about `.8`, even though this may give us an FPR of about `.4`. The ROC curve can help us decide on a threshold that best fits our specific classification problem.

While the ROC curve measures the probabilities, the AUC (Area Under the Curve) gives us a single metric for separability. The AUC tells us how well our model can distinguish between the two classes. An AUC score close to 1 is a near-perfect classifier, whereas a value of 0.5 is equivalent to random guessing. To visualize different AUC scores, look at the ROC curve plots below:
![[Pasted image 20240202135048.png]]

## Class imbalance
Class imbalance occurs when our binary classes for the outcome variable are not evenly split. Technically, anything different from a 50/50 distribution would be imbalanced and need appropriate care. In the case of rare events, sometimes the positive class can be less than 1% of the total. If your classes are significantly imbalanced, this could create a bias towards the majority class since the model learns that it can have a higher accuracy if it predicts the majority class more often.
The positivity rate of a class can tell us how balanced our data is. The positivity rate is the rate of occurrence for the positive class. With our breast cancer data, the formula is `Positivity Rate = Total Malignant Cases / Total Cases`. If our positivity rate is close to .5, then our classes are balanced.

### Stratification
If your classes are imbalanced (more likely to happen with smaller datasets) then this difference can become even greater after you split your data into a training and testing dataset. One way to mitigate this is to randomly split using stratification on the class labels. Stratification is when the data is sorted into subgroups to ensure a nearly equal class distribution in your train and test sets. After using stratification, the training and testing datasets should have a very similar positivity rate (but stratification does not necessarily cause the positivity rate of the dataset to reach closer to 0.5).
### Under-sampling/Oversampling
To bring the positivity rate of the dataset closer to .5, we can under-sample the majority class or oversample the minority class. For oversampling, repeated samples (with replacement) are taken from the minority class until the size is equal to that of the majority class. This causes the same data to be used multiple times, giving a higher weight to these samples. Alternatively, under-sampling leaves out some of the majority class data to have the same number of samples as the minority class, leaving fewer data to build the model.
### Balance the class weight
When training a model, it is the default for every sample to be weighted equally. However, in the case of class imbalance, this can result in poor predictive power for the smaller of the two classes. A way to counteract this in logistic regression is to use the parameter `class_weight=balanced`. This applies a weight inversely proportional to the class frequency, therefore supplying higher weight to misclassified instances in the smaller class. While overall accuracy may not increase, this can increase the accuracy for the smaller class (e.g. increase the number of malignant cases correctly diagnosed).

```Python
import pandas as pd
import numpy as np
import codecademylib3
import matplotlib.pyplot as plt
import seaborn as sns

#Import models from scikit learn module:

from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import confusion_matrix, classification_report, accuracy_score, precision_score, recall_score, f1_score


#https://www.kaggle.com/uciml/breast-cancer-wisconsin-data
df = pd.read_csv('breast_cancer_data.csv')
#encode malignant as 1, benign as 0
df['diagnosis'] = df['diagnosis'].replace({'M':1,'B':0})
predictor_var = ['radius_mean', 'texture_mean', 'compactness_mean','symmetry_mean']
outcome_var='diagnosis'



x_train, x_test, y_train, y_test = train_test_split(df[predictor_var], df[outcome_var], random_state=6, test_size=0.3)

print('Train positivity rate: ')
print(sum(y_train)/y_train.shape[0])
print('Test positivity rate: ')
print(sum(y_test)/y_test.shape[0])

log_reg = LogisticRegression(penalty='none', max_iter=1000, fit_intercept=True, tol=0.000001)
log_reg.fit(x_train, y_train)
y_pred = log_reg.predict(x_test)
recall = recall_score(y_test, y_pred)
accuracy = accuracy_score(y_test, y_pred)
print('Recall and Accuracy scores')
print(recall, accuracy)

  

## 1. Stratified Sampling

x_train_str, x_test_str , y_train_str, y_test_str = train_test_split(df[predictor_var], df[outcome_var], random_state=6, test_size=0.3, stratify=df[outcome_var])

### 2. Stratified positivity rates

print('Stratified train positivity rate: ')
malig_train = [x for x in y_train_str if x == 1]
print(len(malig_train)/len(y_train_str))
print('Stratified test positivity rate: ')
malig_test = [x for x in y_test_str if x==1]
print(len(malig_test)/len(y_train_str))

  

### 3. Model predictions after Stratified sampling

log_reg.fit(x_train_str, y_train_str)
y_pred2 = log_reg.predict(x_test_str)
recall2 = recall_score(y_test_str, y_pred2)
accuracy2 = accuracy_score(y_test_str, y_pred2)

#print('Stratified Sampling: Recall and Accuracy scores')
print('Recall and Accuracy scores')
print(recall2, accuracy2)

### 4. Balanced Class Weights

log_reg = LogisticRegression(penalty='none', max_iter=1000, fit_intercept=True, tol=0.000001, class_weight = 'balanced')


### 5. Model Predictions after balancing Class Weights
log_reg.fit(x_train_str, y_train_str)
y_pred3 = log_reg.predict(x_test_str)
recall3 = recall_score(y_test_str, y_pred3)
accuracy3 = accuracy_score(y_test_str, y_pred3)
#print('Balanced Class Weights: Recall and Accuracy scores')
print('Recall and Accuracy scores')
print(recall3, accuracy3)
```

```
Train positivity rate: 
0.34673366834170855
Test positivity rate: 
0.4327485380116959
Recall and Accuracy scores
0.8783783783783784 0.9181286549707602
Stratified train positivity rate: 
0.37185929648241206
Stratified test positivity rate: 
0.16080402010050251
Recall and Accuracy scores
0.875 0.9239766081871345
Recall and Accuracy scores
0.90625 0.9298245614035088
```



