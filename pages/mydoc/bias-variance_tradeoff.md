---
title: "Bias-Variance Tradeoff"
keywords: bias-variance, tradeoff
date: 2020-01-04 18:25:32 -0800
last_updated: December 26, 2020
tags: [machine_learning]
summary: "The bias-variance trade-off is a common issue generally present in machine learning systems. Today we
characterize this issue quantitatively with the equality for bias-variance-noise decomposition."
sidebar: none
permalink: bias-variance_tradeoff.html
folder: mydoc
---

The bias-variance trade-off is a common issue generally present in machine learning systems. Today we characterize this
issue quantitatively with the equality for bias-variance-noise decomposition.

## Loss Functions
When we discussed decision theory for regression problems, we considered various loss functions each of which leads to a
corresponding optimal prediction once we are given the conditional distribution $p(t\vert\mathbf{x})$. A popular choice
is the squared loss function, for which the optimal prediction is given by the conditional expectation, which we denote
by $h(\mathbf{x})$ and which is given by

$$
  {h(\mathbf{x})=\mathbb{E}[t\vert\mathbf{x}]=\int tp(t\vert\mathbf{x})dt.}
$$

Here we consider briefly one simple generalization of the squared loss, called the *Minkowski loss*, whose expectation
is given by

$$
  {\mathbb{E}[L_q]=\int\int\vert y(\mathbf{x})-t\vert^q p(\mathbf{x},t)d\mathbf{x}dt}
$$

which reduces to the expected squared loss for $q=2$. The minimum of $\mathbb{E}[L_q]$ is given by the conditional mean
for $q=2$, the conditional median for $q=1$, and the conditional mode for $q\rightarrow 0$.

## Function Estimation with Unlimited Training Data
Consider the noisy target described by the conditional distribution $p(t\vert\mathbf{x})$. For mean squared loss which
corresponds to the case $q=2$, the loss function is decomposed into

$$
  {\mathbb{E}[L_2]=\int\int(y(\mathbf{x})-t)^2p(\mathbf{x},t)d\mathbf{x}dt
  =\int\int(y(\mathbf{x})-h(\mathbf{x})+h(\mathbf{x})-t)^2p(\mathbf{x},t)d\mathbf{x}dt}\\
  {=\int(y(\mathbf{x})-h(\mathbf{x}))^2p(\mathbf{x})d\mathbf{x}+\int\int(h(\mathbf{x})-t)^2p(\mathbf{x},t)d\mathbf{x}dt.}
$$

The first term depends on our choice for the function $y(\mathbf{x})$, and we will seek a solution for it which makes
this term a minimum. The second term, which is independent of $y(\mathbf{x})$, arises from the intrinsic noise on the
data and represents the minimum achievable value of the expected loss. The third cross-term vanishes since

$$
  {\int\int2(y(\mathbf{x})-h(\mathbf{x}))(h(\mathbf{x})-t)p(\mathbf{x},t)d\mathbf{x}dt
  =\int\int2(y(\mathbf{x})-h(\mathbf{x}))(h(\mathbf{x})-t)p(t\vert\mathbf{x})dtp(\mathbf{x})d\mathbf{x}=0.}
$$

By basic calculus, we make it clear that this cross-term always vanishes, irrespective of whether the noise in target
$t$ is supposed to arise from **intrinsic** or **extrinsic** sources. That is to say, we need not assume independence of
the noise from either $\mathbf{x}$ or $t$.

If we had an unlimited supply of data (and unlimited computational resources), we could in principle find the regression
function $h(\mathbf{x})$ to any desired degree of accuracy, and this would represent the optimal choice for
$y(\mathbf{x})$.

## Function Estimation with Limited Training Data
Again, consider the noisy target described by the conditional distribution $p(t\vert\mathbf{x})$. However, in practice
we have a data set $\mathcal{D}$ containing only a finite number $N$ of data points, and consequently we do not know the
regression function $h(\mathbf{x})$ exactly. Suppose we had a large number of data sets each of size $N$ and each drawn
independently from the distribution $p(\mathbf{x}, t)$. For any given data set $\mathcal{D}$, we can run our learning
algorithm and obtain a prediction function $y(\mathbf{x};\mathcal{D})$. Different data sets from the ensemble will give
different functions and consequently different values of the squared loss. The performance of a particular learning
algorithm is then assessed by taking the average over this ensemble of data sets.

Consider the integrand of the first term, which for a particular data set $\mathcal{D}$ takes the form
$(y(\mathbf{x};\mathcal{D})-h(\mathbf{x}))^2$. Because this quantity will be dependent on the particular data set
$\mathcal{D}$, we take its average over the ensemble of data sets. If we add and subtract the quantity
$\mathbb{E}_{\mathcal{D}}[y(\mathbf{x};\mathcal{D})]$ inside the braces, and then expand, we obtain

$$
  {\mathbb{E}_{\mathcal{D}}[L_2]=\mathbb{E}_{\mathcal{D}}[(y(\mathbf{x};\mathcal{D})-h(\mathbf{x}))^2]
  =(\mathbb{E}_{\mathcal{D}}[y(\mathbf{x};\mathcal{D})]-h(\mathbf{x}))^2
  +\mathbb{E}_{\mathcal{D}}[(y(\mathbf{x};\mathcal{D})-\mathbb{E}_{\mathcal{D}}[y(\mathbf{x};\mathcal{D})])^2]
  +\mathbb{E}_{\mathcal{D}}[(h(\mathbf{x})-t)^2].}
$$

So far, we have considered a single input value $\mathbf{x}$. If we substitute this expansion back, we obtain the
following decomposition of the expected squared loss

$$
  {\text{expected loss}=(\text{bias})^2+\text{variance}+\text{noise}}
$$

where

$$
  {(\text{bias})^2=\int(\mathbb{E}_{\mathcal{D}}[y(\mathbf{x};\mathcal{D})]-h(\mathbf{x}))^2p(\mathbf{x})d\mathbf{x}}\\
  {\text{variance}=\int\mathbb{E}_{\mathcal{D}}[(y(\mathbf{x};\mathcal{D})-\mathbb{E}_{\mathcal{D}}[y(\mathbf{x};\mathcal{D})])^2]p(\mathbf{x})d\mathbf{x}}\\
  {\text{noise}=\int\int(h(\mathbf{x})-t)^2p(\mathbf{x},t)d\mathbf{x}dt.}
$$

The bias and variance terms now refer to integrated quantities, and the noise term does not depend on $\mathcal{D}$.

## Remarks
Assuming deterministic target $t$ given input $\mathbf{x}$, most technical treatments do not need to mention the noise
term and its related conditional probability $p(t\vert\mathbf{x})$. Take the $L_2$ loss for example, in which the
bias-variance relation takes the form

$$
  {\mathbb{E}_{\mathcal{D}}[L_2]=(\mathbb{E}_{\mathcal{D}}[y(\mathbf{x};\mathcal{D})]-h(\mathbf{x}))^2
  +\mathbb{E}_{\mathcal{D}}[(y(\mathbf{x};\mathcal{D})-\mathbb{E}_{\mathcal{D}}[y(\mathbf{x};\mathcal{D})])^2],}
$$

and the term $h(\mathbf{x})$ corresponds to the true target $t$ given $\mathbf{x}$. This fact is obvious by the
observation that as the conditional probability $p(t\vert\mathbf{x})$ approaches a Dirac Delta function sharply peaked
around $t$, the noise term vanishes as expected.

Although the bias-variance decomposition may provide some interesting insights into the model complexity issue from a
frequentist perspective, it is of limited practical value, because the bias-variance decomposition is based on averages
with respect to ensembles of data sets, whereas in practice we have only the single observed data set. If we had a large
number of independent training sets of a given size, we would be better off combining them into a single large training
set, which of course would reduce the level of over-fitting for a given model complexity.

## References
Christopher M. Bishop. 2006. Pattern Recognition and Machine Learning. Chapter 1.5.1, 3.2.

Ian Goodfellow, Yoshua Bengio, and Aaron Courville. 2016. Deep Learning. Chapter 5.4.

Sergios Theodoridis and Konstantinos Koutroumbas. 2009. Pattern Recognition. Chapter 3.5.3.

{% include links.html %}
