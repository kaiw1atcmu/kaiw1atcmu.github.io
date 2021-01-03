---
title: Formulas In Statistics [more to come]
permalink: formulas_in_statistics.html
date: 2019-12-09
tags: [statistics,mathematics]
published: false
---

Today we are discussing some equalities and inequalities in statistics.

## Jensen's Inequality
For a convex function $f$ and a discrete variable $\mathbf{x}$, we have
$f(\mathbb{E}[\mathbf{x}])\leq\mathbb{E}[f(\mathbf{x})]$. For a continuous variable $\mathbf{x}$, Jensen's inequality
takes the form
$$
    {f\left(\int\mathbf{x}p(\mathbf{x})d\mathbf{x}\right)\leq\int f(\mathbf{x})p(\mathbf{x})d\mathbf{x}.}
$$

## Extended Jensen's Inequality
An extended version of Jensen's inequality specifies that for a convex function $f$ and a bounded function $g$, we have
$$
    {f\left(\int p(\mathbf{x})g(\mathbf{x})d\mathbf{x}\right)\leq\int p(\mathbf{x})f(g(\mathbf{x}))d\mathbf{x}.}
$$
See the [Chinese literature on inequalities](#references) for details on its proof.

## References
匡继昌. 2004. 常用不等式. p356 (Jensen不等式).

{% include links.html %}
