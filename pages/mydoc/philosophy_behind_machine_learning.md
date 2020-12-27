---
title: "Philosophy Behind Machine Learning [more to come]"
keywords: philosophy
date: 2019-11-28 11:34:25 -0800
last_updated: December 26, 2020
tags: [machine_learning]
summary: "Machine Learning, as a fast-developing research field, still has a relatively stable philosophical background
on which many concepts and algorithms are based. Let us now explain and discuss some of them."
sidebar: mydoc_sidebar
permalink: philosophy_behind_machine_learning.html
folder: mydoc
---

Machine Learning, as a fast-developing research field, still has a relatively stable philosophical background on which
many concepts and algorithms are based. Let us now explain and discuss some of them.

## Occam's Razor
Any effective machine learning algorithm must have an inductive bias, or else it is perplexed by equivalent models in
the hypothesis space. The Occam's Razor principle we've mentioned before does not only serve as a model selection
criterion. More to come...

## Principle of Multiple Explanations
As proposed by Epicurus, this principle suggests retaining all hypotheses that are consistent with empirical data. Its
philosophy coincides perfectly with a subfield of machine learning called ensemble learning. More to come...

## No-Free-Lunch Theorem (NFL)
The No-Free-Lunch Theorem for machine learning [Wolpert and Macready (1997)](#references) states that, averaged over all
possible data generating distributions, every classification algorithm has the same error rate when classifying
previously unobserved points. In other words, in some sense, no machine learning algorithm is universally any better
than any other. Fortunately, these results hold only when we average over all possible data generating distributions. If
we make assumptions about the kinds of probability distributions we encounter in real-world applications, then we can
design learning algorithms that perform well on these distributions. See [Ian Goodfellow et al. (2016)](#references).
More to come...

## References
David H. Wolpert and William G. Macready. 1997. No Free Lunch Theorems for Optimization.

Zhihua Zhou. 2016. Machine Learning. Chapter 1.7. (In Simplified Chinese)

Ian Goodfellow, Yoshua Bengio, and Aaron Courville. 2016. Deep Learning. Chapter 5.2.1.

{% include links.html %}
