--- 
title: "Kullback-Leibler Divergence, Cross-Entropy, And Mutual Information"
keywords: kullback-leibler divergence, cross-entropy, mutual information
date: 2019-12-09 15:28:32 -0800
last_updated: December 26, 2020
tags: [machine_learning]
summary: "The concepts of Kullback-Leibler (KL) divergence, cross-entropy (CE), and mutual information (MI) lie in the
core of many machine learning optimization tasks."
sidebar: none
permalink: kullback-leibler_divergence_cross-entropy_and_mutual_information.html
folder: mydoc
---

The concepts of <font face="Lora">Kullback-Leibler (KL) divergence</font>, <font face="Lora">cross-entropy (CE)</font>,
and <font face="Lora">mutual information (MI)</font> lie in the core of many machine learning optimization tasks.
According to [Ian Goodfellow et al. (2016)](#references), the use of cross-entropy losses greatly improved the
performance of models with sigmoid and softmax outputs, which had previously suffered from saturation and slow learning
when using the mean squared error loss.

## The Kullback-Leibler (KL) Divergence
The Kullback-Leibler (KL) divergence, a.k.a. <font face="Lora">relative entropy</font>, measures the extent to which two
separate probability distributions $p(x)$ and $q(x)$ over the same random variable $x\in\mathcal{X}$ differ:

$$ 
  {D_{\text{KL}}(p\Vert q)=\mathbb{E}_{\mathbf{x}\sim p}\left[\log\frac{p(x)}{q(x)}\right]=\mathbb{E}_{\mathbf{x}\sim p}[\log p(x)-\log q(x)].}
$$

The KL divergence is non-negative and attains $0$ if and only if $p$ and $q$ are the same distribution in the case of
discrete variables, or equal "almost everywhere" in the case of continuous variables. In spite of this, the KL
divergence is not a true distance measure since it is not symmetric:
 
$$
  {\exists\text{ distributions }p,\ q,\quad D_{\text{KL}}(p\Vert q)\not=D_{\text{KL}}(q\Vert p).}
$$

Moreover, the KL divergence may violate the triangle inequality required by a true distance measure. That is to say,

$$
  {\exists\text{ distributions }p,\ q,\ r,\quad D_{\text{KL}}(p\Vert q)+D_{\text{KL}}(q\Vert r)\lt D_{\text{KL}}(p\Vert r),}
$$

which is to show

$$
  {\exists\text{ distributions }p,\ q,\ r,\quad\int q(x)\log\frac{q(x)}{r(x)}dx\lt \int p(x)\log\frac{q(x)}{r(x)}dx.}
$$

Let us construct an obvious example to confirm that such distributions $p, q, r$ exist. Imagine distributions $q,r$ are
given and $\exists\ x,\ q(x)\not=r(x)$, then there are regions of $x$ where $\log q(x)/r(x)$ takes on positive and
negative values, respectively. The distribution of $p$ is rendered by assigning higher probabilities than $q$ for
regions where $\log q(x)/r(x)>0$, and lower probabilities than $q$ elsewhere, as long as its normalization conditions
$p(x)\geq0$ and $\int p(x)dx=1$ hold true. Put together, these two reasons may in part explain why the KL divergence is
called a "divergence", not a "distance".

Intuitively, a symmetric version of the KL divergence, called the <font face="Lora">Jensen-Shannon divergence</font>,
more resembles a true distance measure, except for the triangle inequality. That is defined to be

$$
  {D_{\text{JS}}(p\Vert q)=\frac{1}{2}D_{\text{KL}}(p\Vert\frac{p+q}{2})+\frac{1}{2}D_{\text{KL}}(q\Vert\frac{p+q}{2}).}
$$

It remains to show why the triangle inequality still does not hold for $D_{\text{JS}}(p\Vert q)$.

### Choose Between $D_{\text{KL}}(p\Vert q)$ and $D_{\text{KL}}(q\Vert p)$
<center>
    <img src="{{ "images/20191209-1.png" }}" alt="KL Divergence"/>
    <font face="Lora">Figure 1. Asymmetry in the KL-divergence by courtesy of [Kevin P. Murphy (2012)](#references) Figure 21.1.</font>
</center>

Suppose we wish to approximate a multimodal Gaussian distribution $p(x)$ with a unimodal Gaussian distribution $q(x)$.
The blue curves are the contours of the true distribution $p$. The red curves are the contours of the unimodal
approximation $q$. (a) minimizes $D_{\text{KL}}(p\Vert q)$ and $q$ tends to cover $p$, whereas (b) and (c) minimize
$D_{\text{KL}}(q\Vert p)$ and $q$ locks on to one of the two modes.

* $D_{\text{KL}}(p\Vert q)$ is called the <font face="Lora">forward KL divergence</font>, the <font face="Lora">moment
projection</font>, or simply the <font face="Lora">M-projection</font>. This is infinite if $q(x)=0$ and $p(x)>0$. Thus
if $p(x)>0$ we must ensure $q(x)>0$. This <font face="Lora">zero-avoiding</font> property for $q$ will typically
overestimate the support of $p$.

* $D_{\text{KL}}(q\Vert p)$ is called the <font face="Lora">reverse KL divergence</font>,
the <font face="Lora">information projection</font>, or simply the <font face="Lora">I-projection</font>. This is
infinite if $p(x)=0$ and $q(x)>0$. Thus if $p(x)=0$ we must ensure $q(x)=0$. This <font face="Lora">zero-forcing</font>
property for $q$ will typically underestimate the support of $p$.

As [Ian Goodfellow et al. (2016)](#references) pointed out, the choice of which direction of the KL divergence to use is
problem-dependent. Some applications require an approximation that usually places high probability anywhere that the
true distribution places high probability (e.g., in variational inference the solution of $q$ which merely tries to
capture one mode of $p$ is considered more appropriate than one that blurs multiple modes together), while other
applications require an approximation that rarely places high probability anywhere that the true distribution places low
probability (e.g. Can you come up with an example?). The choice of the direction of the KL divergence reflects
which of these considerations takes priority for each application.

{% include note.html content="Can you come up with an use case that requires an approximation that rarely places high
probability anywhere that the true distribution places low probability?" %}

Sometimes we have to take into account computational issues. For example, in multi-class classifiers,
$D_{\text{KL}}(p\Vert q)$ should be preferred over $D_{\text{KL}}(q\Vert p)$, because the true targets $p$ can only
take on values $0$ or $1$, and thus $\log p$ will either take on values $-\infty$ or $0$, making optimizations
concerning cost function $D_{\text{KL}}(p\Vert q)$ rather inconvenient.

### An Interesting Correspondence between KL divergence and EM algorithm
Interestingly, the <font face="Lora">expectations maximization (EM)</font> algorithm bears a great resemblance to the KL
divergence, in that the EM algorithm is reformulated as a procedure that alternatingly optimizes
$D_{\text{KL}}(p\Vert q)$ and $D_{\text{KL}}(q\Vert p)$.

Suppose $p$ is not fixed and parametrized by $\theta$, and we are tasked with approximating $p$ with $q$. Recall the EM
algorithm is equivalent to the maximization maximization algorithm on the induced $F$ function w.r.t. $\theta$ and $q$
alternatingly. As a reminder, the $F$ function is given by

$$
  {F(q(z),\theta^{(i)})=\sum_z\log p(z,x\vert\theta^{(i)})q(z)+H(q(z)).}
$$

Bearing in mind that optimal $q(z)$ and $\theta$ which maximize the $F$ function depend on $x$ and $\theta^{(i)}$, we
can write $p(z,x\vert\theta^{(i)})$ as $p(z\vert\theta^{(i)})$ and $q(z\vert x,\theta^{(i)})$ as $q(z)^{(i)}$ for
notational simplicity.

* The E-step corresponds to maximizing $F(q(z),\theta^{(i)})$ w.r.t. $q(z)$, i.e.

$$
  {q(z)^{(i+1)}=\max_{q(z)}D_{\text{KL}}(q(z)\Vert p(z\vert\theta^{(i)})),}
$$ 

* The M-step corresponds to maximizing $F(q(z)^{(i)},\theta)$ w.r.t. $\theta$, i.e.

$$
  {\theta^{(i+1)}=\max_{\theta}D_{\text{KL}}(q(z)^{(i+1)}\Vert p(z\vert\theta)).}
$$

We conclude our discussion by proposing an intuitive explanation on how these E-steps (reverse KL divergence) and
M-steps (forward KL divergence) approximate $p$ with $q$. Recall that the reverse KL divergence is zero-forcing and
underestimates (shrinks) the support of $p$, and the forward KL divergence is zero-avoiding and overestimates (expands)
the support of $p$, then a procedure that alternates between such counteracting operations is expected to let $q$
stabilize near a reasonable approximation of $p$.

## The Cross-Entropy (CE)
A closely related quantity to the KL divergence is the cross-entropy $CE(p\Vert q)=H(p)+D_{\text{KL}}(p\Vert q)$, which
is similar to the KL divergence but lacking the term on the left:

$$
  {CE(p\Vert q)=-\mathbb{E}_{\mathbf{x}\sim p}\log q(x).}
$$

Minimizing the cross-entropy with respect to $q$ is equivalent to minimizing the KL divergence, since $q$ does not
participate in the omitted term.

## The Mutual Information (MI)
In information-theoretic literature, another closely related concept is the mutual information, a measure to quantify
"how much information we know about $\mathcal{X}$ from $\mathcal{Y}$; or equivalently, the other way round". That is,

$$
  {I(\mathcal{X},\mathcal{Y})=H(\mathcal{X},\mathcal{Y})-H(\mathcal{Y})=H(\mathcal{X})-H(\mathcal{X}\vert\mathcal{Y}).}
$$

The mutual information has a desirable property of symmetry, i.e.
$I(\mathcal{X},\mathcal{Y})=I(\mathcal{Y},\mathcal{X})$. In addition, an interesting identity exists that relates KL
divergence to mutual information and demonstrates this symmetry,

$$
  {I(\mathcal{X},\mathcal{Y})=D_{\text{KL}}(p(x,y)\Vert p(x)p(y)).}
$$

## References
Ian Goodfellow, Yoshua Bengio, and Aaron Courville. 2016. Deep Learning. Chapter 3.13.

Kevin P. Murphy. 2012. Machine Learning: A Probabilistic Perspective. Chapter 21.2.

{% include links.html %}
