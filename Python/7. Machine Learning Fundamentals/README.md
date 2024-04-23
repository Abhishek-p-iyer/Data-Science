## Introduction

Machine Learning algorithms can be classified into two main classes: 
1. Supervised Machine Learning: data is labeled and the program learns to predict the output from the input data
2. Unsupervised Machine Learning: data is unlabeled and the program learns to recognize the inherent structure in the input data

## EDA for Machine Learning Models
Before fitting any model, it is often important to conduct an exploratory data analysis (EDA) in order to check assumptions, inspect the data for anomalies (such as missing, duplicated, or mis-coded data), and inform feature selection/transformation. Hence [[EDA for Machine Learning Models]] is a necessary step that we need to take before we fit out model
- EDA Prior to Fitting a Regression Model
- EDA Prior to Fitting a Classification Model
- EDA Prior to Fitting a Clustering Mode


## Normalization
[[Normalization]]: Many machine learning algorithms attempt to find trends in the data by comparing features of data points. However, there is an issue when the features are on drastically different scales. Hence by normalizing data, each of the datapoints are given equal weightage. 

## Training set Vs Test set
[[Training set Vs Test set]] divides the data into two parts, the training set that we'll use to train our ML model and a test set which we will use to test our ML model based on its accuracy. 

## Feature Selection Methods
There are going to be several redundant or irrelevant features in our dataset, we are going to use the following [[Feature Selection Methods]]  to make sure that our model is fed with a subset of data that is useful for the model
- Filter methods
- Wrapper methods
- Regularization
- Feature importance
- Hyperparameter Tuning

  
## Feature Engineering 
[[Introduction to Feature Engineering]] deals with introduction of feature engineering, types of feature engineering. We will also look into [[Feature Engineering - Numerical Transformation]] which will focus on the following numerical transformation
- Centering
- Standard Scaler
- Min and Max Scaler
- Binning 
- Log transformations


[[Feature Engineering - Categorical Variables]] focuses on transforming categorical data into numerical data because some ML models cannot process text based input. We will learn about
- Original Encoding
- Label Encoding
- One-hot encoding 
- Binary Encoding
- Hashing
- Target Encoding
- Encoding Date-time variables


## Accuracy, Precision  Recall, F1 score
There are four main [[Metrics of ML models]] 
- Accuracy
- Precision
- Recall
- F1 score 
