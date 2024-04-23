___
A **Support Vector Machine** (SVM) is a powerful supervised machine learning model used for classification. An SVM makes classifications by defining a decision boundary and then seeing what side of the boundary an unclassified point falls on. Decision boundaries are easiest to wrap your head around when the data has two features. In this case, the decision boundary is a line. Take a look at the example below.

![[Pasted image 20240210101508.png]]

Decision boundaries exist even when your data has more than two features. If there are three features, the decision boundary is now a plane rather than a line.

![[Pasted image 20240210101533.png]]

As the number of dimensions grows past 3, it becomes very difficult to visualize these points in space. Nonetheless, SVMs can still find a decision boundary. However, rather than being a separating line, or a separating plane, the decision boundary is called a _separating hyperplane_.\

## Optimal Decision Boundary 
One problem that SVMs need to solve is figuring out what decision boundary to use. After all, there could be an infinite number of decision boundaries that correctly separate the two classes. Take a look at the image below:
![[Pasted image 20240210101633.png]]

==Maximizing the distance between the decision boundary and points in each class will decrease the chance of false classification.==

## Support Vectors and Margins
The _support vectors_ are the ==points in the training set closest to the decision boundary.== These vectors are crucial in defining the decision boundary — that’s where the “support” comes from. If you are using `n` features, there are at least `n+1` support vectors. ==The distance between a support vector and the decision boundary is called the _margin_==.
![[Pasted image 20240210102013.png]]

>Because the support vectors are so critical in defining the decision boundary, many of the other training points can be ignored. This is one of the advantages of SVMs. Many supervised machine learning algorithms use every training point in order to make a prediction, even though many of those training points aren’t relevant. SVMs are fast because they only use the support vectors!

## Implementing SVM with the help of `scikit-learn`
```Python
from sklearn.svm import SVC
classifier = SVC(kernel = 'linear')
training_points = [[1, 2], [1, 5], [2, 2], [7, 5], [9, 4], [8, 2]]  
labels = [1, 1, 1, 0, 0, 0]  
classifier.fit(training_points, labels)
print(classifier.predict([[3, 2]]))
print(classifier.support_vectors_)
```

```
OUTPUT
[1]
[[7, 5],  
[8, 2],  
[2, 2]]
```

## Outliers
SVMs try to maximize the size of the margin while still correctly separating the points of each class. As a result, outliers can be a problem. Consider the image below.
![[Pasted image 20240210102843.png]]

The size of the margin decreases when a single outlier is present, and as a result, the decision boundary changes as well. However, if we allowed the decision boundary to have some error, we could still use the original line.
SVMs have a parameter `C` that determines how much error the SVM will allow for. If `C` is large, then the SVM has a hard margin — it won’t allow for many misclassifications, and as a result, the margin could be fairly small. If `C` is too large, the model runs the risk of overfitting. It relies too heavily on the training data, including the outliers.
On the other hand, if `C` is small, the SVM has a soft margin. Some points might fall on the wrong side of the line, but the margin will be large. This is resistant to outliers, but if `C` gets too small, you run the risk of underfitting. The SVM will allow for so much error that the training data won’t be represented.

When using scikit-learn’s SVM, you can set the value of `C` when you create the object:

```
classifier = SVC(C = 0.01)
```

## Kernels
Up to this point, we have been using data sets that are linearly separable. This means that it’s possible to draw a straight decision boundary between the two classes. However, what would happen if an SVM came along a dataset that wasn’t linearly separable?
![[Pasted image 20240210103118.png]]

It’s impossible to draw a straight line to separate the red points from the blue points!

Luckily, SVMs have a way of handling these data sets. Remember when we set `kernel = 'linear'` when creating our SVM? Kernels are the key to creating a decision boundary between data points that are not linearly separable.

```Python
from sklearn.svm import SVC
from graph import points, labels
from sklearn.model_selection import train_test_split
training_data, validation_data, training_labels, validation_labels = train_test_split(points, labels, train_size = 0.8, test_size = 0.2, random_state = 100)
classifier = SVC(kernel = 'poly', degree=2 )
classifier.fit(training_data,training_labels)
score = classifier.score(validation_data,validation_labels)
print(score)
```

```
##   
Output-only Terminal

Output:

1.0
```

### Working of a polynomial kernel
We are going to project the 2D data into a 3D data by adding another dimension to each point.
The kernel transforms the data in a clever way to make it linearly separable. We used a polynomial kernel which transforms every point in the following way:
$$(x,y) -> (\sqrt2 *x*y, x^2, y^2)$$
The kernel has added a new dimension to each point! For example, the kernel transforms the point `[1, 2]` like this:
$$ (1,2) -> (2 \sqrt2, 1 , 4) $$
If we plot these new three dimensional points, we get the following graph:
![[Pasted image 20240210103850.png]]
## Radial Basis Function kernel 
The most commonly used kernel in SVMs is a radial basis function (`**rbf**`) kernel. This is the default kernel used in scikit-learn’s `SVC` object. If you don’t specifically set the kernel to `"linear"`, `"poly"` the `SVC` object will use an `rbf` kernel. 

#### The gamma parameter 
```Python
classifier = SVC(kernel = "rbf", gamma = 0.5, C = 2)
```
`gamma` is similar to the `C` parameter. You can essentially tune the model to be more or less sensitive to the training data. A higher `gamma`, say `100`, will put more importance on the training data and could result in overfitting. Conversely, A lower `gamma` like `0.01` makes the points in the training data less relevant and can result in underfitting.
![[Pasted image 20240210104421.png]]

```Python
from data import points, labels
from sklearn.model_selection import train_test_split
from sklearn.svm import SVC

training_data, validation_data, training_labels, validation_labels = train_test_split(points, labels, train_size = 0.8, test_size = 0.2, random_state = 100)

classifier = SVC(kernel = "rbf", gamma=1)
classifier.fit(training_data, training_labels)
print(classifier.score(validation_data, validation_labels))
```



