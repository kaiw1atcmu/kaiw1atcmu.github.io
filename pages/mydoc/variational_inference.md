---
title: "Variational Inference"
keywords: site introduction
date: 2020-07-25 07:34:26 -0800
last_updated: December 26, 2020
tags: [statistics,deep_learning]
summary: "Variational Inference is a general Bayesian statistical procedure to learn unknown distribution with certain
priors given. It has recently been gaining popularity in the statistics and deep learning communities."
sidebar: none
permalink: variational_inference.html
folder: mydoc
---

In the same context as the expectation maximization algorithm, we usually have a set of observed variables $\mathcal{X}$
and a set of latent (i.e., unobserved) variables $\mathcal{Z}$. The challenge of inference usually refers to the
difficult problem of computing $p(\mathbf{z}\vert\mathbf{x})$ or taking expectations with respect to it. Unfortunately, we
are more frequently confronted with intractable posterior distributions $p(\mathbf{z}\vert\mathbf{x})$, especially in
graphical modeling.

## The Definition of Variational Inference (VI)
Like in the **expectation maximization (EM)**, we are concerned in computing the log probability of the observed data,
or the **evidence function**, $\log p(\mathbf{x};\mathbf{\theta})$, or its lower bound when this value is intractable or
costly to compute. This numeric value is called the **evidence lower bound (ELBO)**, or the negative **variational free
energy**,

$$
  {L(\mathbf{z},\mathbf{\theta},q)=\log p(\mathbf{x};\mathbf{\theta})-D_{KL}(q(\mathbf{z}\vert\mathbf{x})\Vert p(\mathbf{z}\vert\mathbf{x};\mathbf{\theta})).}
$$

Since the **Kullback-Leibler (KL) divergence** is always non-negative, it follows
$L\leq\log p(\mathbf{x};\mathbf{\theta})$ always provides a lower bound on $\log p(\mathbf{x};\mathbf{\theta})$.
Interestingly, $L$ is easier to optimize for some pre-selected distribution families of $q$. Straight-forward algebra
gives us another variant expression of $L$,

$$
  {L(\mathbf{z},\mathbf{\theta},q)=\mathbb{E}_{\mathbf{z}\sim q}[\log p(\mathbf{z}\vert\mathbf{x};\mathbf{\theta})]+H(q).}
$$

As the optimization proceeds, the lower bound $L$ approaches closer to $\log p(\mathbf{x})$, until the lower bound is
tight if and only if $q(\mathbf{z}\vert\mathbf{x};\mathbf{\theta})=p(\mathbf{z}\vert\mathbf{x};\mathbf{\theta})$. At the
same time, we arrived at a parametrized functional form of $q$ (by way of calculus of variation), an approximation of
the intractable or costly true posterior $p$. The entire set of techniques is called **variational inference (VI)** in
that it enables us to solve for the explicit functional form of $q$ with calculus of variation.

## Variational Inference as Expectation Maximization (EM)
Recall the $F$ function discussed in our previous post, which has essentially the same functional form as the evidence
lower bound $L$ given in the second form. In the E-step, set
$q(\mathbf{z}\vert\mathbf{x})=p(\mathbf{z}\vert\mathbf{x};\mathbf{\theta})$, and in the M-step, update $\mathbf{\theta}$
by standard optimization algorithms at our choice. Since the EM algorithm successively increases the evidence lower
bound on $\log p(\mathbf{x};\mathbf{\theta})$, the same optimization objective as the VI algorithm, we end up with
basically the same procedure that optimizes parameter $\mathbf{\theta}$ required by the VI algorithm.

Actually, variational inference is performed using gradient descent or its variants instead of expectation maximization,
since no closed-form solution to $q(\mathbf{z})$ is found; otherwise, both VI and EM are in effect **identical**. We
roughly summarize them as

| ... | hidden vars incl. | no hidden vars incl. |
| :----: | :----: | :----: |
| closed-form | EM and VI | directly solving for roots |
| no closed-form | VI | gradient descent |

<center><font face="Lora">Table 1: Conditions Under Which EM And VI Are Feasible</font></center>

## Variational Inference as Maximum a Posteriori (MAP)
An alternative form of inference is to compute the single most likely value of the missing variables, rather than to
infer the entire distribution over their possible values. This means restricting $q$ to the family of dirac functions,
and computing

$$
  {\mathbf{z}^*={\arg\max}_{\mathbf{z}}p(\mathbf{z}\vert\mathbf{x}).}
$$

For MAP inference, the posterior $q$ degenerates and shrinks into a single point Dirac Delta function
$q(\mathbf{z}\vert\mathbf{x};\mathbf{\theta})=\delta(\mathbf{z}-h(\mathbf{x};\mathbf{\theta}))$, which is shown to be
equivalent to the aforementioned problem. We can thus justify a learning procedure similar to EM, in which we alternate
between performing MAP inference to infer $\mathbf{z}^*$ and then update $\mathbf{\theta}$ to increase the evidence
function $\log p(\mathbf{x};\mathbf{\theta})$.

## Variational Inference and Learning
The evidence lower bound $L$ always provides a lower bound on $\log p(\mathbf{z}\vert\mathbf{x})$, irrespective of how
we choose the distribution families of $q$. The core idea behind variational learning is that we can maximize $L$ over a
restricted family of distributions $q$. This family should be chosen so that it is easy to compute
$\mathbb{E}_{\mathbf{z}\sim q}[\log p(\mathbf{z}\vert\mathbf{x};\mathbf{\theta})]$.

A common approach to variational learning called the **mean field** approach is to impose the restriction that $q$
factorizes into products of individual components given the observed data $\mathbf{x}$,

$$
  {p(\mathbf{z}\vert\mathbf{x})=\Pi_i p(z_i\vert\mathbf{x}).}
$$

Under the mean field hypothesis, there is a general equation for the fixed point updates. Fix $q(z_j\vert\mathbf{x})$
for all $i\ne j$, then the optimal $q(z_i\vert\mathbf{x})$ may be obtained by normalizing the unnormalized distribution

$$
  {\tilde{q}(z_i\vert\mathbf{x})=\exp(\mathbb{E}_{\mathbf{z}_{-i}\sim q(\mathbf{z}_{-i}\vert\mathbf{x})}[\log\tilde{p}(\mathbf{x},\mathbf{z})]).}
$$

More generally, we can impose any graphical model structure we choose on $q$, to flexibly determine how many
interactions we want our approximation to capture. This fully general graphical model approach is called structured
variational inference. See [Saul and Jordan (1996)](#references) for more details.

## Interactions between Learning and Inference
Using approximate inference as part of a learning algorithm affects the learning process, and this in turn affects the
accuracy of the inference algorithm. The learned parameter $\mathbf{\theta}$ largely depends on the family of
distributions from which we draw $q$, since the training process increases $p(\mathbf{z}\vert\mathbf{x})$ for values of
$\mathbf{z}$ where $q(\mathbf{z}\vert\mathbf{x})$ has high probabilities, and decreases $p(\mathbf{z}\vert\mathbf{x})$
for values of $\mathbf{z}$ where $q(\mathbf{z}\vert\mathbf{x})$ has low probabilities. This behavior causes our
approximating assumptions to become "self-fulfilling prophecies".

Computing the true amount of harm imposed on a model by a variational approximation is thus very difficult. It is shown
the gap of $p(\mathbf{x};\mathbf{\theta})$ with $L$ will usually be small for certain scenarios, however, we should not
conclude that the variational approximation is assumed to be accurate in general. If the optimal
$\mathbf{\theta}^{\ast}$ induces too complicated of a posterior distribution for our $q$ family to capture, then the
learning process will never approach $\mathbf{\theta}^{\ast}$. Such a problem is very difficult to detect.

## References
Ian Goodfellow, Yoshua Bengio, and Aaron Courville. 2016. Deep Learning. Chapter 19.

Saul, L. K. and Jordan, M. I. 1996. Exploiting tractable substructures in intractable networks. In Advances in Neural
Information Processing Systems 8 (NIPS'95).

{% include links.html %}
