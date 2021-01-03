---
title: Training, Validation, And Test
permalink: training_validation_and_test.html
date: 2019-11-28
tags: [machine_learning,deep_learning]
published: false
---

In machine learning, the study and construction of algorithms are expected to learn from and make predictions on data
is a common task. The data used to build the final model usually comes from multiple datasets. In particular, three data
sets are concerned in different stages of the creation of the model.

## The Training Set, Validation Set, and Test Set
In the ideal case where data is plentiful, one approach is simply to use some of the available data, called a **training
set**, to train a range of models, or a given model with a range of values for its complexity parameters, and then to
compare them on independent data, sometimes called a **validation set**, and select the one having the best predictive
performance.

The validation set provides an unbiased evaluation of a model fit on the training set, corresponding to individual
values of the hyperparameters. However, if the model design is iterated many times using a limited size data set, then
some over-fitting to the validation data can occur and so it may be necessary to keep aside a third **test set** on
which the performance of the selected model is finally evaluated. The test set provides an unbiased evaluation of the
final model, that is, the model having the smallest error with respect to the validation set.

In many applications, however, the supply of data for training and testing will be limited, and in order to build good
models, we wish to use as much of the available data as possible for training. However, if the validation set is small,
it will give a relatively noisy estimate of predictive performance. Hence, we should introduce several splitting
techniques that help us split them wisely among the training set, validations set and test set, such as **hold-out
validation**, **S-fold cross validation**, **leave-one-out cross validation**, and **bootstraping validation**.

## The Final Model
After model selection, a final round of training or fine-tuning should be done on all available data $\mathcal{D}$. In
Tom M. Mitchell's [Machine Learning](#references), an example was given in which the number of iterations served as the
hyperparameter. On each experiment the cross-validation approach is used to determine the number of iterations $i$ that
yield the best performance on the validation set. The mean $\bar{i}$ of these estimates for $i$ is then calculated, and
a final run of training is performed on all data samples in $\mathcal{D}$ for $\bar{i}$ iterations, with no validation
set. However, in [Deep Learning](#references) Ian Goodfellow suggests an incremental learning strategy that continues
training the model with current parameters, but now using all of the data.

Also according to [Deep Learning](#references), the "training time" or "training epochs" hyperparameter is a special
one, since most hyperparameters must be chosen using an expensive guess and check process, whereas the "training time"
hyperparameter is unique in that by definition, a single run of training tries out many values of the hyperparameter.

## Remarks
The key difference between validation and test is, we use the validation set to compare models with different
hyperparameters and to select the one having the best predictive performance. However, we use the test set to give a
relatively realistic assessment of the selected model on its generalization capability over unseen data.

## References
Christopher M. Bishop. 2006. Pattern Recognition and Machine Learning. Chapter 7.

Ian Goodfellow, Yoshua Bengio, and Aaron Courville. 2016. Deep Learning. Chapter 5.3.

Tom M. Mitchell. 1997. Machine Learning. p112.

{% include links.html %}
