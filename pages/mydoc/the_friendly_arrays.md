---
title: "The Friendly Arrays [more to come]"
keywords: friendly array
date: 2020-01-22 22:14:35 -0800
last_updated: December 26, 2020
tags: [algorithms,hackers_delight]
summary: "Today we are discussing a very interesting algorithmic problem called the friendly arrays."
sidebar: mydoc_sidebar
permalink: the_friendly_arrays.html
folder: mydoc
---

Today we are discussing a very interesting algorithmic problem called the friendly arrays.

## Problem Formulation
Informally, we define the $k$-th friendly array to be one integer array of length $2k$, in which each integer lies in
the range $[1,k]$ and appears exactly twice, in an manner that each pair of $i$'s are spaced by $i$ integers,
$i\in[1,k]$. Let us enumerate the first few friendly arrays, for example:

| the ordinal | the n-th friendly arrays |
| :----: | :----: |
| the 1-st | (none) |
| the 2-nd | (none) |
| the 3-rd | (3, 1, 2, 1, 3, 2), (2, 3, 1, 2, 1, 3) |
| the 4-th | (2, 3, 4, 2, 1, 3, 1, 4), (4, 1, 3, 1, 2, 4, 3, 2) |
| the 5-th | (none) |
| the 6-th | (none) |
| the 7-th | (4, 6, 1, 7, 1, 4, 3, 5, 6, 2, 3, 7, 2, 5), and 51 more... |
| the 8-th | (6, 2, 8, 5, 2, 4, 7, 6, 3, 5, 4, 8, 3, 1, 7, 1), and 299 more... |

_Table 1: The Friendly Arrays For The First Few Ordinals_
    
Then what is the systematic procedure to generate (preferably in an exhaustive manner) such $k$-th friendly arrays?

{% include links.html %}
