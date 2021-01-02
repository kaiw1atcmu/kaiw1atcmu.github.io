---
title: "Gradient Descent, Newton's Method, and Beyond [more to come]"
keywords: gradient descent, newton's method
date: 2020-02-02 18:22:35 -0800
last_updated: December 26, 2020
tags: [mathematics,machine_learning,deep_learning]
summary: "Optimization methods always lie in the core of many successful machine learning algorithms. Popular
optimization methods include gradient descent or steepest descent, Newton's method, quasi Newton method,
Davidson-Fletcher-Powell (DFP) algorithm, Broyden-Fletcher-Goldfarb-Shanno (BFGS) algorithm, limited-memory BFGS or
limited-storage BFGS (L-BFGS) algorithm, and tons of variant algorithms based on them."
sidebar: mydoc_sidebar
permalink: gradient_descent_newtons_method_and_beyond.html
folder: mydoc
---

Optimization methods always lie in the core of many successful machine learning algorithms. Popular optimization
methods include gradient descent or steepest descent, Newton's method, quasi Newton method,
Davidson-Fletcher-Powell (DFP) algorithm, Broyden-Fletcher-Goldfarb-Shanno (BFGS) algorithm, limited-memory BFGS or
limited-storage BFGS (L-BFGS) algorithm, and tons of variant algorithms based on them.

## Gradient Descent or Steepest Descent
$$
  {\mathbf{x}_{k+1}=\mathbf{x}_k-\lambda\nabla f(\mathbf{x}_k).}
$$

Compared with Newton's method and other second-order derivative based methods, gradient descent's first-order nature
makes it unaware of and insensitive to saddle points. As a result, it might succeed where second-order methods like
Newton's method might fail, for instance, near a saddle point. However, gradient descent fails to exploit the curvature
information contained in the Hessian matrix, which makes it difficult to choose a good stepsize $\lambda$. The
appropriate stepsize must be small enough to avoid overshooting the minimum and going uphill in directions with strong
positive curvature.

<img src="{{ "images/20200202-1.png" }}" alt="overshooting"/>
_Figure 1: Overshooting Caused By Inappropriate Hessian Matrix. By courtesy of Figure 4.6 in
[Ian Goodfellow et al.](#references)._

## Gradient Descent or Steepest Descent with Line Search
Figure 1 illustrates an example of overshooting occurred while performing the gradient descent method. Another approach
called line search is to evaluate $\mathbf{x}_k-\lambda\nabla f(\mathbf{x}_k)$ for several values of $\lambda$ and
choose the one that results in the smallest objective function value.

$$
  {\lambda_k=\arg\min_{\lambda\geq0}f(\mathbf{x}_k-\lambda\nabla f(\mathbf{x}_k)),}\\
  {\mathbf{x}_{k+1}=\mathbf{x}_k-\lambda_k\nabla f(\mathbf{x}_k).}
$$

## Newton's Method
Newton's method aims to approximate the second-order Taylor expansion of the objective function $f(\mathbf{x})$ around
$\mathbf{x}_k$,

$$
  {f(\mathbf{x})\approx f(\mathbf{x}_k)+\nabla f(\mathbf{x}_k)^t(\mathbf{x}-\mathbf{x}_k)
  +\frac{1}{2}(\mathbf{x}-\mathbf{x}_k)^t\nabla^2f(\mathbf{x}_k)(\mathbf{x}-\mathbf{x}_k).}
$$

Denote $\nabla f(\mathbf{x}_k)$, the gradient vector of $f(\mathbf{x})$ at $\mathbf{x}_k$, as $\mathbf{g}_k$, and
$\nabla^2 f(\mathbf{x}_k)$, the Hessian matrix of $f(\mathbf{x})$ at $\mathbf{x}_k$, as $\mathbf{H}_k$ for notational
simplicity. Since we are solving for $\mathbf{x}$ which corresponds to a local minimum of the objective function, for
Hessian matrix $\mathbf{H}_k$ it follows that

$$
  {\mathbf{g}_k+\mathbf{H}_k(\mathbf{x}-\mathbf{x}_k)\approx0,}\\
  {\mathbf{x}\approx\mathbf{x}_k-\mathbf{H}_k^{-1}\mathbf{g}_k,}
$$

where we call $\mathbf{d}_k=-\mathbf{H}_k^{-1}\mathbf{g}_k$, the direction of change in $\mathbf{x}$, Newton' direction.
Then, starting from an arbitrarily chosen $\mathbf{x}_0$, we have the recurrence relation

$$
  {\mathbf{x}_{k+1}=\mathbf{x}_k-\mathbf{H}_k^{-1}\mathbf{g}_k,\ k=0,1,\ldots.}
$$

This original Newton's method performs best for objective functions with strong quadratic properties, or in the
proximity of local minima for general objective functions. However, if these conditions are not satisfied, Newton's
method might result in $f(\mathbf{x}_{k+1})>f(\mathbf{x}_k)$ which might even fail to converge, or end up being
attracted to saddle points where $\mathbf{H}_k$ is not guaranteed to be positive definite, a key observation suggested
by [Ian Goodfellow (2016) et al.](#references).

## Damped Newton Method
To remedy these undesirable effects, the damped Newton method, an improved version of Newton's method, adopts the
line-search philosophy and introduces an additional stepsize parameter $\lambda$. The damped Newton method picks out the
stepsize $\lambda$ which maximizes reduction in the objective function $f(\mathbf{x})$, that is:

$$
  {\lambda_k=\arg\min_{\lambda\geq0}f(\mathbf{x}_k+\lambda\mathbf{d}_k).}
$$

We should point out that the original Newton's method does not require solving for the best stepsize parameter $\lambda$
explicitly, since the default choice $\lambda=1$ is already optimal, supposed that the objective function
$f(\mathbf{x})$ is quadratic. However, for general classes of $f(\mathbf{x})$, $\lambda=1$ will probably not produce the
maximal reduction in objective function.

## Quasi Newton Method
The original Newton method is high-demanding for computational resources and memory space, and special algorithms should
be devised so that the calculation of the Hessian matrix $\mathbf{H}$ is carried out in an efficient fashion. What is
even worse, the Newton method might get trapped at saddle points and fail to capture true local minima. To overcome
these shortcomings, the family of quasi Newton methods strives to approximate a positive definite matrix whose behavior
adequately resembles that of the Hessian matrix, typically formed by a recurrence relation. To achieve this goal, the
quasi Newton method must be consistent with a condition required by the Hessian matrix, known as the quasi Newton
condition or the secant condition. Recall the Taylor expansion of $f(\mathbf{x})$ at $\mathbf{x}_{k+1}$,

$$
  {f(\mathbf{x})\approx f(\mathbf{x}_{k+1})+\nabla f(\mathbf{x}_{k+1})^t(\mathbf{x}-\mathbf{x}_{k+1})
  +\frac{1}{2}(\mathbf{x}-\mathbf{x}_{k+1})^t\nabla^2f(\mathbf{x}_{k+1})(\mathbf{x}-\mathbf{x}_{k+1}).}
$$

By reapplying the gradient operator to expressions on both sides, we have

$$
  {\nabla f(\mathbf{x})\approx\nabla f(\mathbf{x}_{k+1})+\nabla^2f(\mathbf{x}_{k+1})(\mathbf{x}-\mathbf{x}_{k+1}).}
$$

By setting $\mathbf{s_k}=\mathbf{x_{k+1}}-\mathbf{x_k}$ and $\mathbf{y_k}=\mathbf{g}_{k+1}-\mathbf{g}_k$ to avoid
clutter, at the point $\mathbf{x}=\mathbf{x}_k$ the equation becomes

$$
  {\mathbf{y_k}\approx\mathbf{H}_{k+1}\mathbf{s}_k.}
$$

Under the quasi Newton condition or the secant condition, we approximate $\mathbf{H}_k$ with $\mathbf{B}_k$, or
equivalently, approximate $\mathbf{H}_k^{-1}$ with $\mathbf{G}_k$, by equating both sides. That is,

$$
  {\mathbf{s}_k=\mathbf{G}_{k+1}\mathbf{y}_k,}\\
  {\mathbf{y}_k=\mathbf{B}_{k+1}\mathbf{s}_k.}
$$

Next, we introduce a couple of variants on quasi Newton methods, which construct recurrence relations on positive
definite Hessian matrices while ensuring the quasi Newton condition or the secant condition. That is,

$$
  {\mathbf{G}_{k+1}=\mathbf{G}_k+\Delta\mathbf{G}_k,}\\
  {\mathbf{B}_{k+1}=\mathbf{B}_k+\Delta\mathbf{B}_k.}
$$

## The Davidson-Fletcher-Powell (DFP) Algorithm
The DFP algorithm assumes that $\Delta\mathbf{G_k}$ is composed of two terms of rank one, and then
$\mathbf{G}_{k+1}=\mathbf{G}_k+\mathbf{P}_k+\mathbf{Q}_k$. Multiplying both side by $\mathbf{y}_k$ on the right,

$$
  {\mathbf{G}_{k+1}\mathbf{y}_k=\mathbf{G}_k\mathbf{y}_k+\mathbf{P}_k\mathbf{y}_k+\mathbf{Q}_k\mathbf{y}_k.}
$$

A reasonable strategy is to choose

$$
  {\mathbf{P}_k=\frac{\mathbf{s}_k\mathbf{s}_k^t}{\mathbf{s}_k^t\mathbf{y}_k},}\\
  {\mathbf{Q}_k=-\frac{\mathbf{G}_k\mathbf{y}_k\mathbf{y}_k^t\mathbf{G}_k}{\mathbf{y}_k^t\mathbf{G}_k\mathbf{y}_k},}
$$

which satisfies the quasi Newton condition or the secant condition by ensuring

$$
  {\mathbf{P}_k\mathbf{y}_k=\mathbf{s}_k,}\\
  {\mathbf{Q}_k\mathbf{y}_k=-\mathbf{G}_k\mathbf{y}_k.}
$$

Fletcher et al. showed that if $\mathbf{G_k}$ is symmetric and positive definite, so is $\mathbf{G}_{k+1}$. Starting
from an arbitrary $\mathbf{x}_0$ and $\mathbf{G}_0=\mathbf{I}$, the DFP algorithm's framework is formulated by:

> $\mathbf{d_k}=-\mathbf{G_k}\mathbf{g_k}$.  
> $\lambda_k=\arg\min_{\lambda\geq0}f(\mathbf{x_k}+\lambda\mathbf{d_k})$.  
> $\mathbf{x_{k+1}}=\mathbf{x_k}+\lambda_k\mathbf{d_k}$. Calculate $\mathbf{G_{k+1}}
=\mathbf{G}_k+\Delta\mathbf{G}_k$ and increment $k$.

## The Broyden-Fletcher-Goldfarb-Shanno (BFGS) Algorithm
Like the DFP algorithm, the BFGS algorithm assumes that $\Delta\mathbf{B_k}$ is composed of two terms of rank one, and
then $\mathbf{B}_{k+1}=\mathbf{B}_k+\mathbf{P}_k+\mathbf{Q}_k$. Multiplying both side by $\mathbf{s}_k$ on the right,

$$
  {\mathbf{B}_{k+1}\mathbf{s}_k=\mathbf{B}_k\mathbf{s}_k+\mathbf{P}_k\mathbf{s}_k+\mathbf{Q}_k\mathbf{s}_k.}
$$

A reasonable strategy is to choose

$$
  {\mathbf{P}_k=\frac{\mathbf{y}_k\mathbf{y}_k^t}{\mathbf{y}_k^t\mathbf{s}_k},}\\
  {\mathbf{Q}_k=-\frac{\mathbf{B}_k\mathbf{s}_k\mathbf{s}_k^t\mathbf{B}_k}{\mathbf{s}_k^t\mathbf{B}_k\mathbf{s}_k},}
$$

which satisfies the quasi Newton condition or the secant condition by ensuring

$$
  {\mathbf{P}_k\mathbf{s}_k=\mathbf{y}_k,}\\
  {\mathbf{Q}_k\mathbf{s}_k=-\mathbf{B}_k\mathbf{s}_k.}
$$

Fletcher et al. showed that if $\mathbf{B_k}$ is symmetric and positive definite, so is $\mathbf{B}_{k+1}$. Starting
from an arbitrary $\mathbf{x}_0$ and $\mathbf{B}_0=\mathbf{I}$, the BFGS algorithm's framework is formulated by:

> $\mathbf{B_k}\mathbf{d_k}=-\mathbf{g_k}$.  
> $\lambda_k=\arg\min_{\lambda\geq0}f(\mathbf{x_k}+\lambda\mathbf{d_k})$.  
> $\mathbf{x_{k+1}}=\mathbf{x_k}+\lambda_k\mathbf{d_k}$. Calculate $\mathbf{B}_{k+1}
=\mathbf{B}_k+\Delta\mathbf{B}_k$ and increment $k$.

<img src="{{ "images/20200202-2.jpg" }}" alt="BFGS Gang of Four"/>
_Figure 2: Yet Another "Gang Of Four". By courtesy of [understanding-lbfgs](#references)._

## Broyden's Algorithm
Denote $\mathbf{G_k}=\mathbf{B_k}^{-1}$ and $\mathbf{G_{k+1}}=\mathbf{B}_{k+1}^{-1}$, and apply twice the
Sherman-Morrison-Woodbury identity

$$
  {(\mathbf{M}+\mathbf{uv}^T)^{-1}=\mathbf{M}^{-1}
  -\frac{\mathbf{M}^{-1}\mathbf{uv}^T\mathbf{M}^{-1}}{1+\mathbf{v}^T\mathbf{M}^{-1}\mathbf{u}},}
$$

we arrive at the BFGS algorithm's recursion formula for $\mathbf{G}_k$, denoted as $\mathbf{G}_k^{DFGS}$ to be
differentiated from $\mathbf{G}_k^{DFP}=\mathbf{G}_k$,

$$
  {\mathbf{G}_{k+1}=(\mathbf{I}-\frac{\mathbf{s}_k\mathbf{y}_k^t}{\mathbf{s}_k^t\mathbf{y}_k})\mathbf{G}_k
  (\mathbf{I}-\frac{\mathbf{s}_k\mathbf{y}_k^t}{\mathbf{s}_k^t\mathbf{y}_k})^t
  +\frac{\mathbf{s}_k\mathbf{s}_k^t}{\mathbf{s}_k^t\mathbf{y}_k}.}
$$

Apparently, any convex combination of $\mathbf{G}_k^{DFP}$ and $\mathbf{G}_k^{DFGS}$ is a valid choice of $\mathbf{G}_k$
that is consistent with the quasi Newton condition or the secant condition, i.e.,

$$
  {\mathbf{G}_k=\alpha\mathbf{G}_k^{DFP}+(1-\alpha)\mathbf{G}_k^{DFGS},\ 0\leq\alpha\leq1.}
$$

## The Limited-Memory BFGS or Limited-Storage BFGS (L-BFGS) Algorithm
The BFGS algorithm, in its pure form, makes it necessary to allocate up to $O(N^2)$ memory units for entries in the huge
Hessian matrix. For computer systems with only limited memory space, we could instead store $2m$ most recent vectors
$\mathbf{s_k}$ and $\mathbf{y_k}$, requiring only up to $O(Nm)$ memory units organized as a FIFO queue. As $k$
increases, fresh vectors $\mathbf{s_k}$ and $\mathbf{y_k}$ are pushed into the queue and stale vectors
$\mathbf{s_{k-m}}$ and $\mathbf{y_{k-m}}$ are popped out from the queue. In each run of iteration, $\mathbf{G_{k+1}}$ is
initialized with $\mathbf{I}$ and produced by computation involving only the most recent $2m$ vectors
$\{\mathbf{s_k},\ldots,\mathbf{s_{k-m+1}}\}$ and $\{\mathbf{y_k},\ldots,\mathbf{y}_{k-m+1}\}$. Fortunately, the
procedure can be dramatically simplified using only vector computations, as suggested by
[Jorge Nocedal (1980)](#references).

## References
Jorge Nocedal. 1980. Updating Quasi-Newton Matrices With Limited Storage. Mathematics of Computation Volume 35 (151):
773--782.

Ian Goodfellow, Yoshua Bengio, and Aaron Courville. 2016. Deep Learning. Chapter 4.

[understanding-lbfgs](http://aria42.com/blog/2014/12/understanding-lbfgs)

{% include links.html %}
