___
## Confusion Matrix
Let’s say we are fitting a machine learning model to try to predict whether or not an email is spam. We can pass the features of our evaluation set through the trained model and get an output list of the predictions our model makes. We then compare each of those predictions to the actual labels. There are four possible categories that each of the comparisons can fall under:

- True Positive (**TP**): The algorithm predicted spam and it was spam
- True Negative (**TN**): The algorithm predicted not spam and it was not spam
- False Positive (**FP**): The algorithm predicted spam and it was not spam
- False Negative (**FN**): The algorithm predicted not spam and it was spam

One common way to visualize these values is in a **confusion matrix**. In a confusion matrix the predicted classes are represented as columns and the actual classes are represented as rows.

|.|Predicted -|Predicted +|
|---|---|---|
|Actual -|TN|FP|
|Actual +|FN|TP|

## Accuracy, Recall, Precision and F1 score
Once we have a confusion matrix, there are a few different statistics we can use to summarize the four values in the matrix.These include accuracy, precision, recall, and F1 score. We won’t go into much detail about these metrics here, but a quick summary is shown below (T = true, F = false, P = positive, N = negative). For all of these metrics, a value closer to 1 is better and closer to 0 is worse.
 - ##### Accuracy: 
	 -  Accuracy is calculated by finding the total number of correctly classified predictions (true positives and true negatives) and dividing by the total number of predictions.
	 - (TP+TN)/(TP+FP+TN+FN)
 - ##### Precision:
	 - Precision is the ratio of correct positive classifications to all positive classifications made by the model.
	 - (TP/TP+FP)
 - ##### Recall:
	 - Recall is the ratio of correct positive predictions classifications made by the model to all actual positives. For the spam classifier, this would be the number of correctly labeled spam emails divided by all the emails that were actually spam in the dataset.
	 - TP/(TP+FN)
 - ##### F1 score:
	 - The **F1-score** combines both precision and recall into a single statistic, by determining their harmonic mean.
	 - It is the weighted average of precision and recall
	 - $$ F1-score = \frac{2 * precision * recall}{precision+recall}$$

In `sklearn` we can calculate these four statistics by:
```Python
# accuracy:  
from sklearn.metrics import accuracy_score  
print(accuracy_score(y_true, y_pred))  
# output: 0.7  
  
# precision:  
from sklearn.metrics import precision_score  
print(precision_score(y_true, y_pred))  
# output: 0.67  
  
# recall:  
from sklearn.metrics import recall_score  
print(recall_score(y_true, y_pred))  
# output: 0.8  
  
# F1 score  
from sklearn.metrics import f1_score  
print(f1_score(y_true, y_pred))  
# output: 0.73
