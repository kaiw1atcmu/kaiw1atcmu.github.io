---
title: "Network Pruning The Old-School Methods [more to come]"
keywords: network pruning, old-school
date: 2019-11-12 09:08:08 -0800
last_updated: December 26, 2020
tags: [deep_learning,machine_learning]
summary: "As deep neural networks grow exponentially in size, the topic of network pruning comes into play with
ever-increasing importance. This post described a couple of typical old-school (pre-deep-learning) network pruning
methods, in reverent reminiscence of the pioneers of network pruning techniques."
sidebar: none
permalink: network_pruning_the_old-school_methods.html
folder: mydoc
---

As deep neural networks grow exponentially in size, the topic of network pruning comes into play with ever-increasing
importance. This post described a couple of typical old-school (pre-deep-learning) network pruning methods, in reverent
reminiscence of the pioneers of network pruning techniques.

## Optimal Brain Damage
In [Yann Le Cun et al. (1989)](#references), the authors devised a class of practical and nearly optimal schemes called
<font face="Lora">Optimal Brain Damage</font> for adapting the size for adapting the size of a neural network, by
removing unimportant weights of it. The paper firstly introduced a per-parameter measure called
<font face="Lora">saliency</font>, i.e. the change in the objective function caused by deleting that parameter, using
analytical tools for the Hessian:

$$
  {\delta L=\sum_i \frac{\partial L}{\partial u_i}+\frac{1}{2}\sum_i\frac{\partial^2 L}{\partial u_i^2}(\delta u_i)^2+
\frac{1}{2}\sum_{i\not=j}\frac{\partial^2 L}{\partial u_i\partial u_j}\delta u_i\delta u_j+O(\vert\delta u\vert^3).}
$$

Since pruning is preformed after training has converged to a (probably local) minimum, the first-order derivatives
all vanish. Besides, under certain conditions, it is reasonable to make the diagonal approximation on the Hessian.
Therefore, neglecting the higher-order term, we are left with only the diagonal second-order infinitesimal terms.
The formula reduces to:

$$
  {\delta L=\frac{1}{2}\sum_i\frac{\partial^2 L}{\partial u_i^2}(\delta u_i)^2,}\\
  {\text{$\delta u=-u$, since it changed from $u$ to $0$.}}
$$

Before this paper, previous methods mostly focused on deleting parameters with smallest magnitude (absolute value).
However, the merit of this paper was to define "saliency" associated with parameter $u_i$ as:

$$
  {\frac{1}{2}\partial^2 L/\partial u_i^2(\delta u_i)^2=\frac{1}{2}\partial^2 L/\partial u_i^2(u_i)^2,}
$$

the increase in the objective function caused by simply deleting that parameter. Using an empirical complexity measure
of the number of nonzero free parameters, it makes sense to delete parameters with smallest "saliency". Deleting a
parameter means setting it to $0$ and freezing it. Several variant can be defined, e.g. decreasing the weights
instead of simply setting them to $0$, or allowing deleted weights to adapt again. The downside is after pruning, we
need to retrain the network until convergence. This pruning-retraining procedure can continue multiple times, until some
reasonable complexity-performance trade-off is achieved.

## Optimal Brain Surgeon
Published after its predecessor, the <font face="Lora">Optimal Brain Surgeon</font> algorithm in
[Babak Hassibi et al. (1993)](#references) took its title, seemingly to intimate that Optimal Brain Damage did not
really achieve optimal performance, and we need a brain surgeon to have the damage rectified. In spite of the joking
title, it indeed resulted in better performance and most importantly, a retraining-free scheme to update all the
weights.

The paper advocated that Optimal Brain Damage made an assumption about the Hessian begin diagonal. In fact, however, the
Hessians are strongly non-diagonal, i.e. with nonzero off-diagonal entries. Consequently, the Optimal Brain Damage
often deleted the wrong weights. Moreover, unlike other methods, the Optimal Brain Surgeon did not demand retraining
after pruning. That was indeed a huge improvement. Still, consider the change in the objective function in the form of
functional Taylor series:

$$
  {\delta L=(\frac{\partial L}{\partial\mathbf{w}})^t\delta\mathbf{w}+\frac{1}{2}\delta\mathbf{w}^t
  \mathbf{H}\delta\mathbf{w}+O(\vert\mathbf{w}\vert^3).}
$$

Omitting higher-order infinitesimal terms, at local minima the equation simplified to:

$$
  {\delta L=\frac{1}{2}\delta\mathbf{w}^t\mathbf{H}\delta\mathbf{w}.}
$$

The Optimal Brain Surgeon method's goal was to set one of the weights zero (while allowing other weights to change), to
minimize the increase in objective function. The optimization problem was formulated as follows in a Lagrangian:

$$
  {L=\frac{1}{2}\delta\mathbf{w}^t\mathbf{H}\delta\mathbf{w}+\lambda(\mathbf{e}_q^t\delta\mathbf{w}+w_q).}
$$

Following the standard Lagrangian procedure, the solution turned out to be:

$$
  {\delta\mathbf{w}=-\frac{w_q}{\mathbf{H}_{qq}^{-1}}\mathbf{H}_{-1}\mathbf{e}_q,}\\
  {L_q=\frac{1}{2}\frac{w_q^2}{\mathbf{H}_{qq}^{-1}}.}
$$

Then eliminate the $q$th weight $\mathbf{w}_q$, since it resulted in the least increase in the objective function $L$.
In the same spirit as in Optimal Brain Damage, the authors called $L_q$ the "saliency" of weight $q$. The algorithm
manifested its superiority by an inequality in terms of the objective function:
$L(\text{mag})\geq L(\text{OBD})\geq L(\text{OBS})$.

It the sequel, the authors addressed two essential aspects of the method. First, they made a contribution by developing
a general recursive algorithm of the inverse Hessian for a fully trained neural network. Starting with a scaled identity
matrix, the algorithm guaranteed there were no singularities in the calculation of $\mathbf{H}^{-1}$. Second, the
authors showed the justification that the approximation was valid both on computational and functional grounds, even
late in pruning when the training error was not negligible. Despite its success, there was still one issue with pruning
worth mentioning. The deletion of every single weight necessitated recalculation of $\mathbf{H}^{-1}$, a computational
burden not to be underestimated, though.

As pointed out by the authors, in some rare cases such as two-layer xor, both Magnitude method and Optimal Brain Damage
might delete an incorrect weight, and this mistake could not be overcome by further network training, so that they could
never learn that function correctly (i.e. achieve zero error).

<center>
    <img src="{{ "images/20191112-1.png" }}" alt="OBS outperformed others"/>
    <font face="Lora">Figure 1: Comparison That Demonstrates The Superiority Of Optimal Brain Damage (OBS). By courtesy of Figure 3 in
    [Babak Hassibi et al. (1993)](#references). Reproduced for better visualization.</font>
</center>

## Cascade-Correlation
One contrary idea to network pruning is to begin with a network containing no hidden units, then grow the network as
needed by adding hidden units until the training error is reduced to some acceptable level. The Cascade-Correlation
algorithm suggested by [Fahlman and Lebiere (1990)](#references) is one such algorithm.

More to come...

## Dropout
Proposed by [Nitish Srivastava et al. (2014)](#references), Dropout quickly became an entrancing tool and almost
constantly achieved state-of-the-art performance for general neural network pruning. Moreover, the idea of dropout is
not limited to feed-forward neural nets. Its use extends naturally to a wider variety of machine learning models beyond
neural networks, such as graphical models and Boltzmann Machines.

More to come in a dedicated post on the technique of Dropout...

## References
Yann Le Cun, John S. Denker and Sara A. Solla. 1989. Optimal Brain Damage. Advances in NIPS.

Babak Hassibi, David G. Stork and Gregory J. Wolff. 1993. Optimal Brain Surgeon and General Network Pruning. IEEE
International Conference on Neural Networks.

Nitish Srivastava, Geoffrey Hinton, Alex Krizhevsky, Ilya Sutskever and Ruslan Salakhutdinov. 2014. Dropout: A Simple
Way to Prevent Neural Networks from Overfitting. Journal of Machine Learning Research.

Fahlman, S.E. and Liebiere, C. 1990. The Cascade-Correlation Learning Architecture. Advances in Neural Information
Processing Systems.

{% include links.html %}
