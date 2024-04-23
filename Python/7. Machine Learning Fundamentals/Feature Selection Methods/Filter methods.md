___
**Filter methods** are a type of feature selection method that works by selecting features based on some criteria prior to building the model. Because they don’t involve actually testing the subsetted features using a model, they are computationally inexpensive and flexible to use for any type of machine learning algorithm. This makes filter methods an efficient initial step for narrowing down the pool of features to only the most relevant, predictive ones.
There are several filter methods that we can use, we will learn about variance threshold, mutual information, correlation to rank and select the top features. 

## Dataset
Let’s suppose we have the following dataset containing information on a class of middle school students:
```Python
import pandas as pd  
df = pd.DataFrame(data={  
    'edu_goal': ['bachelors', 'bachelors', 'bachelors', 'masters', 'masters', 'masters', 'masters', 'phd', 'phd', 'phd'],  
    'hours_study': [1, 2, 3, 3, 3, 4, 3, 4, 5, 5],  
    'hours_TV': [4, 3, 4, 3, 2, 3, 2, 2, 1, 1],  
    'hours_sleep': [10, 10, 8, 8, 6, 6, 8, 8, 10, 10],  
    'height_cm': [155, 151, 160, 160, 156, 150, 164, 151, 158, 152],  
    'grade_level': [8, 8, 8, 8, 8, 8, 8, 8, 8, 8],  
    'exam_score': [71, 72, 78, 79, 85, 86, 92, 93, 99, 100]  
})  
print(df)
```
![[Pasted image 20240204074249.png]]

Our goal is to use the data to predict how well each student will perform on the exam. Thus, our target variable is `exam_score` and the remaining 6 variables are our features. We’ll prepare the data by separating the features matrix (`X`) and the target vector (`y`):

```Python
X = df.drop(columns = ['exam_score'])
y = df['exam_score']
```

## Variance threshold
One of the most common filtering methods is the variance threshold and it works on the principle that features with less variance cause very little information gain. Hence it removes the features that have a low variance.  Since variance can only be calculated on numeric values, this method only works on quantitative features. If we want to filter out categorical data, we would need to convert the categorical data into a dummy variable and we remove the values that have all or a majority of the values as the same. 
In our example dataset, `edu_goal` is the only feature which is categorical, we will remove this feature temporarily, apply the variance threshold and add it back. 
```Python 
from sklearn.feature_selector import VarianceThreshold

X_num = X.drop(columns = ['edu_goal'])
selector = VaraianceThreshold(threshold = 0)
print(selector.fit_transform(X_num)) #Returns the filtered features as a numpy array
```

```
[[  1   4  10 155]  
[  2   3  10 151]  
[  3   4   8 160]  
[  3   3   8 160]  
[  3   2   6 156]  
[  4   3   6 150]  
[  3   2   8 164]  
[  4   2   8 151]  
[  5   1  10 158]  
[  5   1  10 152]]
```

As we can see, `grade_level` was removed because there is no variation in its values — all students are 8th graders. Since this data is the same across the board, a student’s grade level will not be able to provide any useful predictive information about their exam score, so it makes sense to drop `grade_level` as a feature.

> From a human perspective, one downside of working with numpy arrays as compared to pandas DataFrame is that we lose information like column headings, making the data harder to visually inspect. Luckily, `VarianceThreshold` offers another method called `.get_support()` that can return the indices of the selected features, which we can use to manually subset our numeric features DataFrame:

```Python
print(selector.get_support(indices=True))
# OUTPUT
[0 1 2 3]

num_cols = list(X_num.columns[selector.get_support(indices=True)])
print(num_cols)

# ['hours_study', 'hours_TV', 'hours_sleep', 'height_cm']

X_num = X_num[num_cols]
print(X_num)
```

![[Pasted image 20240204075756.png]]

Finally, to obtain our entire features DataFrame, including the categorical column `edu_goal`, we could do:
```Python
X = X[['edu_goal']+num_cols]
print(X)
```

![[Pasted image 20240204075910.png]]

## Pearson's correlation
Another type of filtering method is to find the correlation between variables.  A coefficient close to 1 represents a positive correlation whereas a coefficient close to -1 represents a negative correlation. Like variance, Pearson’s correlation coefficient cannot be calculated for categorical variables.
There are 2 main ways of using correlation for feature selection — to detect correlation between features and to detect correlation between a feature and the target variable.

#### Correlation between features
When two features are highly correlated with one another, then keeping just one to be used in the model will be enough because otherwise they provide duplicate information. The second variable would only be redundant and serve to contribute unnecessary noise.
To determine which variables are correlated with one another, we can use the [`.corr()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.corr.html) method from `pandas` to find the correlation coefficient between each pair of numeric features in a DataFrame. By default, `.corr()` computes the Pearson’s correlation coefficient, but alternative methods can be specified using the `method` parameter. We can visualize the resulting correlation matrix using a heatmap:
```Python
import matplotlib.pyplot as plt
import seaborn as sns

corr_matrix = X_num.corr(method = 'pearson')
sns.heatmap(corr_matrix, annot =True, cmap = 'EdBu_r')
plt.show()
```
![[Pasted image 20240204081218.png]]

Let’s define high correlation as having a coefficient of greater than `0.7` or less than `-0.7` . We can loop through the matrix to identify the correlation matrix that has a high correlation coefficient.
```Python
# Loop over bottom diagonal of correlation matrix  
for i in range(len(corr_matrix.columns)):  
    for j in range(i):  
  
        # Print variables with high correlation  
        if abs(corr_matrix.iloc[i, j]) > 0.7:  
            print(corr_matrix.columns[i], corr_matrix.columns[j], corr_matrix.iloc[i, j])
```

```
hours_TV hours_study -0.780763315142435
```

As seen, `hours_TV` appears to be highly negatively correlated with `hours_study` — a student who watches a lot of TV tends to spend fewer hours studying, and vice versa. Because they provide redundant information, we can choose to remove one of those variables. To decide which one, we can look at their correlation with the target variable, then remove the one that is less associated with the target. This is explored in the next section.

#### Correlation between target and feature
As mentioned, the second way correlation can be used is to determine if there is a relationship between a feature and the target variable. In the case of Pearson’s correlation, this is especially useful if we intend to fit a linear model, which assumes a linear relationship between the target and predictor variables. If a feature is not very correlated with the target variable, such as having a coefficient of between `-0.3` and `0.3`, then it may not be very predictive and can potentially be filtered out.

We can use the same `.corr()` method seen previously to obtain the correlation between the target variable and the rest of the features. First, we’ll need to create a new DataFrame containing the numeric features with the `exam_score` column:
```Python
X_y = X_num.copy()
X_y['exam_score'] = y

target_corr = X_y.corr(method = 'pearson')
corr_target = corr_matrix[['exam_score']].drop(labels=['exam_score'])  
sns.heatmap(corr_target, annot=True, fmt='.3', cmap='RdBu_r')  
plt.show()
```
![[Pasted image 20240204081237.png]]

- As seen, `hours_study` is positively correlated with `exam_score` and `hours_TV` is negatively correlated with it. It makes sense that `hours_study` and `hours_TV` would be negatively correlated with each other as we saw earlier, and just one of those features would suffice for predicting `exam_score`. Since `hours_study` has a stronger correlation with the target variable.
- The other two features, `hours_sleep` and `height_cm`, both do not seem to be correlated with `exam_score`, suggesting they would not be very good predictors. We could potentially remove either or both of them as being uninformative. But before we do, it is a good idea to use other methods to double check that the features truly are not predictive

To conclude this section, we’ll briefly note an alternative approach for assessing the correlation between variables. Instead of generating the full correlation matrix, we could use the [`f_regression()`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.f_regression.html) function from `scikit-learn` to find the F-statistic for a model with each predictor on its own. The F-statistic will be larger (and p-value will be smaller) for predictors that are more highly correlated with the target variable, thus it will perform the same filtering:
```Python
from sklearn.feature_selection import f_regression  
print(f_regression(X_num, y))
```
```
(array([3.61362007e+01, 3.44537037e+01, 0.00000000e+00, 1.70259066e-03]),  
array([3.19334945e-04, 3.74322763e-04, 1.00000000e+00, 9.68097878e-01]))
```

The function returns the F-statistic in the first array and the p-value in the second. As seen, the result is consistent with what we had observed in the correlation matrix — the stronger the correlation (either positive or negative) between the feature and target, the higher the corresponding F-statistic and lower the p-value. For example, amongst all the features, `hours_study` has the largest correlation coefficient (`0.905`), highest F-statistic (`3.61e+01`), and lowest p-value (`3.19e-04`).

## Mutual Information
 Mutual Information is a measure of dependence between two variables and can be used to gauge how much a feature contributes to the prediction of the target variable. It is similar to Pearson’s correlation, but is not limited to detecting linear associations. This makes mutual information useful for more flexible models where a linear functional form is not assumed. Another advantage of mutual information is that it also works on discrete features or target, unlike correlation. Although, categorical variables need to be numerically encoded first.
 In our example, we can encode the `edu_goal` column using the `LabelEncoder` class from `scikit-learn`‘s `preprocessing` module:
```Python
from sklearn.preprocessing import LabelEncoder  
le = LabelEncoder()  
# Create copy of `X` for encoded version  
X_enc = X.copy()  
X_enc['edu_goal'] = le.fit_transform(X['edu_goal'])  
print(X_enc)
```

![[Pasted image 20240204082116.png]]

Now, we can compute the mutual information between each feature and `exam_score` using [`mutual_info_regression()`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.mutual_info_regression.html). This function is used because our target variable is continuous, but if we had a discrete target variable, we would use [`mutual_info_classif()`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.mutual_info_classif.html). We specify the `random_state` in the function in order obtain reproducible results:
```Python
from sklearn.feature_selection import mutual_info_regression  
print(mutual_info_regression(X_enc, y, random_state=68))
```
```
[0.50396825 0.40896825 0.06896825 0.        ]
```

The estimated mutual information between each feature and the target is returned in a numpy array, where each value is a non-negative number — the higher the value, the more predictive power is assumed.
However, we are missing one more important piece here. Earlier, even though we encoded `edu_goal` to be numeric, that does not mean it should be treated as a continuous variable. In other words, the values of `edu_goal` are still discrete and should be interpreted as such.
In order to ==properly calculate the mutual information==, we need to tell `mutual_info_regression()` which features are discrete by providing their index positions using the `discrete_features` parameter:
```Python
print(mutual_info_regression(X_enc, y, discrete_features=[0], random_state=68))
```
```
[0.75563492 0.38896825 0.18563492 0.        ]
```

Compared to the earlier results, we now get greater mutual information between `edu_goal` and the target variable once it is correctly interpreted as a discrete feature.
From the results, we can also see that there is `0` mutual information between `height_cm` and `exam_score`, suggesting that these variables are largely independent. This is consistent with what we saw earlier with Pearson’s correlation, where the correlation coefficient between them is very close to `0` as well.
What is interesting to note is that the mutual information between `hours_sleep` and `exam_score` is a positive value, even though their Pearson’s correlation coefficient is `0`. The answer becomes more clear when we plot the relationship between `hours_sleep` and `exam_score`:
![[Pasted image 20240204082511.png]]

As seen, there do seem to be some association between the variables, only it is not a linear one, which is why it was detected using mutual information but not Pearson’s correlation coefficient.

Finally, let’s look at using the [`SelectKBest`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectKBest.html) class from `scikit-learn` to help pick out the top `k` features with the highest ranked scores. In our case, we are looking to select features that share the most mutual information with the target variable. When we instantiate `SelectKBest`, we’ll specify which scoring function to use and how many top features to select. Here, our scoring function is `mutual_info_regression()`, but because we want to specify additional arguments besides the `X` and `y` inputs, we’ll need the help of the [`partial()`](https://docs.python.org/3/library/functools.html#functools.partial) function from Python’s built-in `functools` module. Then, the `.fit_transform()` method will return the filtered features as a numpy array:
```Python
from sklearn.feature_selection import SelectKBest  
from functools import partial  
  
score_func = partial(mutual_info_regression, discrete_features=[0], random_state=68)  
  
# Select top 3 features with the most mutual information  
selection = SelectKBest(score_func=score_func, k=3)  
  
print(selection.fit_transform(X_enc, y))
```
```
OUTPUT
[[ 0  1 10]  
[ 0  2 10]  
[ 0  3  8]  
[ 1  3  8]  
[ 1  3  6]  
[ 1  4  6]  
[ 1  3  8]  
[ 2  4  8]  
[ 2  5 10]  
[ 2  5 10]]
```

As seen above, we selected the top 3 features based on mutual information, thus dropping `height_cm`. Like `VarianceThreshold`, `SelectKBest` also offers the `.get_support()` method that returns the indices of the selected features, so we could subset our original features DataFrame:
```pYTHON
X = X[X.columns[selection.get_support(indices=True)]]  
print(X)
```

![[Pasted image 20240204082750.png]]