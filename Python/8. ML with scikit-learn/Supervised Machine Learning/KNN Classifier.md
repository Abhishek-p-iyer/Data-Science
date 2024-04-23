___
The K-Nearest algorithm classifies data by finding its nearest neighbors and based on the majority, the data is then classified into a particular class. We use k different neighbors to classify our data point. To find the nearest neighbor, we will use the distance formula to find the distance between the unknown data point and its neighbors. We will then filter it down to `k` number of nearest neighbors. 
To implement the KNN algorithm, we do the following
1. Normalize the data
2. Find `k` nearest neighbors
3. Classify the point based on the neighbors

```Python
from movies import training_set, training_labels, validation_set, validation_labels
def distance(movie1, movie2):
  squared_difference = 0
  for i in range(len(movie1)):
    squared_difference += (movie1[i] - movie2[i]) ** 2
  final_distance = squared_difference ** 0.5
  return final_distance
def classify(unknown, dataset, labels, k):
  distances = []
  #Looping through all points in the dataset
  for title in dataset:
    movie = dataset[title]
    distance_to_point = distance(movie, unknown)
    #Adding the distance and point associated with that distance
    distances.append([distance_to_point, title])
  distances.sort()
  #Taling only the k closest points
  neighbors = distances[0:k]
  num_good = 0
  num_bad = 0
  for neighbor in neighbors:
    title = neighbor[1]
    if labels[title] == 0:
      num_bad += 1
    elif labels[title] == 1:
      num_good += 1
  if num_good > num_bad:
    return 1
  else:
    return 0

  

print(validation_set["Bee Movie"])
print(validation_labels["Bee Movie"])
BeeMovie = [0.012279463360232739, 0.18430034129692832, 0.898876404494382]
guess = classify(BeeMovie,training_set, training_labels, 5)
print(guess)
print("Correct!")
```

## Choosing K value
The validation accuracy changes as `k` changes. The first situation that will be useful to consider is when `k` is very small. Let’s say `k = 1`. We would expect the validation accuracy to be fairly low due to _overfitting_. Overfitting is a concept that will appear almost any time you are writing a machine learning algorithm. Overfitting occurs when you rely too heavily on your training data; you assume that data in the real world will always behave exactly like your training data. In the case of K-Nearest Neighbors, overfitting happens when you don’t consider enough neighbors. A single outlier could drastically determine the label of an unknown point.
On the other hand, if `k` is very large, our classifier will suffer from _underfitting_. Underfitting occurs when your classifier doesn’t pay enough attention to the small quirks in the training set. Imagine you have `100` points in your training set and you set `k = 100`. Every single unknown point will be classified in the same exact way. The distances between the points don’t matter at all! This is an extreme example, however, it demonstrates how the classifier can lose understanding of the training data if `k` is too big.

### Graph of K
![[Pasted image 20240202174350.png]]

## `sklearn` for KNN
```Python
from movies import movie_dataset, labels
from sklearn.neighbors import KNeighborsClassifier

classifier = KNeighborsClassifier(n_neighbors = 5)
classifier.fit(movie_dataset, labels)
unknown_points = [[.45, .2, .5],[.25,.8,.9],[.1,.1,.9]]
guesses = classifier.predict(unknown_points)
print(guesses)
```

