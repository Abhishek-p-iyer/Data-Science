## Ensemble Learning

In ensembling, often the base models that are chosen tend to underachieve on their own and are typically referred to as **weak learners**. The reason this is preferable is to have the model implementation be computationally efficient. Combining strong learners doesn’t necessarily make the resultant ensembled model more performant so one might as well choose learners that cost less computationally.

What makes a model a “weak learner”? There are many ways in which a base model can be considered weak. For example, it can have high bias or high variance. The nature of the weakness of the base model is typically taken into consideration and is a design choice when determining the best ensembling method to use.

The basic components of ensemble learning are:

- Base models that are weak learners
- An ensembling method that combines the base models to improve performance and robustness

It is possible to have an ensemble model that performs worse than any one of its contributing base estimators. To circumvent this outcome it is important that the base estimators are uncorrelated and independent. Having a higher diversity among the trained base estimators leads to a stronger ensemble model.

Three common ensembling methods in machine learning that will be covered briefly in this article and later on in separate modules are [[Bagging]], [[Boosting]], and [[Stacking]].

## Bagging 
- Bagging also known as Bootstrap Aggregating is an ensemble method that combines the concepts of Bootstrapping and Aggregating. 
- It can be used for both regression and classification
- Bagging methods use weak learners as base estimators that are complex and suffer from a high variance 
- Bagging is a learning technique that is done in parallel. Each of the base models is trained independently of the others. Additionally, each base model is trained using only a subset of the original features. Even though the base models tend to be complex, they overfit to both a subset of the available training data and a subset of the available features. This allows them to be diverse from one another, often leading to a very strong ensemble model when aggregated. In the Bagging panel of the figure, the base models are decision trees that are relatively large and overfit to the bootstrapped subset of data provided to each of them
- Once each of the base models is trained, the method for ensembling tends to be a simple aggregation technique over each of the models; a majority vote for classification problems and averaging for regression problems.

## Boosting 
**Boosting** is an ensemble learning technique where the weak learners are too simple and tend to suffer from high bias. In the Boosting panel of the figure above, the base models are decision trees with only one level, a decision stump. Decision stumps can only make a decision based off of one feature at a time, causing them to underfit the data substantially.

Boosting is a sequential learning technique where each of the base models builds off the previous model. Each subsequent model aims to improve the performance of the final ensembled model by attempting to fix the errors in the previous stage.

In the Boosting panel of the figure, you may notice that some of the candies atop the cookies are larger than the others. These particular training instances were misclassified by the previous decision stump and are therefore given more weight by the next decision stump. This is one method in which boosting methods may learn from their mistakes.

## Stacking 
**Stacking** is an extremely flexible ensembling technique where a final model is trained to learn how to best combine a set of base models to make strong predictions. In contrast to bagging and boosting, the base models in stacking do not need to be the same type of learning algorithm.

While bagging and boosting are built with base models that are weak learners, that does not necessarily have to be the case for stacking. A stacking algorithm can be used to combine decently performing learners as well.
