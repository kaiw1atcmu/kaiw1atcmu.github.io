---
title: Formulas In Matrix Theory [more to come]
permalink: formulas_in_matrix_theory.html
date: 2019-12-09
tags: [mathematics,statistics]
published: false
---

Today we are discussing some equalities and inequalities in matrix theory.

## An Unnamed Matrix Inversion Identity
Direct matrix computation gives a simple (yet useful) matrix inversion identity
$$
    {(\mathbf{A}^{-1}+\mathbf{B}^{-1})^{-1}=\mathbf{A}^{-1}(\mathbf{A}+\mathbf{B})\mathbf{B}^{-1}.}
$$

## Woodbury Matrix Inversion Identity
$$
    {(\mathbf{A}+\mathbf{BCD})^{-1}=\mathbf{A}^{-1}-\mathbf{A}^{-1}\mathbf{B}(\mathbf{C}^{-1}+\mathbf{D}\mathbf{A}^{-1}\mathbf{B})^{-1}\mathbf{D}\mathbf{A}^{-1}.}
$$

## Sherman-Morrison-Woodbury Identity
And a simpler, special version is called Sherman-Morrison-Woodbury Identity:
$$
    {(\mathbf{M}+\mathbf{uv}^T)^{-1}=\mathbf{M}^{-1}-\frac{\mathbf{M}^{-1}\mathbf{uv}^T\mathbf{M}^{-1}}{1+\mathbf{v}^T\mathbf{M}^{-1}\mathbf{u}}.}
$$
If $\mathbf{u}=\mathbf{v}$, then
$$
    {(\mathbf{M}+\mathbf{vv}^T)^{-1}=\mathbf{M}^{-1}-\frac{\mathbf{M}^{-1}\mathbf{vv}^T\mathbf{M}^{-1}}{1+\mathbf{v}^T\mathbf{M}^{-1}\mathbf{v}}.}
$$

## Partitioned Matrix Inversion Identity
Suppose matrix $\mathbf{A}$ is partitioned into $2\times2$ submatrices
$\mathbf{A}_{11},\mathbf{A}_{12},\mathbf{A}_{21}$ and $\mathbf{A}_{22}$, then the inverse of matrix $\mathbf{A}$ is
given in partitioned form $\mathbf{M}$, by
$$
    {\mathbf{M}_{11}=(\mathbf{A}-\mathbf{B}\mathbf{D}^{-1}\mathbf{C})^{-1},\\
    \mathbf{M}_{12}=-(\mathbf{A}-\mathbf{B}\mathbf{D}^{-1}\mathbf{C})^{-1}\mathbf{B}\mathbf{D}^{-1},\\
    \mathbf{M}_{21}=-\mathbf{D}^{-1}\mathbf{C}(\mathbf{A}-\mathbf{B}\mathbf{D}^{-1}\mathbf{C})^{-1},\\
    \mathbf{M}_{22}=\mathbf{D}^{-1}+\mathbf{D}^{-1}\mathbf{C}(\mathbf{A}-\mathbf{B}\mathbf{D}^{-1}\mathbf{C})^{-1}\mathbf{B}\mathbf{D}^{-1}.}
$$

{% include links.html %}
