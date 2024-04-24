___
The motivation of Principal Component Analysis (PCA) is to find a new set of features that are ordered by the amount of variation (and therefore, information) they contain. We can then select a subset of these PCA features. This leaves us with lower-dimensional data that still retains most of the information contained in the larger dataset.
## Implementing PCA in `Numpy`
The eigne values and eigen vectors are related to relative variation described by each principle component. The eigen vectors are also know as principal axes. They tell us how to transform (rotate) our data into new features that capture this variation.
To implement this, we will first implement the correlation matrix and use the `np.linalg.eig()` to get the eigen values and eigen vectors. 
```Python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import codecademylib3

data_matrix = pd.read_csv('./data_matrix.csv')

correlation_matrix = data_matrix.corr()
eigenvalues, eigenvectors = np.linalg.eig(correlation_matrix)

print(eigenvalues)
print(eigenvectors)
## Heatmap code:

red_blue = sns.diverging_palette(220, 20, as_cmap=True)
sns.heatmap(correlation_matrix, vmin = -1, vmax = 1, cmap=red_blue)
plt.show()

print('eigenvectors: ')
print('eigenvalues: ')

```

![[Pasted image 20240208165244.png]]

After performing PCA, we generally want to know how useful the new features are. One way to visualize this is to create a scree plot, which shows the proportion of information described by each principal component.

The proportion of information explained is equal to the relative size of each eigenvalue:
```Python
info_prop = eigenvalues / eigenvalues.sum()
print(info_prop)
```
To create a scree plot, we can then plot these relative proportions:
```Python
plt.plot(np.arange(1,len(info_prop)+1),  
         info_prop,  
         'bo-')  
plt.show()
```

![[Pasted image 20240208165548.png]]
From this plot, we see that the first principal component explains about 50% of the variation in the data, the second explains about 30%, and so on.
Another way to view this is to see how many principal axes it takes to reach around 95% of the total amount of information. Ideally, we’d like to retain as few features as possible while still reaching this threshold.

To do this, we need to calculate the cumulative sum of the `info_prop` vector we created earlier:
```Python
cum_info_prop = np.cumsum(info_prop)
```
We can then plot these values using matplotlib:
```Python
plt.plot(np.arange(1,len(info_prop)+1),  
         cum_info_prop,  
         'bo-')  
plt.hlines(y=.95, xmin=0, xmax=15)  
plt.vlines(x=4, ymin=0, ymax=1)  
plt.show()
```

![[Pasted image 20240208165638.png]]

## Implementing PCA with `scikit-learn`
Another way to perform PCA is using the `sklearn.decomposition.PCA` module. 
The steps to perform PCA are:
- Standardize the data
- Decompose the data matrix by fitting the standardized data. We can access the eigen vectors using the `components_` attribute and the proportional sizes of eigen values using the `explained_variance_ratio` attribute.
```Python
import numpy as np
import pandas as pd
from sklearn.decomposition import PCA
import codecademylib3

data_matrix = pd.read_csv('./data_matrix.csv')

# 1. Standardize the data matrix
mean = data_matrix.mean(axis=0)
sttd = data_matrix.std(axis=0)
data_matrix_standardized = (data_matrix - mean)/sttd
print(data_matrix_standardized.head())

# 2. Find the principal components
pca = PCA()
components =  pca.fit(data_matrix_standardized).components_
components = pd.DataFrame(components).transpose()
components.index =  data_matrix.columns
print(components)

# 3. Calculate the variance/info ratios
var_ratio = pca.explained_variance_ratio_
var_ratio = pd.DataFrame(var_ratio).transpose()
print(var_ratio)
```

Example solutions as original solutions were too big to fit into one screenshot 
![[Pasted image 20240208170601.png]]

![[Pasted image 20240208170612.png]]

### Projecting the Data onto the principal Axes
Once we have performed PCA and obtained the eigenvectors, we can use them to project the data onto the first few principal axes. We can do this by taking the dot product of the data and eigenvectors, or by using the `sklearn.decomposition.PCA` module as follows:
```Python
from sklearn.decomposition import PCA  
# only keep 3 PCs  
pca = PCA(n_components = 3)  
# transform the data using the first 3 PCs  
data_pcomp = pca.fit_transform(data_standardized)  
# transform into a dataframe  
data_pcomp = pd.DataFrame(data_pcomp)  
# rename columns  
data_pcomp.columns = ['PC1', 'PC2', 'PC3']  
# print the transformed data  
print(data_pcomp.head())
```

![[Pasted image 20240208170928.png]]

Once we have the transformed data, we can look at a scatter plot of the first two transformed features using seaborn or matplotlib. This allows us to view relationships between multiple features at once in 2D or 3D space. Often, the the first 2-3 principal components result in clustering of the data.

Below, we’ve plotted the first two principal components for a dataset of measurements for three different penguin species:
```Python
sns.lmplot(x='PC1', y='PC2', data=data_pcomp, hue='species', fit_reg=False)  
plt.show()
```

## PCA as features
So far we have used PCA to find principal axes and project the data onto them. We can use a subset of the projected data for modeling, while retaining most of the information in the original (and higher-dimensional) dataset.
For example, recall in the previous exercise that the first four principal axes already contained 95% of the total amount of variance (or information) in the original data. We can use the first four components to train a model, just like we would on the original 16 features.
Because of the lower dimensionality, we should expect training times to be faster. Furthermore, the principal axes ensure that each new feature has no correlation with any other, which can result in better model performance.
In this checkpoint, we will be using the first four principal components as our training data for a Support Vector Classifier (SVC). We will compare this to a model fit with the entire dataset (16 features) using the average likelihood score. Average likelihood is a model evaluation metric; the higher the average likelihood, the better the fit.

```Python
import pandas as pd
from sklearn.decomposition import PCA
from sklearn.svm import LinearSVC
from sklearn.model_selection import train_test_split
data_matrix_standardized = pd.read_csv('./data_matrix_standardized.csv')
classes = pd.read_csv('./classes.csv')
# We will use the classes as y
y = classes.Class.astype('category').cat.codes
# Get principal components with 4 features and save as X
pca_1 = PCA(n_components=4)
X = pca_1.fit_transform(data_matrix_standardized)
# Split the data into 33% testing and the rest training
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)
# Create a Linear Support Vector Classifier
svc_1 = LinearSVC(random_state=0, tol=1e-5)
svc_1.fit(X_train, y_train)
# Generate a score for the testing data
score_1 = svc_1.score(X_test, y_test)
print(f'Score for model with 4 PCA features: {score_1}')
# Split the original data intro 33% testing and the rest training
X_train, X_test, y_train, y_test = train_test_split(data_matrix_standardized, y, test_size=0.33, random_state=42)
# Create a Linear Support Vector Classifier
svc_2 = LinearSVC(random_state=0)
svc_2.fit(X_train, y_train)
# Generate a score for the testing data
score_2 = svc_2.score(X_test, y_test)
print(f'Score for model with original features: {score_2}')
```

```
Score for model with 4 PCA features: 0.4363312555654497
Score for model with original features: 0.39158504007123773
```