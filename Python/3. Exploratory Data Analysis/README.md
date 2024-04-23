_Exploratory Data Analysis_ (EDA for short) is all about getting curious about your data – finding out what is there, what patterns you can find, and what relationships exist. EDA is the important first step towards analysis and model building. When done well, it can help you formulate further questions and areas for investigation, and it almost always helps uncover aspects of your data that you wouldn’t have seen otherwise.

Before we start the EDA process, lets look into the fundamentals of [[Variable Types]]. There are two kinds of variable types that exist in Python 
- Categorical: The data types that provide information. There are three types of Categorical variables 
	- Nominal 
	- Ordinal 
	- Binary
- Numerical: The data types that are used for analysis. There are two types of Numerical data types
	- Continuous 
	- Discrete

## Goals of EDA 
- Uncover the data by finding patterns
- Using visualization tools to detect relationships 
- Summarize columns to understand the data and detect missing values, outliers and other anomalies. 
- Find new avenues for further research
- Prepare for model building 
	- Check assumptions 
	- Select features 
	- Choose an appropriate method

## EDA: Inspect, Clean, and Validate a Dataset 
One of the most challenging parts of data cleaning is diagnosing data issues and figuring out HOW to most effectively address them. In order to accomplish this, exploratory data analysis (EDA) can be an extremely useful tool. EDA is all about following the data, verifying your assumptions, and investigating anything that is unexpected. The article [[Inspect, Clean and Validate]] will go in depth into what to look for in a dataset before the EDA process begins. 

## EDA: Summary Statistics 
Summary Statistics is an important part of data analysis where we condense large amount of data into a small set of numbers that can be easily interpreted. There are different two kinds of summary statistics that can be performed depending upon the number of variables.
- Univariate Statistics 
	- Univariate Statistic for Categorical Variables 
	- Univariate Statistics for Numerical Variables 
- Bivariate Statistics 
	- Bivariate statistics between one categorical variable and one quantitative variable 
	- Bivariate statistics between two categorical variables 
	- Bivariate statistics between two quantitative variables 

## Summarizing a single feature
While [[Summarizing a single features]], we will look into central locations and spread along with some visualizations. We will also look into getting a tabular form for value proportions. 
## Summarizing the relationship between two features
While [[Summarizing the relationship between two features]] there are three possible scenarios that can take place:
- The association between a quantitative and categorical variable 
- The association between two quantitative variables 
- The association between two categorical variables 
