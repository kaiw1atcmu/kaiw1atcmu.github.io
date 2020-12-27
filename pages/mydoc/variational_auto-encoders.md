---
title: "Variational Auto-Encoders"
keywords: representation learning, variational inference, variational Bayes
date: 2020-08-21 21:18:19 -0800
last_updated: December 26, 2020
tags: [deep_learning,statistics]
summary: "This post introduced fundamentals of the variational auto-encoder (VAE), a powerful generative component for
representation learning, and explained its mathematical properties with applications."
sidebar: mydoc_sidebar
permalink: variational_auto-encoders.html
folder: mydoc
---

The recently published variational auto-encoders (VAE) have emerged as one basic building block for generative networks,
one of the most promising research sub-fields in deep learning. In this post, let's talk about rationales behind VAE and
go through a couple of typical applications of it.

## Why We Need Variational Inference?
The auto-encoding variational Bayes does not make the common simplifying about the marginal or posterior probabilities.
Conversely, this algorithm makes it more generally applicable to such problem scenarios including:

* **Intractability**. 1) The integral of the evidence function
$p_{\theta}(\mathbf{x})=\int p_{\theta}(\mathbf{z})p_{\theta}(\mathbf{x}\vert\mathbf{z})$ is intractable, or else we
will be able to differential $p_{\theta}(\mathbf{x})$ w.r.t. $\theta$ directly. Or, 2) the exact posterior density
$p_{\theta}(\mathbf{z}\vert\mathbf{x})$ is intractable, so the EM algorithm cannot be used. Or, 3) the required
integrals for any reasonable mean-field VB algorithm are also intractable, so the mean-field approach is not possible.
All these cases appear in the common case of moderately complicated likelihood functions
$p_{\theta}(\mathbf{x}\vert\mathbf{z})$, e.g. a neural network with a nonlinear hidden layer.
* **A large dataset**. For larger datasets, sampling-based solutions, e.g. Monte Carlo EM, would in general be too slow.
In addition, the SGVB algorithm naturally enables the online learning procedure, an advantage not found in most similar
algorithms.

The authors therefore proposed a solution to three related problems in the above scenario:

* Efficient approximate ML or MAP estimation for the parameters $\theta$.
* Efficient approximate posterior inference of the latent variable $\mathbf{z}$ given an observed value $\mathbf{x}$
for a choice of parameters $\theta$.
* Efficient approximate marginal inference of the variable $\mathbf{x}$. This allows us to perform all kinds of
inference tasks where a prior over $\mathbf{x}$ is required.

## The Model
The authors made some conventions for notational simplicity in variational inference. Given variational parameters
$\phi$ and generative parameters $\theta$, they introduced a **recognition model** or **encoder**
$q_{\phi}(\mathbf{z}\vert\mathbf{x})$, an approximation to the intractable true posterior
$p_{\theta}(\mathbf{z}\vert\mathbf{x})$, and a **generative model** or **decoder**
$p_{\theta}(\mathbf{x}\vert\mathbf{z})$, since given a code $\mathbf{z}$ it produces a distribution over the possible
corresponding values of $\mathbf{x}$. Without further simplifying assumptions, the authors introduced a method for
learning the recognition model parameters $\phi$ jointly with the generative model parameters $\theta$.

![the model](http://pic2.027cgb.com/627357/github_blog/20200821-2.PNG)
_Figure 1: The Type Of Directed Graphical Model Under Consideration_

In Figure 1, solid lines denote the generative model $p_{\theta}(\mathbf{z})p_{\theta}(\mathbf{x}\vert\mathbf{z})$,
dashed lines denote the variational approximation $q_{\phi}(\mathbf{z}\vert\mathbf{x})$ to the intractable posterior
$p_{\phi}(\mathbf{z}\vert\mathbf{x})$. The variational parameters $\phi$ are learned jointly with the generative model
parameters $\theta$.

Notice that in the variational inference literature, an **approximate** probability $q$ is used to emphasize the true
probability $p$ is intractable. In the above settings, since we normally assume that $\mathbf{x}$ given $\theta$ and
$\mathbf{z}$ are drawn from a predefined distribution, then its probability is **exact**, denoted by $p$.

## The Variational Bound
The marginal likelihood is composed of a sum over the marginal likelihoods of individual datapoints

$$
{\log p_{\theta}(\mathbf{x}^1;\ldots;\mathbf{x}^N)=\sum_{i=1}^N\log p_{\theta}(x^i),}
$$

which can each be rewritten as:

$$
{\log p_{\theta}(x^i)=\text{D}_{KL}(q_{\phi}(\mathbf{z}\vert\mathbf{x}^i)\Vert p_{\theta}(\mathbf{z}\vert\mathbf{x}^i))
+\mathcal{L}(\theta,\phi;\mathbf{x}^i).}
$$

Since the Kullback-Leibler divergence is non-negative, the $\mathcal{L}(\theta,\phi;\mathbf{x}^i)$ term is called the
(variational) lower bound or the more frequently referenced **evidence lower bound (ELBO)** on the marginal likelihood
of datapoint $\mathbf{x}^i$, and can be written as:

$$
{\log p_{\theta}(\mathbf{x}^i)\ge\mathcal{L}(\theta,\phi;\mathbf{x}^i)
=\mathbb{E}_{q_{\phi}(\mathbf{z}\vert\mathbf{x}^i)}[-\log q_{\phi}(\mathbf{z}\vert\mathbf{x}^i)+\log p_{\theta}(\mathbf{x}^i,\mathbf{z})],}
$$

which can also be rewritten as:

$$
{\underbrace{\log p_{\theta}(\mathbf{x}^i)}_{\text{evidence}}
\ge\underbrace{\mathcal{L}(\theta,\phi;\mathbf{x}^i)}_{\text{evidence lower bound (ELBO)}}
=\underbrace{-\text{D}_{KL}(q_{\phi}(\mathbf{z}\vert\mathbf{x}^i))}_{\text{regularization term}}
+\underbrace{\mathbb{E}_{q_{\phi}(\mathbf{z}\vert\mathbf{x}^i)}[\log p_{\theta}(\mathbf{x}^i\vert\mathbf{z})]}_{\text{reconstruction error}}.}
$$

The usual (naive) Monte Carlo gradient estimator is common in reinforcement learning (RL). However, this type of
estimator for the variational inference problem is:

$$
{\nabla_{\phi}\mathbb{E}_{q_{\phi}(\mathbf{z})}[f(\mathbf{z})]
=\mathbb{E}_{q_{\phi}(\mathbf{z})}[f(\mathbf{z})\nabla_{\phi}\log q_{\phi}(\mathbf{z})]
\simeq\frac{1}{L}\sum_{l=1}^Lf(\mathbf{z}^l)\nabla_{q_{\phi}(\mathbf{z}^l)}\log q_{\phi}(\mathbf{z}^l).}
$$

As in RL, the Monte Carlo gradient estimator exhibits very high variance and is impractical for the VI purposes,
although there already exist several algorithms to mitigate this problem. To effectively estimate
$\mathcal{L}(\theta,\phi;\mathbf{x}^i)$, the authors compared two alternatives of SGVB estimators:

$$
{\mathcal{L}(\theta,\phi;\mathbf{x}^i)\simeq\tilde{\mathcal{L}}^A(\theta,\phi;\mathbf{x}^i)=\frac{1}{L}\sum_{l=1}^L(\log p_{\theta}(\mathbf{x}^i,\mathbf{z}^{i,l})
-\log q_{\phi}(\mathbf{z}^{i,l}\vert\mathbf{x}^i)).}
$$

Often, the Kullback-Leibler divergence term can be integrated analytically, such that only the expected reconstruction
error $\mathbb{E_{q_{\phi}(\mathbf{z}\vert\mathbf{x}^i)}}[\log p_{\theta}(\mathbf{x}^i\vert\mathbf{z})]$ requires
estimation by sampling. This yields a second version of the SGVB estimator:

$$
{\mathcal{L}(\theta,\phi;\mathbf{x}^i)\simeq\tilde{\mathcal{L}}^B(\theta,\phi;\mathbf{x}^i)=-\text{D}_{KL}(q_{\phi}(\mathbf{z}\vert\mathbf{x}^i)\Vert p_{\theta}(\mathbf{z}))
+\frac{1}{L}\sum_{l=1}^L\log p_{\theta}(\mathbf{x}^i\vert\mathbf{z}^{i,l}).}
$$

## The Reparametrization Trick
In order to solve the problem the authors invoked an alternative method for generating samples from
$q_{\phi}(\mathbf{z}\vert\mathbf{x})$. The essential parameterization trick is quite simple. Since $\mathbf{z}$ is drawn
from a predefined distribution family $q_{\phi}(\mathbf{z}\vert\mathbf{x}^i)$, we are actually concerned with parameter
estimation by gradient descents of $f(\mathbf{z})$ w.r.t. these parameters. That is, $\mu^i$ and $\sigma^i$ for Gaussian
assumptions. To recognize the fact that $\sigma^i$ should be non-negative, a necessary engineering trick is to model
$\log(\sigma^i)^2=2\log\sigma^i$ instead of $\sigma^i$. Then,

$$
{\mathbf{z}=\mu^i+\sigma^i\epsilon\sim q_{\phi}(\mathbf{z}\vert\mathbf{x}^i)=\mathcal{N}(\mu^i,\sigma^i\epsilon),}
$$

where $\epsilon$ is an auxiliary noise variable $\epsilon\sim\mathcal{N}(0,1)$. In the training and test processes,
noise $\epsilon$ is randomly sampled entirely independent of the datasets and models. The authors formulated general
guidelines to generate $\mathbf{z}$ by change of variables:

* **Tractable inverse CDF**. Recall the basic result in probability theory that the value of the cumulative distribution
function (CDF) is always $\mathcal{U}(0,1)$ for a variable with any arbitrary probability density functions (PDFs). Then
it is straight-forward to sample from $\mathcal{U}(0,1)$ (i.e. $p(F(\mathbf{z}))$) and reversely map it to $\mathbf{z}$,
as long as the inverse CDF is tractable. Examples: Exponential, Cauchy, Logistic, Rayleigh, Pareto, Weibull, Reciprocal,
Gompertz, Gumbel and Erlang distributions.
* **Location-scale families**. For these distributions, we can choose the standard distribution (with
$\text{location}=0$ and $\text{scale}=1$) as the auxiliary variable $\epsilon$, and let
$g(\cdot)=\text{location}+\text{scale}\ \epsilon$. Examples: Laplace, Elliptical, Student's t, Logistic, Uniform,
Triangular and Gaussian distributions.
* **Composition**. It is often possible to express random variables as different transformations of auxiliary variables.
Examples: Log-Normal (exponentiation of normally distributed variable), Gamma (a sum over exponentially distributed
variables), Dirichlet (weighted sum of Gamma variates), Beta, Chi-Squared, and $F$ distributions.

## Discussions
Unsurprisingly, as a novel generative auto-encoder model, VAEs have interesting details for researchers to delve into.

### The Impact of Latent Dimensionalities
![the impact of latent dimensionalities](http://pic2.027cgb.com/627357/github_blog/20200821-1.PNG)
_Figure 2: The Impact Of Latent Dimensionalities_

Figure 2 demonstrates the impact of latent dimensionalities. Interestingly enough, more latent variables does not result
in more overfitting, which is explained by the regularizing effect of the lower bound imposed by the Kullback-Leibler
divergence term.

### MLP's as probabilistic encoders and decoders
The paper listed two exemplary encoder-decoder pipelines based on relatively simple neural networks, namely
multi-layered perceptrons (MLPs). For a multivariate Bernoulli $\mathbf{x}$, the decoder maps $\mathbf{z}$ to parameter
$\mathbf{\lambda}$, and

$$
{\log p_{\theta}(\mathbf{x}\vert\mathbf{z})=\sum_{d=1}^D{\lambda_d}^{\mathbf{x}_d}(1-\lambda_d)^{1-\mathbf{x}_d}.}
$$

For a multivariate Gaussian $\mathbf{x}$, the decoder maps $\mathbf{z}$ to parameters $\mathbf{\mu}$ and
$\log\mathbf{\sigma}^2$, and

$$
{\log p_{\theta}(\mathbf{x}\vert\mathbf{z})=\log\mathcal{N}(\mathbf{x};\mathbf{\mu},\mathbf{\sigma}^2\mathbf{I}).}
$$

{% include note.html content="A most commonly encountered mistake committed by casual researchers on the VAE might be
their unawareness of using improper probabilities $\tilde{p_{\theta}}(\mathbf{x}\vert\mathbf{z})$. It could be a minor
mistake, but it could also cause serious consequences, as pinpointed and investigated by <a href='#references'>the
continuous Bernoulli paper</a> as \"fixing a pervasive error\". In short, the probabilities
$p_{\theta}(\mathbf{x}\vert\mathbf{z})$ must be a normalized one, which usually implies tractability. As a
counter-example, let's consider an incorrect model which directly outputs $\log p_{\theta}(\mathbf{x}\vert\mathbf{z})$
with an MLP. The problem is, we can not ensure this is a normalized probability due to the non-linearity of MLPs.
Fortunately, if we bypass this issue by modeling the parameters of a predefined distribution family, not the
probabilities, then the normalization requirement is automatically met." %}

## Future Work
The paper made several proposals for future research directions, such as:

* Learning hierarchical generative architectures with deep neural networks (e.g. convolutional networks) used for the
encoders and decoders, trained jointly with AEVB.
* Time-series models (i.e. dynamic Bayesian networks).
* Application of SGVB to the global parameters.
* Supervised models with latent variables, useful for learning complicated noise distributions.

## References
Diederik P. Kingma, Max Welling. 2014. Auto-Encoding Variational Bayes. In ICML 2014.

Gabriel Loaiza-Ganem, John P. Cunningham. 2019. The continuous Bernoulli: fixing a pervasive error in variational
autoencoders. In NeurIPS 2019.

Danilo Jimenez Rezende, Shakir Mohamed, and Daan Wierstra. 2014. Stochastic backpropagation and variational inference in
deep latent gaussian models. In ICML 2014.

{% include links.html %}
