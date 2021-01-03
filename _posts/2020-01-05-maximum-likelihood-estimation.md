---
title: "Maximum Likelihood Estimation [more to come]"
permalink: maximum_likelihood_estimation.html
date: 2020-01-05 14:42:42 +0700
tags: [machine_learning,deep_learning,mathematics,statistics]
published: false
---

The **maximum likelihood estimator (MLE)** has many desirable properties which make it popular in the machine learning
community. Let's try to discuss some of them today.

## Identifiable Probability Densities
For most of the typically encountered probability densities $p(\mathbf{x}\vert\theta)$, the sequence of posterior
densities does indeed converge to a delta function. Roughly speaking, this implies that with a large number of samples
there is only one value for $\theta$ that causes $p(\mathbf{x}\vert\theta)$ to fit the data, i.e., that $\theta$ can be
determined uniquely from $p(\mathbf{x}\vert\theta)$. When this is the case, $p(\mathbf{x}\vert\theta)$ is said to be
**identifiable**. There are occasions, however, when more than one value of $\theta$ may yield the same value for
$p(\mathbf{x}\vert\theta)$. In such cases, $\theta$ cannot be determined uniquely from $p(\mathbf{x}\vert\theta)$, and
$p(\mathbf{x}\vert\mathcal{D})$ will peak near all of the values of $\theta$ that explain the data. Fortunately, this
ambiguity is often erased when $p(\mathbf{x}\vert\theta)$ is the same for all of these values of $\theta$. Thus,
$p(\mathbf{x}\vert\mathcal{D})$ will typically converge to $p(\mathbf{x})$ whether or not $p(\mathbf{x}\vert\theta)$ is
identifiable. While this might make the problem of identifiabilty appear to be moot, we shall see that identifiability
presents a genuine problem in the case of unsupervised learning. See [Duda et al. (2001)](#references) for a discussion.

Apparently, estimating $\theta$ with the Maximum Likelihood Estimation requires the probability density to be
identifiable.

## Asymptotically Gaussian
The probability density function of the Maximum Likelihood Estimate as $m\to\infty$ approaches the Gaussian distribution
with mean equal to its true value $\mathbf{\theta}$. This property is an offspring of the Central Limit Theorem and the
fact that the Maximum Likelihood Estimate is related to the sum of random variables, that is

$$
  {\partial\ln p(\mathbf{x}_k;\mathbf{\theta})/\partial\mathbf{\theta}.}
$$

## Asymptotically Unbiased
The Maximum Likelihood Estimate is asymptotically unbiased, which by definition means that

$$
  {\lim_{m\to\infty}\mathbb{E}[\hat{\mathbf{\theta}}^{(m)}]=\mathbf{\theta}.}
$$

Alternatively, we say that the estimate converges in the mean to the true value.

## Asymptotically Consistent
The Maximum Likelihood Estimate is asymptotically consistent, that is, it satisfies

$$
  {\lim_{m\to\infty}P(\Vert\hat{\mathbf{\theta}}^{(m)}-\mathbf{\theta}\Vert\leq\epsilon)=1,}
$$

where $\epsilon$ is arbitrarily small. Alternatively, we say that the estimate converges in probability. A stronger
condition for consistency is also true:

$$
  {\lim_{m\to\infty}\mathbb{E}[(\Vert\hat{\mathbf{\theta}}^{(m)}-\mathbf{\theta}\Vert^2]=0.}
$$

In such cases we say that the estimate converges in the mean square.

Consistency is very important for an estimator, because it may be unbiased but the resulting estimates exhibit large
variations around the mean. In such cases we have little confidence in the result from a single set $\mathcal{D}$.
Besides, consistency implies unbiasedness. However, the reverse is not true -- unbiasedness does not imply consistency.

P.S.: The consistency definition above is sometimes referred to as weak consistency, with strong consistency referring
to the almost sure convergence of $\hat{\mathbf{\theta}}^{(m)}$ to $\mathbf{\theta}$. Almost sure convergence of a
sequence of random variables $\hat{\mathbf{\theta}}^{(1)},\hat{\mathbf{\theta}}^{(2)},\ldots$ to a value
$\mathbf{\theta}$ occurs when

$$
  {P(\lim_{m\to\infty}\hat{\mathbf{\theta}}^{(m)}=\mathbf{\theta})=1.}
$$

## Asymptotically Efficient
There are other inductive principles besides the maximum likelihood estimator, many of which share the property of being
consistent estimators. However, consistent estimators can differ in their statistic efficiency, in terms of the expected
mean squared error. The Maximum Likelihood Estimate is asymptotically consistent; that is, it achieves the Cramér-Rao
lower bound, which is the lowest value of variance that any estimate can achieve. This fact also shows that no
consistent estimator has a lower mean squared error than the maximum likelihood estimator.

## MLE with Regularization
For these reasons, maximum likelihood is often considered the preferred estimator to use for machine learning. When the
number of examples is small enough to yield overfitting behavior, regularization strategies such as weight decay may be
used to obtain a biased version of maximum likelihood that has less variance when training data is limited.

## MLE with Missing or Noisy Data
Suppose we encounter cases in which the available data set is incomplete, <I>i.e.</I>, with missing attributes $Z$ or
besides observed attributes $X$. We seek the Bayes rule given the observed features, and for that the posterior
probabilities are needed.

$$
  {p(w_i\vert\mathbf{x})=\frac{p(w_i,\mathbf{x})}{p(\mathbf{x})}=\frac{\int p(w_i,\mathbf{x},\mathbf{z})d\mathbf{z}}{p(\mathbf{x})}
  =\frac{\int p(w_i\vert\mathbf{x},\mathbf{z})p(\mathbf{x},\mathbf{z})d\mathbf{z}}{\int p(\mathbf{x},\mathbf{z})d\mathbf{z}}.}
$$

It is a simple matter to generalize the results to the case where a particular feature has been corrupted by
statistically independent noise. We assume we have uncorrupted features $X$, as before, and a noise model, expressed as
$p(\mathbf{z}\vert\mathbf{z}_t)$. Here we let $\mathbf{z}_t$ denote the true value of the observed but corrupted
features $\mathbf{z}$, i.e., without the noise present; that is, the $\mathbf{z}$ are observed instead of the true
$\mathbf{z}_t$. We assume that if $\mathbf{z}_t$ were known, $\mathbf{z}$ would be independent of $w_i$ and
$\mathbf{x}$. From such an assumption we get:

$$
  {p(w_i\vert\mathbf{x},\mathbf{z})=\frac{\int p(w_i\vert\mathbf{x},\mathbf{z},\mathbf{z}_t)d\mathbf{z}_t}{p(\mathbf{x},\mathbf{z})}
  =\frac{\int p(w_i\vert\mathbf{x},\mathbf{z}_t)p(\mathbf{x},\mathbf{z}_t)p(\mathbf{z}\vert\mathbf{z}_t)d\mathbf{z}_t}
  {\int p(\mathbf{x},\mathbf{z}_t)p(\mathbf{z}\vert\mathbf{z}_t)d\mathbf{z}_t}.}
$$

In the extreme case where $p(\mathbf{z}\vert\mathbf{z}_t)$ is uniform over the entire space (and hence provides no
predictive information for categorization), the equation reduces to the case of missing features -- a satisfying result.

However, the formula shows that we must integrate (marginalize) the posterior probability over the missing or corrupted
features, and this integral often proves difficult to compute in closed form. We shall bypass the integral and consider
alternative estimation techniques, such as the **expectation maximization (EM)** algorithm.

## References
Duda, Richard O. and Hart, Peter E. and Stork, David G. 2001. Pattern Classification. Chapter 2.10, 3.5.

Ian Goodfellow, Yoshua Bengio, and Aaron Courville. 2016. Deep Learning. Chapter 5.4.5, 5.5.2.

Sergios Theodoridis and Konstantinos Koutroumbas. 2009. Pattern Recognition. Chapter 2.5.1.

{% include links.html %}
