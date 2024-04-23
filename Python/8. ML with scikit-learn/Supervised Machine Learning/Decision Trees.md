____
We first split the tree into a smaller group based on a feature. Once we have these subsets, we repeat the process — we split the data in each subset again on a different feature. Eventually, we reach a point where we decide to stop splitting the data into smaller groups. We’ve reached a leaf of the tree. We can now count up the labels of the data in that leaf. If an unlabeled point reaches that leaf, it will be classified as the majority label.

![[Pasted image 20240202192838.png]]

## Implementing a Decision tree

```Python
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTree
from sklearn import tree

df = pd.read_csv("<csv file>")
print(df.head())


f['accep'] = ~(df['accep']=='unacc') #1 is acceptable, 0 if not acceptable
X = pd.get_dummies(df.iloc[:,0:6])
y = df['accep']

print(X.columns)
print(len(X.columns))

x_train, x_test, y_train, y_test = train_test_split(X,y, random_state=0, test_size=0.2)


dt = DecisionTreeClassifier(max_depth=3, ccp_alpha=0.01,criterion='gini')
dt.fit(x_train, y_train)

plt.figure(figsize=(20,12))
tree.plot_tree(dt, feature_names = x_train.columns, max_depth=5, class_names = ['unacc', 'acc'], label='all', filled=True)
plt.tight_layout()
plt.show()
```

![[Pasted image 20240202193444.png]]

1. The root node is identified as the top of the tree. This is notated already with the number of samples and the numbers in each class (i.e. unacceptable vs. acceptable) that was used to build the tree.
2. Splits occur with True to the left, False to the right. Note the right split is a `leaf node` i.e., there are no more branches. Any decision ending here results in the majority class. (The majority class here is `unacc`.)

## Gini impurity
Consider the two trees below. Which tree would be more useful as a model that tries to predict whether someone would get an A in a class?
Let’s say you use the top tree. You’ll end up at a leaf node where the label is up for debate. The training data has labels from both classes! If you use the bottom tree, you’ll end up at a leaf where there’s only one type of label. There’s no debate at all! We’d be much more confident about our classification if we used the bottom tree.
This idea can be quantified by calculating the _Gini impurity_ of a set of data points. For two classes (1 and 2) with probabilities `p_1` and `p_2` respectively, the Gini impurity is:

$$  1 - (p_1^2 + p_2^2) = (1 - (p_1^2 +(1-p_1^2))$$

![[Pasted image 20240202202508.png]]

The goal of a decision tree model is to separate the classes the best possible, i.e. minimize the impurity (or maximize the purity). Notice that if `p_1` is 0 or 1, the Gini impurity is `0`, which means there is only one class so there is perfect separation. From the graph, the Gini impurity is maximum at `p_1=0.5`, which means the two classes are equally balanced, so this is perfectly impure!

In general, the Gini impurity for C classes is defined as:
$$ 1 - \sum p_i^2$$
## Information Gain
We know that we want to end up with leaves with a low Gini Impurity, but we still need to figure out which features to split on in order to achieve this. To answer this question, we can calculate the _information gain_ of splitting the data on a certain feature. Information gain measures the difference in the impurity of the data before and after the split.
For example, let’s start with the root node of our car acceptability tree:
![[Pasted image 20240202202826.png]]

The initial Gini impurity (which we confirmed previously) is 0.418. The first split occurs based on the feature `safety_low<=0.5`, and as this is a dummy variable with values 0 and 1, this split is pushing higher safety cars to the left (912 samples) and low safety cars to the right (470 samples). Before we discuss how we decided to split on this feature, let’s calculate the information gain.
The new Gini impurities for these two split nodes are 0.495 and 0 (which is a pure leaf node!). All together, the now weighted Gini impurity after the split is:
$$ \frac{912}{1328}*0.495 + \frac{470}{1328}*0 = 0.3267$$

Not bad! (Remember we _want_ our Gini impurity to be lower!) This is lower than our initial Gini impurity, so by splitting the data in that way, we’ve gained some information about how the data is structured — the datasets after the split are purer than they were before the split.
Then the information gain (or reduction in impurity after the split) is
$$  0.418 - 0.3267 = 0.0918$$

>The higher the information gain the better 
> If the information gain is 0, then there is no use in splitting the data

## How is a Decision tree built? (Feature split)
We now consider an important question: **How does one know that this is the best node to split on?!** To figure this out we’re going to go through the process of calculating information gain for other possible root node choices and calculate the information gain values for each of these. This is precisely what is going on under the hood when one runs a `DecisionTreeClassifier()` in `scikit-learn`. By checking information gain values of all possible options at any given split, the algorithm decide on the best feature to split on at every node.

Now that we can find the best feature to split the dataset, we can repeat this process again and again to create the full tree. This is a recursive algorithm! We start with every data point from the training set, find the best feature to split the data, split the data based on that feature, and then recursively repeat the process again on each subset that was created from the split.

We’ll stop the recursion when we can no longer find a feature that results in any information gain. In other words, we want to create a leaf of the tree when we can’t find a way to split the data that makes purer subsets.

## Advantages and Disadvantages
As we have seen already, decision trees are easy to understand, fully explainable, and have a natural way to visualize the decision making process. In addition, often little modification needs to be made to the data prior to modeling (such as scaling, normalization, removing outliers) and decision trees are relatively quick to train and predict. However, now let’s talk about some of their limitations.

One problem with the way we’re currently making our decision trees is that our trees aren’t always _globally optimal_. This means that there might be a better tree out there somewhere that produces better results. But wait, why did we go through all that work of finding information gain if it’s not producing the best possible tree?

Our current strategy of creating trees is _greedy_. We assume that the best way to create a tree is to find the feature that will result in the largest information gain _right now_ and split on that feature. We never consider the ramifications of that split further down the tree. It’s possible that if we split on a suboptimal feature right now, we would find even better splits later on. Unfortunately, finding a globally optimal tree is an extremely difficult task, and finding a tree using our greedy approach is a reasonable substitute.

Another problem with our trees is that they are prone to _overfit_ the data. This means that the structure of the tree is too dependent on the training data and may not generalize well to new data. In general, larger trees tend to overfit the data more. As the tree gets bigger, it becomes more tuned to the training data and it loses a more generalized understanding of the real world data.