---
title: "MSE And CE Estimate Posterior Class Probabilities And Achieve Bayes Error"
keywords: posterior, bayes error
date: 2020-01-09 13:52:41 -0800
last_updated: December 26, 2020
tags: [machine_learning]
summary: "A nice surprise about Mean Square Estimate and Cross Entropy Loss is that a Bayesian flavor still remains
inside, although in a somewhat disguised form."
sidebar: mydoc_sidebar
permalink: mse_and_ce_estimate_posterior_class_probabilities_and_achieve_bayes_error.html
folder: mydoc
---

A nice surprise about Mean Square Estimate and Cross Entropy Loss is that a Bayesian flavor still remains inside,
although in a somewhat disguised form.

## The MSE Cost Function
We will consider the multiclass case. Given $\mathbf{x}$, we want to estimate its class label. Let $g_i(\mathbf{x})$ be
the discriminant functions to be designed. The mean square error cost function now becomes

$$
  {\mathbf{J}=\mathbb{E}[\sum_{i=1}^M(g_i(\mathbf{x})-y_i(\mathbf{x}))^2],}
$$

where $\mathbf{y}(\mathbf{x})$ is a one-hot vector consisting of $0$'s and a single $1$ at the appropriate place, and we
seek the best $\mathbf{g}(\mathbf{x})$ to approximate $\mathbf{y}(\mathbf{x})$ in the sense of minimum MSE. Taking into
account that $p(\mathbf{x},w_j)=p(w_j\vert\mathbf{x})p(\mathbf{x})$, it becomes

$$
  {\mathbf{J}=\mathbb{E}[\sum_{j=1}^M\sum_{i=1}^M(g_i(\mathbf{x})-y_i(\mathbf{x}))^2p(w_j\vert\mathbf{x})].}
$$

Expanding this, we get

$$
  {\mathbf{J}=\mathbb{E}[\sum_{j=1}^M\sum_{i=1}^M(g_i^2(\mathbf{x})p(w_j\vert\mathbf{x})-2g_i(\mathbf{x})y_i(\mathbf{x})p(w_j\vert\mathbf{x})+y_i^2(\mathbf{x})p(w_j\vert\mathbf{x}))]
  =\mathbb{E}[\sum_{i=1}^M(g_i^2(\mathbf{x})-2g_i(\mathbf{x})\mathbb{E}[y_i(\mathbf{x})\vert\mathbf{x}]+\mathbb{E}[y_i^2(\mathbf{x})\vert\mathbf{x}])].}
$$

Adding and subtracting $(\mathbb{E}[y_i(\mathbf{x})\vert\mathbf{x}])^2$, it becomes

$$
  {\mathbf{J}=\mathbb{E}[\sum_{i=1}^M(g_i(\mathbf{x})-\mathbb{E}[y_i(\mathbf{x})\vert\mathbf{x}])^2]
  +\mathbb{E}[\sum_{i=1}^M(\mathbb{E}[y_i^2(\mathbf{x})\vert\mathbf{x}]-(\mathbb{E}[y_i(\mathbf{x})\vert\mathbf{x}])^2)].}
$$

The second term does not depend on the functions $g_i(\mathbf{x})$. Thus, minimization of $\mathbf{J}$ with respect to
(the parameters of) $g_i$ affects only the first of the two terms. Minimizing $\mathbf{J}$ with respect to $g_i$ results
in the mean square estimates of the unknown parameters, so that the discriminant functions approximate optimally the
corresponding conditional means, that is, the regressions of $y_i$ conditioned on $\mathbf{x}$. Since $y_i$ only takes
on binary values, then $\mathbb{E}[y_i\vert\mathbf{x}]=p(w_i\vert\mathbf{x})$ and it follows that

$$
  {g_i(\mathbf{x})\text{ is the MSE estimate of }p(w_i\vert\mathbf{x}).}
$$

This is an important result. Training the discriminant functions $g_i$ with desired outputs $1$ or $0$ in the MSE sense
is equivalent to obtaining the MSE estimates of the class posterior probabilities, without using any statistical
information or pdf modeling! It suffices to say that these estimates may in turn be used for Bayesian classification. An
important issue here is to assess how good the resulting estimates are. It all depends on how well the adopted functions
$g_i$ can model the desired (in general) nonlinear functions $p(w_i\vert\mathbf{x})$. In general, 
[Richard and Lippmann (1991)](#references) pointed out that network outputs provide good Bayesian probability estimates
only if sufficient training data are available, if the network is complex enough, and if classes are sampled with the
correct prior class probabilities in the training data. Finally, it must be emphasized that the conclusion above is an
implication of the cost function itself and not of the specific model function used.

## The CE Cost Function
We still consider the same multiclass problem with cross entropy loss. The cost function now becomes

$$
  {\mathbf{J}=-\mathbb{E}[\sum_{i=1}^M(p(w_i\vert\mathbf{x})\log g_i(\mathbf{x})+(1-p(w_i\vert\mathbf{x}))\log(1-g_i(\mathbf{x}))],}
$$

which is maximized whenever the discriminant functions $g_i(\mathbf{x})$ optimally approximate the posterior class
probabilities $p(w_i\vert\mathbf{x})$. That is to say,

$$
  {g_i(\mathbf{x})\text{ is the MSE estimate of }p(w_i\vert\mathbf{x}).}
$$

We should reiterate that for optimal discriminant functions $g_i$ the normalization constraint is satisfied even without
explicitly required. That is, $\sum_{i=1}^Mg_i(\mathbf{x})=1,g_i(\mathbf{x})\geq0$. In spite of this, since the
functional form of $g_i$ are not always guaranteed to adequately model multiclass probability distributions (for
example, the output of a linear layer $\mathbf{g}=\mathbf{W}\mathbf{h}+\mathbf{b}$), we often explicitly impose the
normalization constraint with, for example, the softmax output function. As [Richard and Lippmann (1991)](#references)
mentioned, experimental results showed such normalization techniques proposed to ensure that the outputs of an MLP
network are true probabilities may be unnecessary.

Indeed, we can envision the cross entropy loss above as an element-wise version. However, we arrive at the same
conclusion if the ordinary multiclass cross entropy loss is applied

$$
  {\mathbf{J}=-\mathbb{E}[\sum_{i=1}^Mp(w_i\vert\mathbf{x})\log g_i(\mathbf{x})],}
$$

and again,

$$
  {g_i(\mathbf{x})\text{ is the MSE estimate of }p(w_i\vert\mathbf{x}).}
$$

## Other Cost Functions
Consider general loss functions $f(g_i(\mathbf{x})-y_i(\mathbf{x}))$. According to
[Hampshire and Pearlmutter (1990)](#references), a necessary and sufficient condition for minimization of the expected
loss requires that

$$
  {\frac{f'(x)}{f'(1-x)}=\frac{p(w_i\vert\mathbf{x})}{1-p(w_i\vert\mathbf{x})},}
$$

and that $f(\vert x\vert)$ is a strictly increasing function of $\vert x\vert$. Since we wish outputs of the
discriminant functions $g_i(\mathbf{x})$ to equal the posterior probabilities, we can assure this equivalence by
constraining the functional form of $f(x)$. One family of reasonable functions which can be derived by inspection is

$$
  {f(x)=\int x^r(1-x)^{r-1}dx,}
$$

for which $r=0$ corresponds to Cross Entropy Loss $f(x)=-\log(1-x)$, and $r=1$ corresponds to Mean Squared Error
$f(x)=\frac{1}{2}x^2$, respectively.

Because the major goal in machine learning is concerned with minimizing the classification error, methods like the
posterior probability estimation try to learn more than what is necessary for classification, which may limit its
performance for a fixed size network. In contrast, a number of techniques are devised to directly minimize the
classification error (in its reasonably smoothed versions to allow gradient-based methods) known as discriminative
learning. However, as was pointed out in [Richard and Lippman (1991)](#references), in a number of practical situations
the use of alternative cost functions did not necessarily lead to substantial performance improvements.

## Remarks
In addition to accurate estimation of Bayesian probabilities, interpretation of network outputs as Bayesian
probabilities makes it possible to compensate for differences in pattern class probabilities between test and training
data (rescaling by priors), to combine outputs of multiple classifiers for higher level decision making (combining
predicts from multiple networks using bagging), to use alternative risk functions different from minimum-error risk
(introducing bayes risk), to implement conventional optimal rules for pattern rejection (setting rejection thresholds),
and to compute alternative measures of network performance (checking the extent to which
$\sum_{i=1}^Mp(w_i\vert\mathbf{x})=1,\forall\mathbf{x}$ and $p(w_i)=\frac{1}{N}\sum_{j=1}^Np(w_i\vert\mathbf{x}_j)$
hold). Interested readers are recommended to refer to <a href="#references">Richard and Lippman (1991)</a> for more
details.

## References
Sergios Theodoridis and Konstantinos Koutroumbas. 2009. Pattern Recognition. Chapter 3.5.2. Chapter 4.8.

John B. Hampshire II and Barak A. Pearlmutter. 1990. Equivalence Proofs for Multi-Layer Perceptron Classifiers and the
Bayesian Discriminant Function. Proceedings of the 1990 Connectionist Models Summer School.

Michael D. Richard and Richard P. Lippmann. 1991. Neural Network Classifiers Estimate Bayesian a posteriori
Probabilities. Neural Computation.

{% include links.html %}
