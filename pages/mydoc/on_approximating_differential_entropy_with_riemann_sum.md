---
title: "On Approximating Differential Entropy With Riemann Sum"
keywords: differential entropy, entropy, Riemann sum
date: 2020-08-12 10:10:15 -0800
last_updated: August 12, 2020
tags: [maths,statistics,machine_learning]
summary: "This post formulated a general procedural framework on approximating differential entropy with Riemann sum, a
basic practice borrowed from mathematical analysis. The author made the loose assumption that intended readers should be
familiar with Calculus and its formal definitions."
sidebar: mydoc_sidebar
permalink: on_approximating_differential_entropy_with_riemann_sum.html
folder: mydoc
---

It seems obvious to approximate differential entropy (i.e., the entropy of continuous variables) with Riemann sum. But
unfortunately, there are lots of intricacies involved in doing so. It deserves an entire blog post to disentangle the
nuances and sort all tricky things out.

## Differential Entropy Has Bad Features
For a continuously distributed variable $\mathcal{X}$ with finite support $[a,b]$, we further assume its probability
density $p(x)\in C^0$ and its differential entropy's integrand $p(x)\log\frac{1}{p(x)}\in C^0$, and recall that its
differential entropy is defined to be

$$
  {H(\mathcal{X})=\int_a^b p(x)\log\frac{1}{p(x)}dx.}
$$

In real-world applications, we are provided with a large number of observations assumed to be drawn i.i.d. from
$\mathcal{X}$, and we should approximate the true differential entropy $H(\mathcal{X})$ with its discrete counterpart
$H(\mathcal{X},\Delta_I)$, where $\Delta_I$ indicates a finite set of sub-intervals $[a_i,b_i]$ that partition variable
$\mathcal{X}$'s support $[a,b]$. That is,

$$
  {H(\mathcal{X},\Delta_I)=\sum_{i\in I}\int_{\Delta_i}p(x)dx\log\frac{1}{\int_{\Delta_i}p(x)dx}.}
$$

Unfortunately, this approximated differential entropy approaches infinity as the partitioning sub-intervals $\Delta_I$
grow in number and become finer in lengths. Because $H(\mathcal{X},\Delta_I)$ is a discrete entropy and is interpreted
as the minimum average length to encode the variable $\mathcal{X}$, it must go unbounded when $\mathcal{X}$ can take on
infinitely many values.

Another inconvenience associated with differential entropy is its lack of invariance under bijective mappings. To see
this, consider a bijective mapping $p\mapsto P$ from the probability density function $p(x)$ to its probability
cumulative function $P(x)$, which is bijective as $p(x)>0$ in $\mathcal{X}$'s support $[a,b]$. Since the probability
cumulative function $P(x)$ is always uniform in $[0,1]$ irrespective of $p(x)$ (a basic result taken from probability
theory), its differential entropy is constantly $0$ and cannot equal that of an arbitrary density function having
support $[a,b]$. A contradiction. In contrast, the fact that discrete entropy is invariant under bijective mappings is
self-evident.

{% include note.html content="If you're not clear with this important property of the probability cumulative function,
please consult any elementary textbook on probability theory for clarification." %}

## The Riemann Integral
Applying the mean value theorem of calculus to $p(x)$, we reformulate integral with summation since
$\forall\Delta_i\in\Delta_I, \exists\xi_i\in\Delta_i$, such that $\int_{\Delta_i}p(x)dx=p(\xi_i)\Delta_i$. Insert this
into $H(\mathcal{X},\Delta_I)$ and we have

$$
  {H(\mathcal{X},\Delta_I)=\sum_{i\in I}p(\xi_i)\log\frac{1}{p(\xi_i)\Delta_i}\Delta_i
  =\sum_{i\in I}p(\xi_i)\log\frac{1}{p(\xi_i)}\Delta_i+\sum_{i\in I}p(\xi_i)\Delta_i\log\frac{1}{\Delta_i}.}
$$

Applying the mean value theorem of calculus to $p(x)\log\frac{1}{p(x)}$, it follows that
$\forall\Delta_i\in\Delta_I,\exists\xi_i'\in\Delta_i$, such that
$\int_{\Delta_i}p(x)\log\frac{1}{p(x)}dx=p(\xi_i')\log\frac{1}{p(\xi_i')}\Delta_i$.Since $p(x)\log\frac{1}{p(x)}$ is
continuous with finite support $[a,b]$, it is therefore Riemann integrable. By the definition of Riemann integral,

$$
  {\forall\eta>0,\exists\epsilon>0,\forall\Delta_I\text{ s.t. }\max_{i\in I}{\Delta_i}\leq\epsilon, \forall\zeta_i,\zeta_i'\in\Delta_i,
  \vert\sum_{i\in I}p(\zeta_i')\log\frac{1}{p(\zeta_i')}\Delta_i-\sum_{i\in I}p(\zeta_i)\log\frac{1}{p(\zeta_i)}\Delta_i\vert\leq\eta.}
$$

By the property of Riemann integrable functions, the choice of $\zeta_i$ and $\zeta_i'$ are immaterial for the
inequality to hold, as long as $\max_{i\in I}\Delta_i\leq\epsilon$ is satisfied. Letting
$\zeta_i=\xi_i,\zeta_i'=\xi_i'$, we have

$$
  {\vert H(\mathcal{X})-H(\mathcal{X},\Delta_I)+\sum_{i\in I}p(\xi_i)\Delta_i\log\frac{1}{\Delta_i}\vert
  =\vert\sum_{i\in I}p(\zeta_i')\log\frac{1}{p(\zeta_i')}\Delta_i-\sum_{i\in I}p(\zeta_i)\log\frac{1}{p(\zeta_i)}\Delta_i\vert\leq\eta.}
$$

Letting $\Delta_i=\frac{1}{n}$ for the sake of simplicity, the infinitesimal terms
$\sum_ip(\xi_i)\Delta_i\log\frac{1}{\Delta_i}$ simplify to $\log\frac{1}{\Delta_i}$. Summing up,

$$
  {\lim_{n\to\infty}\left\{H(\mathcal{X},\Delta_n)-\log\frac{n}{b\!-\!a}\right\}=H(\mathcal{X}).}
$$

## Mutual Information Remains Unaffected for Most Discrete Conditioning Variables
Apparently, we have to exclude the term $\log\frac{n}{b-a}$ to account for the difference in partition granularity.
Because in many algorithms such as Chow-Liu tree or ID3 decision tree, we are more frequently concerned with estimating
the *mutual information (MI)*, $I(\mathcal{X},\mathcal{Y})=H(\mathcal{X})-H(\mathcal{X}\vert\mathcal{Y})$, we wonder if
it's still valid without taking into account that term. Fortunately, the answer is yes for a discrete conditioning
variable $\mathcal{Y}$. To see this, for conditional differential entropy $H(\mathcal{X}\vert\mathcal{Y})$, if the
bivariate function $F(n,y)=H(\mathcal{X},\Delta_n\vert y)-\log\frac{n}{b-a}-H(\mathcal{X}\vert y)$ uniformly converges
in $y\in\mathcal{Y}$ to $0$ as $n\to\infty$, then infinitesimal terms cancel out exactly. That is,

$$
  {\lim_{n\to\infty}\left\{(H(\mathcal{X})-H(\mathcal{X},\Delta_n))-(H(\mathcal{X}\vert\mathcal{Y})-H(\mathcal{X},\Delta_n\vert\mathcal{Y}))\right\}=0,}\\
  {i.e.,\lim_{n\to\infty}\left\{H(\mathcal{X},\Delta_n)-H(\mathcal{X},\Delta_n\vert\mathcal{Y})\right\}
  =H(\mathcal{X})-H(\mathcal{X}\vert\mathcal{Y}).}
$$

{% include note.html content="The requirement of uniform convergence is a sufficient but not necessary condition for
the above equation to hold true. Intuitively, this corresponds exactly to finding a common $n$ and its resulting error
bound, for all possible choices of the conditioning variable $\mathcal{Y}$." %}

The uniform convergence condition holds true for nearly all discrete distributions of variable $\mathcal{Y}$, including
but not limited to $\mathcal{Y}$ taking on finitely many or countably many values. Naively, we are able to compute the
weighted conditional entropy $(H(\mathcal{X})-H(\mathcal{X}\vert\mathcal{Y}=y))\cdot P(\mathcal{Y}=y)$ for each
$y\in\mathcal{Y}$ and sum them up. However, we have a better way of doing it as explained below.

For a continuously distributed conditioning variable $\mathcal{Y}$, we should apply the 2-dimensional version Riemann
integral theorem to the formula below, and subtract two infinitesimal terms for $\mathcal{X}$ and $\mathcal{Y}$,
respectively.

$$
  {H(\mathcal{X},\mathcal{Y})=\int_c^d\int_a^bp(x,y)\log\frac{1}{p(x,y)}dxdy.}
$$

It is advisable to use the identity $H(\mathcal{X}\vert\mathcal{Y})=H(\mathcal{X},\mathcal{Y})-H(\mathcal{Y})$, to avoid
direct approximation of conditional probabilities.

## The Differential Entropy for a Variable with Infinite Support
It is not painstaking to extend our results to probability densities with infinite support $(-\infty,\infty)$, if we
call to mind the fact that integration over infinite intervals is defined to be the double limit when $a,b$ approach
$-\infty$ and $\infty$, respectively. Let us temporarily define a truncated differential entropy
$H_{[a,b]}(\mathcal{X})$ that is constrained in a finite interval $[a,b]$, then

$$
  {H(\mathcal{X})=\lim_{a\to-\infty\\ b\to\infty}\int_a^bp(x)\log\frac{1}{p(x)}dx=\lim_{a\to-\infty\\ b\to\infty}H_{[a,b]}(\mathcal{X}).}
$$

Denote $P_{[a,b]}(\mathcal{X})=\int_a^bp(x)dx$ as the truncated probability cumulative function and
$P_{[a,b]}(\mathcal{X},\Delta_n)$ as its value estimated by Riemann sum. Since

$$
  {H_{[a,b]}(\mathcal{X})=\lim_{n\to\infty}\left\{H_{[a,b]}(\mathcal{X},\Delta_n)-P_{[a,b]}(\mathcal{X},\Delta_n)\log\frac{n}{b\!-\!a}\right\},}
$$

then taking limit with respect to $a,b$ on both sides,

$$
  {H(\mathcal{X})=\lim_{a\to-\infty\\ b\to\infty}H_{[a,b]}(\mathcal{X})
  =\lim_{a\to-\infty\\ b\to\infty}\lim_{n\to\infty}\left\{H_{[a,b]}(\mathcal{X},\Delta_n)-P_{[a,b]}(\mathcal{X},\Delta_n)\log\frac{n}{b\!-\!a}\right\}.}
$$

Similar steps hold true when it comes to approximate mutual information for variables with infinite support.

## A Worked Example
Suppose we are asked to approximate the differential entropy of a variable $\mathcal{X}\sim\exp(1)$ with a plethora of
i.i.d. observations, and we decide to truncate the support from $[0,\infty)$ to $[0,b]$. Evenly partition the support
$[0,b]$ into $n$ sub-intervals, and suppose we have adequately precise estimates of each $p_i$, the probability
$\mathcal{X}$ fall into the $i$-th sub-interval. With some algebra we have $p_i=e^{-ib/n}(e^{b/n}-1)$, and then the
approximated differential entropy is

$$
  {\lim_{n\to\infty}H_{[a,b]}(\mathcal{X},\Delta_n)=\lim_{n\to\infty}\left\{\sum_{i=1}^np_i\log\frac{1}{p_i}-\sum_{i=1}^np_i\log\frac{n}{b\!-\!0}\right\}}\\
  {=\lim_{n\to\infty}\left\{(1-e^{-b})\log\frac{1}{(e^{b/n}\!-\!1)}+(e^{b/n}-1)\sum_{i=1}^ne^{-ib/n}\frac{ib}{n}-(1-e^{-b})\log\frac{n}{b}\right\}
  =\int_0^be^{-x}xdx=H_{[a,b]}(\mathcal{X}).}
$$

Taking limit with respect to $b$ on both sides ($a$ is set to $0$ and disappears), we have proved

$$
  {H(\mathcal{X})=\int_0^{\infty}e^{-x}xdx=\lim_{b\to\infty}\int_0^be^{-x}xdx
  =\lim_{a\to-\infty\\ b\to\infty}H_{[a,b]}(\mathcal{X}).}
$$

{% include links.html %}
