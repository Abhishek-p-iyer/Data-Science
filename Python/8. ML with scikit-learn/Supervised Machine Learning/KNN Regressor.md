___
The KNN Regressor works similar to the KNN classifier algorithm, where it finds out the neighbors and then averages the rating to give our unknown data a value.

## Weighted Regression
We can be even more clever in the way that we compute the average. We can compute a _weighted_ average based on how close each neighbor is. Let’s say we’re trying to predict the rating of movie X and we’ve found its three nearest neighbors. Consider the following table:
![[Pasted image 20240202183248.png]]

If we find the mean, the predicted rating for X would be 6.93. However, movie X is most similar to movie C, so movie C’s rating should be more important when computing the average. Using a weighted average, we can find movie X’s rating:
$$ weighted \ regression = \frac{(\frac{5.0}{3.2})+(\frac{6.8}{11.5}) + (\frac{9.0}{1.1})}{(\frac{1}{3.2})+(\frac{1}{11.5}) + (\frac{1}{1.1})} = 7.9$$

The numerator is the sum of every rating divided by their respective distances. The denominator is the sum of one over every distance. Even though the ratings are the same as before, the _weighted_ average has now gone up to 7.9.

```Python
from movies import movie_dataset, movie_ratings
def distance(movie1, movie2):
  squared_difference = 0
  for i in range(len(movie1)):
    squared_difference += (movie1[i] - movie2[i]) ** 2
  final_distance = squared_difference ** 0.5
  return final_distance
def predict(unknown, dataset, movie_ratings, k):
  distances = []
  #Looping through all points in the dataset
  for title in dataset:
    movie = dataset[title]
    distance_to_point = distance(movie, unknown)
    #Adding the distance and point associated with that distance
    distances.append([distance_to_point, title])
  distances.sort()
  #Taking only the k closest points
  neighbors = distances[0:k]
  numerator = 0
  denominator = 0
  for neighbor in neighbors:
    rating = movie_ratings[neighbor[1]]
    distance_to_neighbor = neighbor[0]
    numerator += rating / distance_to_neighbor
    denominator += 1 / distance_to_neighbor
  return numerator / denominator
  
print(predict([0.016, 0.300, 1.022], movie_dataset, movie_ratings, 5))
```

## KNN Regressor with `sklearn`
We can also choose whether or not to use a weighted average using the parameter `weights`. If `weights` equals `"uniform"`, all neighbors will be considered equally in the average. If `weights` equals `"distance"`, then a weighted average is used.
```Python
from movies import movie_dataset, movie_ratings
from sklearn.neighbors import KNeighborsRegressor
regressor = KNeighborsRegressor(n_neighbors = 5, weights="distance")
regressor.fit(movie_dataset, movie_ratings)
print(regressor.predict([[0.016, 0.300, 1.022],[0.0004092981, 0.283, 1.0112],[0.00687649, 0.235, 1.0112]]))
```
```
6.84913968 5.47572913 6.91067999]
```

