___
K-Means clustering a popular algorithm that we use and it tries to address two main questions
- `k`- refers to the number of clusters we expect to find.
- `Means` refers to the average distance of data to the centroid of the cluster which we try to minimize.
The algorithm for K-Means clustering is an iterative process
- Place `k` random centroids for the initial clusters.
- Assign data samples to the nearest centroid.
- Calculate new centroids based on the above-assigned data samples
- Repeat steps 2 and 3 until it converges.

_Convergence_ occurs when points don’t move between clusters and centroids stabilize. This iterative process of updating clusters and centroids is called _training_.
Once we are happy with our clusters, we can take a new unlabeled datapoint and quickly assign it to the appropriate cluster. This is called _inference_.

## Iris Dataset
We're going to implement K means clustering taking the example of the iris dataset. To implement the K-Means clustering algorithm, we are going to follow the steps given down below:
- Randomly initialize the centroids 
- Calculate the distance between each centroid and each data point. 
- Update the centroid based on the above sample 
- Repeat the last two steps until the algorithm converges.

```Python
mport matplotlib.pyplot as plt
import numpy as np
from sklearn import datasets
from copy import deepcopy
iris = datasets.load_iris()
samples = iris.data
x = samples[:,0]
y = samples[:,1]
sepal_length_width = np.array(list(zip(x, y)))

# Step 1: Place K random centroids

k = 3
centroids_x = np.random.uniform(min(x), max(x), size=k)
centroids_y = np.random.uniform(min(y), max(y), size=k)
centroids = np.array(list(zip(centroids_x, centroids_y)))
def distance(a, b):
  one = (a[0] - b[0]) ** 2
  two = (a[1] - b[1]) ** 2
  distance = (one + two) ** 0.5
  return distance

# To store the value of centroids when it updates
centroids_old = np.zeros(centroids.shape)
# Cluster labeles (either 0, 1, or 2)
labels = np.zeros(len(samples))
distances = np.zeros(3)

# Initialize error:
error = np.zeros(3)
for i in range(k):
  error[i] = distance(centroids[i], centroids_old[i])
# Repeat Steps 2 and 3 until convergence:
while error.all!=0:


# Step 2: Assign samples to nearest centroid

  for i in range(len(samples)):
    distances[0] = distance(sepal_length_width[i], centroids[0])
    distances[1] = distance(sepal_length_width[i], centroids[1])
    distances[2] = distance(sepal_length_width[i], centroids[2])
    cluster = np.argmin(distances)
    labels[i] = cluster

# Step 3: Update centroids

  centroids_old = deepcopy(centroids)

  for i in range(3):
    points = [sepal_length_width[j] for j in range(len(sepal_length_width)) if labels[j] == i]
    centroids[i] = np.mean(points, axis=0)

  for i in range(k):
    error[i] = distance(centroids[i], centroids_old[i])
  
colors = ['r','g','b']
for i in range(k):
  points = []
  for j in range(len(sepal_length_width)):
    if lables[j] == i:
      points.append(sepal_length_width[j])
  plt.scatter(points[:,0], points[:,1], c=colors[i], alpha=0.5)
```

## Implementing K-Means with `scikit-learn`

```Python
import matplotlib.pyplot as plt
from sklearn import datasets
from sklearn.cluster import KMeans
# From sklearn.cluster, import Kmeans class
iris = datasets.load_iris()
samples = iris.data
k=3
# Use KMeans() to create a model that finds 3 clusters
model = KMeans(n_clusters = k)
# Use .fit() to fit the model to samples
model.fit(samples)
# Use .predict() to determine the labels of samples
labels = model.predict(samples)
# Print the labels
print(labels)
```

#### Predicting labels of new data
```Python
# Store the new Iris measurements
new_samples = np.array([[5.7, 4.4, 1.5, 0.4],
   [6.5, 3. , 5.5, 0.4],
   [5.8, 2.7, 5.1, 1.9]])

# Predict labels for the new_samples
labels = model.predict(new_samples)
new_names = [iris.target_names[label] for label in labels]
print(new_names)
```

#### Making a scatter plot
```Python
labels = ['r','g','b']
x = samples[:,0]
y = samples[:,1]
plt.scatter(x,y,c = labels, alpha=0.5)
plt.show()
```

![[Pasted image 20240208133550.png]]

#### Evaluation
At this point, we have clustered the Iris data into 3 different groups (implemented using Python and using scikit-learn). But do the clusters correspond to the actual species? Let’s find out!
We will first change the `iris.target` which consists of numerical values into their corresponding categorical data.
According to the metadata:
- All the `0`‘s are _Iris-setosa_
- All the `1`‘s are _Iris-versicolor_
- All the `2`‘s are _Iris-virginica_
Let’s change these values into the corresponding species using the following code:
```Python
species = [iris.target_names[t] for t in list(target)]
```
Now, lets create a dataframe containing labels and species. where labels are the predicted data and the species are the target data.
```Python
df = pd.DataFrame({'labels': labels, 'species': species})  
print(df)
```

Now we use `crosstab()` method to perform cross-tabulation
```Python
ct = pd.crosstab(df['labels'], df['species'])  
print(ct)
```

```
species     setosa  versicolor  virginica
labels                                   
setosa          50           0          0
versicolor       0           2         36
virginica        0          48         14
```

We can then calculate the accuracy of our model

## The number of clusters 
At this point, we have grouped the Iris plants into 3 clusters. But suppose we didn’t know there are three species of Iris in the dataset, what is the best number of clusters? And how do we determine that?

>[!info] How do we define a good cluster 
>Good clustering results in tight clusters, meaning that the samples in each cluster are bunched together. How spread out the clusters are is measured by _inertia_. Inertia is the distance from each sample to the centroid of its cluster. The lower the inertia is, the better our model has done.

You can check the inertia of a model by:
```Python
print(model.inertia_)
```
For the Iris dataset, if we graph all the `k`s (number of clusters) with their inertias:
![[Pasted image 20240208134454.png]]

Ultimately, this will always be a trade-off. If the inertia is too large, then the clusters probably aren’t clumped close together. On the other hand, if there are too many clusters, the individual clusters might not be different enough from each other. The goal is to have low inertia _and_ a small number of clusters.

One of the ways to interpret this graph is to use the _elbow method_: choose an “elbow” in the inertia plot - when inertia begins to decrease more slowly.