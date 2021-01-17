--- 
title: "Extending Jensen's Inequality"
keywords: jensen's inequality
date: 2019-12-09 08:35:22 -0800
last_updated: December 26, 2020
tags: [statistics,machine_learning]
summary: "Jensen's Inequality is a fundamental tool frequently used in optimization problems. Let's devote this post to
Jensen's Inequality and some variants of it."
sidebar: none
permalink: extending_jensens_inequality.html
folder: mydoc
---

Jensen's Inequality is a fundamental tool frequently used in optimization problems. Let's devote this post to Jensen's
Inequality and some variants of it.

## Vanilla Jensen's Inequality
For a convex function $f$ and a discrete variable $\mathbf{x}$, Jensen's Inequality takes the form

$$
  {f(\mathbb{E}[\mathbf{x}])\leq\mathbb{E}[f(\mathbf{x})].}
$$

For a continuous variable $\mathbf{x}$, Jensen's Inequality takes the form

$$
  {f\left(\int\mathbf{x}p(\mathbf{x})d\mathbf{x}\right)\leq\int f(\mathbf{x})p(\mathbf{x})d\mathbf{x}.}
$$

## Extended Jensen's Inequality
An extended version of Jensen's Inequality specifies that for a convex function $f$ and a bounded function $g$, we have

$$
  {f\left(\int p(\mathbf{x})g(\mathbf{x})d\mathbf{x}\right)\leq\int p(\mathbf{x})f(g(\mathbf{x}))d\mathbf{x}.}
$$

Interestingly, we applied this extended Jensen's Inequality more frequently than the "vanilla" one, even without being
aware of it. For example, in the KL divergence:

$$
  {D_{KL}(p\Vert q)=\int p(\mathbf{x})\log\frac{p(\mathbf{x})}{q(\mathbf{x})}d\mathbf{x}\geq 0.}
$$

As a succinct proof, let's introduce a variable $\mathbf{z}=g(\mathbf{x})$ and use the identity for change of variables,
$p_{\mathbf{x}}(\mathbf{x})d\mathbf{x}=p_{\mathbf{z}}(\mathbf{z})d\mathbf{z}$, then it follows

$$
  {\int p_{\mathbf{x}}(\mathbf{x})f(g(\mathbf{x}))d\mathbf{x}
  =\int p_{\mathbf{z}}(\mathbf{z})f(\mathbf{z})d\mathbf{z}\geq f\left(\int p_{\mathbf{z}}(\mathbf{z})d\mathbf{z}\right)
  =f\left(\mathbb{E}[\mathbf{z}])=f(\mathbb{E}[g(\mathbf{x})]\right)=f\left(\int p_{\mathbf{x}}(\mathbf{x})g(\mathbf{x})d\mathbf{x}\right).}
$$

## A Corollary
If function $f>0$, then

$$
  {\exp\int\ln f(\mathbf{x})d\mathbf{x}\le\int f(\mathbf{x})d\mathbf{x}\le\ln\int\exp f(\mathbf{x})d\mathbf{x}.}
$$

## References
Jichang Kuang. 2004. Applied Inequalities (3rd Ed.) Chapter 7.1.2. (In Simplified Chinese)

{% include links.html %}
