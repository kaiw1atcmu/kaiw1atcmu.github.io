---
layout: post
title: Bayesian Learning [more to come]
date: 2020-01-08
categories: [machine learning]
tags: [2020, English]
excerpt_separator: <!--more-->
published: false
---

<p>
    Both MLE and MAP methods compute a specific estimate of the unknown parameter vector $\mathbf{\theta}$.<!--more-->
    In the Bayesian Learning (Bayesian Inference) method, a different path is adopted. Given the set $\mathcal{D}$ of
    the $n$ training vectors and the a priori information about the pdf $p(\mathbf{\theta})$, whose prior distribution
    should be rather broad to provide equal chance to a rather large range of values.
</p>

<h2> Unsupervised Case </h2>
<p>
    One goal is to compute the posterior distribution of parameter $\mathbf{\theta}$. Using Bayes's theorem, we obtain
    $$
    {p(\mathbf{\theta}|\mathcal{D})=\frac{p(\mathcal{D}|\mathbf{\theta})p(\mathbf{\theta})}
    {\int p(\mathcal{D}|\mathbf{\theta})p(\mathbf{\theta})d\mathbf{\theta}}}
    $$
    and presupposing statistical independence among the training samples
    $$
    {p(\mathcal{D}|\mathbf{\theta})=\prod_{k=1}^np(\mathbf{x}_k|\mathbf{\theta}).}
    $$
    &emsp;&emsp;Another goal is to compute the conditional pdf $p(\mathbf{x}|\mathcal{D})$. To this end, and making use of known
    identities from our statistics basics, we have the following set of relations at our disposal:
    $$
    {p(\mathbf{x}|\mathcal{D})=\int p(\mathbf{x}|\mathbf{\theta})p(\mathbf{\theta}|\mathcal{D})d\mathbf{\theta}.}
    $$
    The conditional density $p(\mathbf{\theta}|\mathcal{D})$ is also known as the posterior pdf estimate, since it is
    updated "knowledge" about the statistical properties of $\mathbf{\theta}$, after having observed the data set
    $\mathcal{D}$. If $p(\mathbf{\theta}|\mathcal{D})$ is sharply peaked at $\hat{\mathbf{\theta}}$ and we treat it as a
    delta function, the equation becomes $p(\mathbf{x}|\mathcal{D})\approx p(\mathbf{x}|\hat{\mathbf{\theta}})$. This
    happens, for example, if $p(\mathcal{D}|\mathbf{\theta})$ is concentrated around a sharp peak and
    $p(\mathbf{\theta})$ is broad enough around this peak.
</p>

<h2> Supervised Case </h2>
<p>
    Let $\mathbf{y}=\{y_i,i=1,\ldots,n\}$ be the set of targets for the training set $\mathcal{D}$. Assume a model for
    the likelihood function $p(\mathbf{y}|\mathcal{D};\mathbf{\theta})$ which is, for example, Gaussian. This basically
    models the error distribution between the targets and outputs, and it is the stage at which the input training data
    come into the scene. One goal is to compute the posterior distribution of $\mathbf{\theta}$ given $\mathcal{D}$, as
    before. Using Bayes’s theorem, we obtain the posterior distribution of parameter $\mathbf{\theta}$
    $$
    {p(\mathbf{\theta}|\mathcal{D})=\frac{p(\mathcal{D}|\mathbf{\theta})p(\mathbf{\theta})}{p(\mathcal{D})}}
    $$
    where
    $$
    {p(\mathcal{D})=\int p(\mathcal{D}|\mathbf{\theta})p(\mathbf{\theta})d\mathbf{\theta}.}
    $$
    The resulting posterior pdf will be more sharply shaped around a value $\hat{\mathbf{\theta}}$, since it has learned
    from the available training data.<br>
    <br>
    &emsp;&emsp;Another goal is to predict the posterior distribution of its target $y_k$, given a feature vector
    $\mathbf{x}$.
    Interpreting the output $y_k$ of a network as the respective class probabilities, conditioned on the input
    $\mathbf{x}$ and the parameter vector $\mathbf{\theta}$, the conditional class probability is computed by averaging
    over all $\mathbf{\theta}$
    $$
    {p(y_k|\mathbf{x};\mathcal{D})=\int p(y_k|\mathbf{x};\mathbf{\theta})p(\mathbf{\theta}|\mathcal{D})d\mathbf{\theta}}
    $$
    which is in essence analogous to the aforementioned Bayes's posterior distribution theorem
    $$
    {p(\mathbf{x}|\mathcal{D})=\int p(\mathbf{x}|\mathbf{\theta})p(\mathbf{\theta}|\mathcal{D})d\mathbf{\theta}.}
    $$
</p>

<h2> Approximation Methods </h2>
<p>
    In general, the computation of $p(\mathbf{x}|\mathcal{D})$ requires the integration in the right-hand side. However,
    analytical solutions are feasible only for very special forms of the involved functions. For most of the cases one
    has to resort to numerical approximations.<br>
    <br>
    &emsp;&emsp;The goal to compute the conditional pdf $p(\mathbf{x}|\mathcal{D})$ now becomes that of generating a set of
    samples $p(\mathbf{\theta}_i|\mathcal{D})$. This difficulty is bypassed by a set of methods known as Markov chain
    Monte Carlo (MCMC) techniques. The main rationale behind these techniques is that one can generate samples from in a
    sequential manner that asymptotically follow the distribution $p(\mathbf{\theta}_i|\mathcal{D})$, even without
    knowing the normalizing factor. The Gibbs sampler and the Metropolis-Hastings algorithms are two of the most popular
    schemes of this type.<br>
    <br>
    &emsp;&emsp;more to come...
</p>

<h2> Related Posts </h2>
<font face="georgia"><a href="https://kaiw1atcmu.github.io/blog/2020/01/07/incremental-learning">
    Incremental Learning
</a></font>

<h2> References </h2>
<font face="georgia"><a name="pr">Sergios Theodoridis and Konstantinos Koutroumbas. 2009. Pattern Recognition. Chapter 2.5.3, 4.8.</a></font><br>
<br>
