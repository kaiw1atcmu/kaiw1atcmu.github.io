---
title: "The Hessian Matrix [more to come]"
keywords: hessian matrix
date: 2019-11-11 10:41:20 -0800
last_updated: December 26, 2020
tags: [mathematics,machine_learning]
summary: "Today we are introducing various techniques in evaluating the Hessian of neural networks, either approximately
or accurately."
sidebar: mydoc_sidebar
permalink: the_hessian_matrix.html
folder: mydoc
---

Today we are introducing various techniques in evaluating the Hessian of neural networks, either approximately or
accurately. Much content of this post is accredited to [Christopher Bishop (2006)](#references).

## The Hessian Matrix Is Instrumental
Sometimes we need to compute higher-order derivatives of feedforward networks, in particular, the Hessian matrix. The
Hessian plays an important role in many aspects of neural computing, as mentioned by Christopher Bishop, including the
following:

* several nonlinear optimization algorithms used for training neural networks are based on considerations of the
second-order properties of the error surface, which are controlled by the Hessian matrix;

* the Hessian forms the basis of a fast procedure for re-training a feed-forward network following a small change in the
training data;

* the inverse of the Hessian has been used to identify the least significant weights in a network as part of network
pruning algorithms;

* the Hessian plays a central role in the Laplace approximation for a Bayesian neural network. Its inverse is used to
determine the predictive distribution for a trained network, its eigenvalues determine the values of hyperparameters,
and its determinant is used to evaluate the model evidence.

## Computational Complexity
The Hessian can also be evaluated exactly, for a network of arbitrary feed-forward topology, using extension of the
technique of back propagation used to evaluate first derivatives, which shares many of its desirable features including
computational efficiency. The number of computational steps needed to evaluate the Hessian scales like $O(W^2)$. As we
shall see, there are efficient methods for evaluating the Hessian whose scaling is indeed $O(W^2)$.

## Diagonal Approximation
Some of the applications for the Hessian matrix discussed above require the inverse of the Hessian, rather than the
Hessian itself. For this reason, there has been some interest in using a diagonal approximation to the Hessian, by
simply assuming the off-diagonal entries to be zero, e.g., the Optimal Brain Damage technique proposed by
[Yann LeCun et al. (1989)](#references). Under this assumption, diagonal entries of the Hessian matrix are determined by
formulas similar to that of back propagation, by neglecting off-diagonal entries in the Hessian matrix. Note that the
number of computational steps required to evaluate this approximation is $O(W)$.

## Outer Product Approximation
The major problem with diagonal approximations, however, is that in practice the Hessian matrix is typically found to be
strongly nondiagonal, and so these approximations, which are driven mainly be computational convenience, must be treated
with care. As the Optimal Brain Surgeon technique proposed by [Babak Hassibi et al. (1993)](#references) observed, for
mean square loss functions, the Hessian matrix's entries (both diagonal and off-diagonal) can be approximated by the
products of its first-order derivatives:

$$
  {\frac{\partial^2 L}{\partial w_{kj}^r\partial w_{k'j'}^{r'}}=\sum_{i}\frac{\partial^2 \frac{1}{2}(\hat{y}_i-y_i)^2}{\partial w_{kj}^r\partial w_{k'j'}^{r'}}
  =\sum_{i}\frac{\partial \hat{y}_i}{\partial w_{kj}^r}\frac{\partial \hat{y}_i}{\partial w_{k'j'}^{r'}}
  +\sum_{i}\frac{\partial^2 \hat{y}_i}{\partial w_{kj}^r\partial w_{k'j'}^{r'}}\underbrace{(\hat{y}_i-y_i)}_{\text{vanishes near a minimum}}
  \approx\sum_{i}\frac{\partial \hat{y}_i}{\partial w_{kj}^r}\frac{\partial \hat{y}_i}{\partial w_{k'j'}^{r'}}.}
$$

By neglecting the second term, we arrive at the **Levenberg–Marquardt approximation** or the **outer product
approximation**, given by

$$
  {\mathbf{H}\simeq\sum_{i}\frac{\partial\hat{y}_i}{\partial vec(\mathbf{W})}\otimes\frac{\partial\hat{y}_i}{\partial vec(\mathbf{W})}.}
$$

For cross-entropy loss with logistic sigmoid output-unit activations, the corresponding approximation is given by

$$
  {\mathbf{H}\simeq\sum_{i}\hat{y}_i(1-\hat{y}_i)\frac{\partial\hat{y}_i}{\partial vec(\mathbf{W})}\otimes\frac{\partial\hat{y}_i}{\partial vec(\mathbf{W})}.}
$$

The elements of the matrix can then be found in $O(W^2)$ steps by simple multiplication.

## Fast Exact Multiplication by the Hessian
The fast and exact multiplication of vector $\mathbf{v}$ by the Hessian $\mathbf{H}$ is proposed in
[Barak A. Pearlmutter (1993)](#references), which Ian Goodfellow referred to as **the Krylov methods**. Actually, the
Krylov methods are defined to be a much broader set of iterative techniques for performing various operations like
approximately inverting a matrix or finding approximations to its eigenvectors or eigenvalues, without using any
operation other than matrix-vector products.

For our purpose of using the Krylov methods on the Hessian matrix, we only need to be able to compute $\mathbf{Hv}$, the
product between the Hessian matrix $\mathbf{H}$ and an arbitrary vector $\mathbf{v}$. We can in turn compute
$\mathbf{H}$ by picking a set of vectors $\mathbf{v}$ that span a full-rank vector basis, e.g. $\mathbf{e}_i$, and
$\mathbf{H}\mathbf{e}_i$ corresponds to the $i$-th column in the Hessian matrix $\mathbf{H}$.

More to come...

## References
Bishop, Christopher M. 2006. Pattern recognition and machine learning. Chapter 5.4.

Yann Le Cun, John S. Denker and Sara A. Solla. 1989. Optimal Brain Damage. Advances in NIPS.

Babak Hassibi, David G. Stork and Gregory J. Wolff. 1993. Optimal Brain Surgeon and General Network Pruning. IEEE
International Conference on Neural Networks.

Ian Goodfellow, Yoshua Bengio, and Aaron Courville. 2016. Deep Learning. Chapter 6.5.10.

Barak A. Pearlmutter. 1993. Fast Exact Multiplication by the Hessian. Neural Computation.

{% include links.html %}
