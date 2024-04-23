___
Similar to regression models, it is important to conduct EDA before fitting a classification model. An EDA should check the assumptions of the classification model, inspect how the data are coded, and check for strong relationships between features.
Suppose we want to build a model to predict whether a patient has heart disease or not based on other characteristics about them. We have downloaded a dataset from the [UCI Machine Learning Repository](http://archive.ics.uci.edu/ml/datasets/Heart+Disease) about heart disease which contains patient information such as:

- `age`: age in years
- `sex`: male (1) or female (0)
- `cp`: chest pain type
- `trestbps`: resting blood pressure (mm Hg)
- `chol`: cholesterol level
- `fbs`: fasting blood sugar level (normal or not)
- `restecg`: resting electrocardiograph results
- `thalach`: maximum heart rate from an exercise test
- `exang`: presence of exercise-induced angina
- `oldpeak`: ST depression induced by exercise relative to rest
- `slope`: slope of peak exercise ST segment
- `ca`: number of vessels colored by flourosopy (0 through 3)
- `thal`: type of defect (3, 6 or 7)
The response variable for this analysis will be `heart_disease`, which we have condensed down to either 0 (if the patient does not have heart disease) or 1 (the patient does have heart disease).

### Preview the data
Just like the EDA process in regression, we preview the data using the `.head()` method in Python and we use the `.info()` method to check the data types, if we see any of the datatypes to be different, we dive a little deeper into that column and check for hidden missing data. 

### Pari plot 
We use a pair plot on the dataframe to understand the relationship between each pair of features in the dataframe and see if we can find any clear distinction between them. 
We can explore the relationships between the different numeric variables using a pair plot. If we also color the observations based on heart disease status, we can simultaneously get a sense for a) which features are most associated with heart disease status and b) whether there are any pairs of features that are jointly useful for determining heart disease status:
![[Pasted image 20240129084020.png]]

In this pair plot, we are looking for patterns between the two color groups. Looking at the density plots along the diagonal, there are no features that cleanly separate the groups (age has the most separation). However, looking at the scatterplot for `age` and `thalach` (maximum heart rate from an exercise test), there is more clear separation. It appears that patients who are old and have low `thalach` are more likely to be diagnosed with heart disease than patients who are young and have high `thalach`. This suggests that we want to make sure both of these features are included in our model.

### Correlation heat map
Similar to linear regression, some classification models assume no multicollinearity in the data, meaning that two highly correlated predictors should not be included in the model. We can check this assumption by looking at a correlation heat map:
![[Pasted image 20240129084117.png]]

There is no set value for what counts as “highly correlated”, however a general rule is a correlation of 0.7 (or -0.7). There are no pairs of features with a correlation of 0.7 or higher, so we do not need to consider leaving any features out of our model based on multicollinearity.

## Classification model results
After this EDA, we can run a principal component analysis (PCA), which attempts to identify which features (or combination of features) are highly related to heart disease. Ideal results of a PCA show one or more pairs of principal components with some separation between the colored groups:
![[Pasted image 20240129084242.png]]

We can see here that there are not any clear separations, which would indicate that this is not an effective analysis. However, we might use the weights of the components to further explore relationships between features and use that in other analyses.