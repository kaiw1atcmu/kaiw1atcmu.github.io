---
title: Model Capacity [more to come]
permalink: model_capacity.html
date: 2020-01-04
tags: [machine_learning]
published: false
---

Today we are discussing the relationship between model capacity and generalization error.

## Model Capacity Control
We can control whether a model is more likely to overfit or underfit by altering its capacity. Informally, a model's
capacity is its ability to fit a wide variety of functions. Models with low capacity may struggle to fit the training
set. Models with high capacity can overfit by memorizing properties of the training set that do not serve them well on
the test set.

One way to control the capacity of a learning algorithm is by choosing its hypothesis space, the set of functions that
the learning algorithm is allowed to select as being the solution. Machine learning algorithms will generally perform
best when their capacity is appropriate for the true complexity of the task they need to perform and the amount of
training data they are provided with. Models with insufficient capacity are unable to solve complex tasks. Models with
high capacity can solve complex tasks, but when their capacity is higher than needed to solve the present task they may
overfit.

<img src="{{ "images/20200104-2.png" }}" alt="capacity"/>
_Figure 1: By courtesy of [Deep Learning](#references) Figure 5.2._

On the left, a linear function fit to the data suffers from underfitting, since it cannot capture the curvature that is
present in the data. In the center, a quadratic function fit to the data generalizes well to unseen points. It does not
suffer from a significant amount of overfitting or underfitting. On the right, a polynomial of degree 9 fit to the data
suffers from overfitting. The solution passes through all of the training points exactly, but we have not been lucky
enough for it to extract the correct structure, and it will probably not generalize well on unseen data.

So far we have described only one way of changing a model's capacity: by changing the number of input features it has,
and simultaneously adding new parameters associated with those features. There are in fact many ways of changing a
model's capacity. Capacity is not determined only by the choice of model. In many cases, finding the best function
within this family is a very difficult optimization problem. In practice, the learning algorithm does not actually find
the best function, but merely one that significantly reduces the training error. These additional limitations, such as
the imperfection of the optimization algorithm, mean that the learning algorithm's **effective capacity** may be less
than the **representational capacity** of the model family.

Statistical learning theory provides various means of quantifying model capacity. Among these, the most well-known is
the Vapnik-Chervonenkis dimension, or VC dimension.

## Model Capacity and Error
<img src="{{ "images/20200104-3.png" }}" alt="capacity and error"/>
_Figure 2: By courtesy of [Deep Learning](#references) Figure 5.3._

The graphic illustrates a typical relationship between capacity and error. Training and test error behave differently.
At the left end of the graph, training error and generalization error are both high. This is the underfitting regime. As
we increase capacity, training error decreases, but the gap between training and generalization error increases.
Eventually, the size of this gap outweighs the decrease in training error, and we enter the overfitting regime, where
capacity is too large, above the optimal capacity.

## Model Capacity and Bayes Error
The ideal model is an oracle that simply knows the true probability distribution that generates the data. The error
incurred by an oracle making predictions from the true distribution $p(\mathbf{x},y)$ is called the Bayes error.

<img src="{{ "images/20200104-4.png" }}" alt="bayes error"/>
_Figure 3: By courtesy of [Deep Learning](#references) Figure 5.4._

The top graphic illustrates MSE on the training and test set for two different models: a quadratic model, and a model
with degree chosen to minimize the test error. Both are fit in closed form. For the quadratic model, the training error
increases as the size of the training set increases. This is because larger datasets are harder to fit. Simultaneously,
the test error decreases, because fewer incorrect hypotheses are consistent with the training data. The quadratic model
does not have enough capacity to solve the task, so its test error asymptotes to a high value. The test error at optimal
capacity asymptotes to the Bayes error. The training error can fall below the Bayes error, due to the ability of the
training algorithm to memorize specific instances of the training set. As the training size increases to infinity, the
training error of any fixed-capacity model (here, the quadratic model) must rise to at least the Bayes error.

As the training set size increases, the bottom graphic illustrates the optimal capacity (shown here as the degree of the
optimal polynomial regressor) increases. The optimal capacity plateaus after reaching sufficient complexity to solve the
task.

## Information Criterion
One major drawback of cross-validation is that the number of training runs that must be performed is increased by a
factor of $S$, and this can prove problematic for models in which the training is itself computationally expensive. A
further problem with techniques such as cross-validation that use separate data to assess performance is that we might
have multiple complexity parameters for a single model (for instance, there might be several regularization parameters).
Exploring combinations of settings for such parameters could, in the worst case, require a number of training runs that
is exponential in the number of parameters. Clearly, we need a better approach. Ideally, this should rely only on the
training data and should allow multiple hyperparameters and model types to be compared in a single training run. We
therefore need to find a measure of performance which depends only on the training data and which does not suffer from
bias due to over-fitting.

More to come about AIC and BIC...

## References
Ian Goodfellow, Yoshua Bengio, and Aaron Courville. 2016. Deep Learning. Chapter 5.2.

{% include links.html %}
