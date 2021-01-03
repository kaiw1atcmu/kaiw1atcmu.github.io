---
title: Regularization Techniques [more to come]
permalink: regularization_techniques.html
date: 2020-01-22
tags: [machine_learning,deep_learning]
published: false
---

Today we try to introduce an important concepts in machine learning, that is, the regularization.

## Regularization Relates to Overfitting
In training machine learning algorithms, one key concept, called overfitting, refers to the problem that occurs when the
gap between the training error and test error starts to increase while the training is in progress. Another key concept,
called regularization, is broadly defined to be any modification we make to a learning algorithm that is intended to
reduce its generalization error but not its training error. In this sense, regularization techniques are approximately
tantamount to any practical approaches which might result in reduced overfitting.

In [Deep Learning](#references), we focus on three situations, where the model family being trained either (1)
excludes the true data generating process, corresponding to underfitting and inducing bias, or (2) matches the true
data generating process, or (3) includes the generating process but also many other possible generating processes,
the overfitting regime where variance rather than bias dominates the estimation error. The goal of regularization is
to take a model from the third regime into the second regime.

<img src="{{ "images/20200122-1.png" }}" alt="regularization"/>
$\leftarrow\text{more regularization}\quad----------\quad\text{less regularization}\rightarrow$  
_Figure 1: Regularization. By courtesy of [Deep Learning](#references) Figure 5.6._

In the above figure, although the horizontal axis denotes model capacity, the relationship of the curves should remain
much the same when the horizontal axis comes to denote the amount of regularization. In the context of deep learning,
most regularization strategies are based on regularizing estimators. Regularization of an estimator works by trading
increased bias for reduced variance.

### Parameter Norm Regularization
In this section, we discuss the effects of the various norms when used as penalties on the model parameters. Before
delving into the regularization behavior of different norms, we note that for neural networks, we typically choose to
penalizes only the weights $\mathbf{w}$ of the affine transformation at each layer and leaves the biases $b$
unregularized. [Christopher M. Bishop](#references) gave a mathematical analysis, showing that inclusion of $b$ in the
regularizer arbitrarily favors one solution over another, equivalent one. [Ian Goodfellow](#references) shows that we do
not induce too much variance by leaving the biases unregularized. Also, regularizing the bias parameters can introduce a
significant amount of underfitting.

### $L_2$ Norm Regularization
The $L_2$ parameter norm penalty is more commonly known as **weight decay** in the context of neural networks. This
regularization strategy drives the weights closer to the origin by adding a regularization term
$\Theta(\mathbf{w})=\frac{1}{2}\|\mathbf{w}\|_2^2$ to the objective function. According to Ian Goodfellow, we could
regularize the parameters to be near any specific point in space and, surprisingly, still get a regularization effect,
but better results will be obtained for a value closer to the true one, with zero being a default value that makes sense
when we do not know if the correct value should be positive or negative. Techniques such as this are known in the
statistics literature as **shrinkage** methods because they reduce the value of the coefficients. In particular, $L_2$
regularization is also known as **ridge regression** or **Tikhonov regularization**.

The $L_2$ regularizer will not penalize weight components even-handedly. It imposes less penalty on components of
parameter $\mathbf{w}$ which contribute significantly to reducing the objective function, and more penalty on those
that contribute only marginally to reducing the objective function.

Many regularization strategies can be interpreted as MAP Bayesian inference, and that in particular, $L_2$
regularization is equivalent to MAP Bayesian inference with a Gaussian prior on the weights.

### $L_1$ Norm Regularization
Another option is to use $L_1$ regularization. Formally, $L_1$ regularization on the model parameter $\mathbf{w}$ is
defined as $\Theta(\mathbf{w})=\|\mathbf{w}\|_1$. As with $L_2$ regularization, we could regularize the parameters
towards a value that is not zero, but instead towards some parameter nonzero value. In comparison to $L_2$
regularization, $L_1$ regularization results in a solution that is more sparse. The sparsity property induced by
$L_1$ regularization has been used extensively as a feature selection mechanism.

The $L_1$ regularization is equivalent to the log-prior term that is maximized by MAP Bayesian inference when the prior
is an isotropic Laplace distribution over the parameters $\mathbf{w}$:
$$
    {\log p(\mathbf{w})=\sum_i\log\text{Laplace}(w_i;0,\frac{1}{\alpha})=-\alpha\|\mathbf{w}\|+n\log\alpha-n\log 2.}
$$

### Max-Norm Regularization
The max-norm regularization requires $\Theta(\mathbf{w})$ to be less than some constant $k$, a standard constrained
optimization problem that could be approached with Lagrangian multipliers under the KKT condition. We can modify
algorithms such as stochastic gradient descent to take a step downhill on the cost function, and then project
$\mathbf{w}$ back to the nearest point that satisfies $\Theta(\mathbf{w}) < k$. Another reason is that enforcing
constraints with penalties can cause non-convex optimization procedures to get stuck in local minima corresponding to
small $\mathbf{w}$. Finally, explicit constraints with reprojection can be useful because they impose some stability on
the optimization procedure. <a href="#hinton2012c">Hinton et al.</a> recommend using constraints combined with a high
learning rate to allow rapid exploration of parameter space while maintaining some stability. The same literature also
recommended constraining the norm of each column separately that prevents any one hidden unit from having very large
weights, with a separate KKT multiplier for the weights of each hidden unit.

### Least Absolute Shrinkage and Selection Operator (LASSO)

### Elastic Net

## Dataset Augmentation
The best way to make a machine learning model generalize better is to train it on more data. more to come...

## Noise Injection
For some models, the addition of noise with infinitesimal variance at the input of the model is equivalent to imposing a
penalty on the norm of the weights. more to come...

## Semi-Supervised Learning
In the paradigm of semi-supervised learning, both unlabeled examples and labeled examples are used to make a
discriminative estimate. more to come...

## Multi-Task Learning
Multi-task learning is a way to improve generalization by pooling the examples (which can be seen as soft constraints
imposed on the parameters) arising out of several tasks. more to come...

## Early Stopping
When training large models with sufficient representational capacity to overfit the task, we often observe that training
error decreases steadily over time, but validation set error begins to rise again. This means we can obtain a model with
better validation set error (and thus, hopefully better test set error) by returning to the parameter setting at the
point in time with the lowest validation set error. This strategy is known as early stopping. It is probably the most
commonly used form of regularization in deep learning. Its popularity is due both to its effectiveness and its
simplicity.

Early stopping acts as a regularizer. That is, under certain assumptions, the number of training iterations $\tau$ plays
a role inversely proportional to the $L_2$ regularization parameter, and the inverse of $\tau\epsilon$ plays the role of
the weight decay coefficient $\alpha$.

This simple procedure is complicated in practice by the fact that the validation dataset's error may fluctuate during
training, producing multiple local minima. This complication has led to the creation of many ad-hoc rules for deciding
when overfitting has truly begun.

## Parameter Tying and Parameter Sharing
A common type of dependency that we often want to express is that certain parameters should be close to one another.
Formally, we have model $A$ with parameters $\mathbf{w}(A)$ and model $B$ with parameters $\mathbf{w}(B)$. Let us
imagine that the tasks are similar enough (perhaps with similar input and output distributions) that we believe the
model parameters should be close to each other, then we can use a parameter norm penalty of the form
$$
    {\Theta(\mathbf{w}(A),\mathbf{w}(B))=\|\mathbf{w}(A)−\mathbf{w}(B)\|_2^2.}
$$
This kind of approach was proposed by <a href="#lasserre2006">Lasserre et al.</a>, who regularized the parameters of one
model, trained as a classifier in a supervised paradigm, to be close to the parameters of another model, trained in an
unsupervised paradigm (to capture the distribution of the observed input data). The architectures were constructed such
that many of the parameters in the classifier model could be paired to corresponding parameters in the unsupervised
model.

While a parameter norm penalty is one way to regularize parameters to be close to one another, the more popular way is
to use constraints, to force sets of parameters to be equal. This method of regularization is often referred to as
**parameter sharing**, made popular by **convolutional neural networks (CNNs)** applied to computer vision.

## Sparse Representations
Weight decay acts by placing a penalty directly on the model parameters. Another strategy is to place a penalty on the
activations of the units in a neural network, encouraging their activations to be sparse. more to come...

## Bagging and Other Ensemble Methods
Bagging (short for **bootstrap aggregating**) is a technique for reducing generalization error by combining several
models. This is an example of a general strategy in machine learning called **model averaging**. Techniques employing
this strategy are known as **ensemble methods**.

Model averaging is an extremely powerful and reliable method for reducing generalization error. Its use is usually
discouraged when benchmarking algorithms for scientific papers, because any machine learning algorithm can benefit
substantially from model averaging at the price of increased computation and memory. For this reason, benchmark
comparisons are usually made using a single model.

Not all techniques for constructing ensembles are designed to make the ensemble more regularized than the individual
models. For example, a technique called **boosting** constructs an ensemble with higher capacity than the individual
models.

## Dropout
Dropout provides a computationally inexpensive but powerful method of regularizing a broad family of models. To a first
approximation, dropout can be thought of as a method of making bagging practical for ensembles of very many large neural
networks. (Because of its importance, we have devoted an entire post to demonstrating the topic of dropout.)

## Adversarial Training
more to come...

## Tangent Distance, Tangent Prop, and Manifold Tangent Classifier
more to come...

## References
Hinton, G. E., Srivastava, N., Krizhevsky, A., Sutskever, I., and Salakhutdinov, R. 2012. Improving neural networks by
preventing co-adaptation of feature detectors. Technical report, arXiv:1207.0580. 238, 263, 267.

Lasserre, J. A., Bishop, C. M., and Minka, T. P. 2006. Principled hybrids of generative and discriminative models. In
CVPR'06.

Ian Goodfellow, Yoshua Bengio, and Aaron Courville. 2016. Deep Learning. Chapter 5.4.4. Chapter 7.

Christopher M. Bishop. 2006. Pattern Recognition and Machine Learning. Chapter 5.5.1.

{% include links.html %}
