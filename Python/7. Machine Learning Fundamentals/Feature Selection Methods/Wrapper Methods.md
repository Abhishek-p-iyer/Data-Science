____
Machine learning problems often involve datasets with many features. Some of those features might be very important for a specific machine learning model. Other features might be irrelevant. Given a feature set and a model, we would like to be able to distinguish between important and unimportant features. 
They are feature selection algorithms that evaluate the model based on the subset of features and select the best features based on the performance of the model. 
Wrapper methods have some advantages over filter methods. The main advantage is that wrapper methods evaluate features based on their performance with a specific model. Filter methods, on the other hand, can’t tell how important a feature is to a model. Another upside of wrapper methods is that they can take into account relationships between features. Sometimes certain features aren’t very useful on their own but instead perform well only when combined with other features. Since wrapper methods test subsets of features, they can account for those situations.

We will focus on the following Wrapper methods 
1. [[Sequential forward selection]]
2. [[Sequential backward selection]]
3. [[Sequential forward floating selection]]
4. [[Sequential backward floating selection]]
5. [[Recursive feature elimination]]


>[!info] Before we specify the wrapper method, we will need to specify an ML model.

### Setting up a Logistic Regression mode
Here’s an example of how to do this. The `fire` dataset below was taken from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/547/algerian+forest+fires+dataset) and cleaned for our analysis. Its features are `Temperature`, `RH` (relative humidity), `Ws` (wind speed), `Rain`, `DMC` (Duff Moisture Code), and `FWI` (Fire Weather Index). The final column, `Classes`, contains a `1` if there is a forest fire at a specific location on a given day and `0` if not.
```Python
import pandas as pd  
from sklearn.linear_model import LogisticRegression  

# Load the data  
fire = pd.read_csv("fire.csv")  
# Split independent and dependent variables  
X = fire.iloc[:,:-1]  
y = fire.iloc[:,-1]

# Create and fit the logistic regression model  
lr = LogisticRegression()  
lr.fit(X, y)
print(lr.score(X,y))
```
```
OUTPUT
0.9836065573770492
```

For our testing set, our logistic regression model correctly predicts whether a fire occurred 98.4% of the time.