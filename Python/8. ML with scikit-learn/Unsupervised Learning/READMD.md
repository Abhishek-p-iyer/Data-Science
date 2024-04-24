## Unsupervised Learning

Unsupervised Learning describes a class of algorithms that find patterns from unlabelled or untagged data. It relies on using the underlying distributions of features within the data to figure out clusters of similarity.
Unsupervised learning methods are extremely important to the functioning of many real-world algorithms - think image recognition, ride-shares anticipating demands, snapchat filters and many more! There are three primary ways they’re used:

- _Clustering_: Identifying clusters within a dataset like identifying disease outbreak clusters or in natural language processing, in creating word clouds that are semantically related, etc.
    
- _Dimensionality Reduction/Feature Extraction_: They can be used to condense the number of features in a dataset with a high number of features before applying a supervised learning algorithm.
    
- _Automated Labelling/Tagging_: Unsupervised learning algorithms are immensely useful in categorizing uncategorized data and one can then perform the familiar classification/regression tasks using supervised learning.

## K-means Clustering
#### Clustering
Often, the data you encounter in the real world won’t be sorted into categories and won’t have labeled answers to your question. Finding patterns in this type of data, unlabeled data, is a common theme in many machine learning applications. _Unsupervised Learning_ is how we find patterns and structure in these data.

**Clustering** is the most well-known unsupervised learning technique. It finds structure in unlabeled data by identifying similar groups, or _clusters_. Examples of clustering applications are: 
- Recommendation systems 
- Search engines
- Market segmentation
- Image segmentation
- Text clustering

In the [[K means Clustering]] algorithm, we separate data in such a way that data similar to each other are placed in the same group.

## Principal Component Analysis
[[Principal component Analysis]] is usually used for dimensionality reduction. PCA is a technique where we can reduce the number of features in a dataset without losing any of the information we have.
