---
title: "A View On Entropy With Applications"
keywords: entropy
date: 2020-08-08 10:10:15 -0800
last_updated: December 26, 2020
tags: [mathematics,machine_learning]
summary: "This post demonstrated an alternative view on the definition of entropy."
sidebar: none
permalink: a_view_on_entropy_with_applications.html
folder: mydoc
---

According to *Wikipedia*, a general view on entropy is formulated as follows:

> In information theory, the entropy of a random variable is the average level of "information", "surprise", or
> "uncertainty" inherent in the variable's possible outcomes.

## The Most Probable Ensemble
Suppose a variable $\mathcal{X}$ takes on a limited set of values, ranging from $x_1$ to $x_I$ where $I$ is the
cardinality of its support set. Then an interesting task is to estimate the probability that an ensemble $\mathcal{E}$
of observations occur. It turns out, given a sufficiently large number, say $n$, of independent and identically
distributed (i.i.d.) observations of $\mathcal{X}$, in which $n_i$ observations take on value $x_i, 1\leq i\leq I$, the
joint probability of the occurrence of these observations is (we consider unordered combinations, not ordered
permutations),

$$
  {P(\mathcal{E})=\frac{n!}{\Pi_{i=1}^I n_i!}\Pi_{i=1}^I p_i^{n_i}.}
$$

Assume $P(\mathcal{E})$ is maximized at $x_i=n_i$, and then we have $P(\mathcal{E}')\leq P(\mathcal{E})$ if
$\mathcal{E}'$ has $n_j+1$ occurrences of $x_j$ and $n_k-1$ occurrences of $x_k, \forall j\not=k$. This establishes an
inequality

$$
  {P(\mathcal{E})=\frac{n!}{\Pi_{i=1}^I n_i!}\Pi_{i=1}^I p_i^{n_i}\geq\frac{n!}{\Pi_{i=1}^I n_i!(n_j\!+\!1)/n_k}\Pi_{i=1}^I p_i^{n_i}p_j/p_k=P(\mathcal{E}'), \forall j\not=k.}
$$

That is (note that when $j=k$ the inequality is trivially true),

$$
  {\frac{p_j}{n_j\!+\!1}\leq\frac{p_k}{n_k}, \forall j, k.}
$$

That is,

$$
  {\frac{1}{n\!+\!I}\leq\max_i\{\frac{p_i}{n_i\!+\!1}\}\leq\min_i\{\frac{p_i}{n_i}\}\leq\frac{1}{n},\ \forall i} \\
  {\text{ or equivalently, }np_i\!-\!1\leq n_i\leq np_i\!+\!Ip_i,\ \forall i.}
$$

Although lacking mathematical rigidity, we are still confident to make a well-founded guess that for sufficiently large
$n$, $P(\mathcal{E})$ is maximized if and only if $n_i=np_i, 1\leq i\leq I$.

## Two Alternative Interpretations of Entropy
Assuming the same context as before, we are also interested in estimating the joint probability of observing an ordered
sequence $\mathcal{S}$ of $\mathcal{X}$. That is,

$$
  {P(\mathcal{S})=\Pi_{i=1}^I p_i^{n_i}=\exp(\sum_{i=1}^In_i\log p_i).}
$$

If this sequence $\mathcal{S}$ is a realization of the ensemble $\mathcal{E}$ that maximizes the joint probability, then
$n_i=np_i, 1\leq i\leq I$, and

$$
  {P(\mathcal{S})=\exp(n\sum_{i=1}^Ip_i\log p_i)=\exp(\!-\!nH(X)),\qquad\text{(Equation 1)}} \\
  {P(\mathcal{E})=\frac{n!}{\Pi_{i=1}^I n_i!}P(\mathcal{S})=\frac{n!}{\Pi_{i=1}^I (np_i)!}\exp(\!-\!nH(X)).\qquad\text{(Equation 2)}}
$$

Hence we have two different, alternative interpretations of entropy $H(\mathcal{X})$ in addition to its conventional
definition, i.e.:

* (The conventional definition of entropy) The entropy $H(\mathcal{X})$ is a metric for the average-case negative
log-likelihood of an observation of $\mathcal{X}$.

* (A first alternative interpretation) The entropy $H(\mathcal{X})$ is related by Equation 1 to the sequence
$\mathcal{S}$ that induces the maximum joint probability of ordered observations of $\mathcal{X}$. The sample space
ranges over all **ordered** sequences.

* (A second alternative interpretation) The entropy $H(\mathcal{X})$ is related by Equation 2 to the ensemble
$\mathcal{E}$ that induces the maximized joint probability of unordered observations of $\mathcal{X}$. The sample space
ranges over all **unordered** ensembles.

{% include links.html %}
