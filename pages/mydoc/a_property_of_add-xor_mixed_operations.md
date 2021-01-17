---
title: "A Property Of Add-Xor Mixed Operations"
keywords: add-xor mixed operations
date: 2020-01-20 22:32:34 -0800
last_updated: December 26, 2020
tags: [hacker's_delight,algorithms]
summary: "In this post, we will introduce a relatively infrequently mentioned property between the addition and
exclusive-or operators, when they are used in a mixed manner."
sidebar: none
permalink: a_property_of_add-xor_mixed_operations.html
folder: mydoc
---

In this post, we will introduce a relatively infrequently mentioned property between the addition and exclusive-or
operators, when they are used in a mixed manner.

## Problem Formulation
Given two arbitrary $k$-bit unsigned integer arrays $\mathbf{a}$ and $\mathbf{b}$ of lengths $m$ and $n$, respectively,
we wish to determine the xor result of the sums of the $m\times n$ unsigned integer pairs

$$
  {\bigoplus_{1\leq i\leq m,1\leq j\leq n}(a_i\!+\!b_j).}
$$

That is, xoring the sums of all possible unsigned integer pairs, formed by picking one unsigned integer from array
$\mathbf{a}$ and the other from array $\mathbf{b}$.

## The Algorithm in Scalar Form
In the first place, we should abandon the naive approach to this problem, that is, the brute-force for-loop methods.
This approach apparently requires a time complexity of $O(mn)$ and a (constant) space complexity of $O(1)$. What is
different here is that the xor operator completely discards carry information, whereas the add operator preserves and
propagates this information from the LSB through the MSB, and may produce a carry to the next bit higher than the MSB.
Let $c_{ijk}$ denote the carry into the $k$-th bit, that is, the carry produced from the $(k-1)$-th bit. The operations
of computing carry bits should proceed sequentially, from the LSB to the MSB, like this:

$$
  {c_{ij1}=0,}\\
  {c_{ij2}=a_{i1}\!\cap\! b_{j1},}\\
  {c_{ij3}=a_{i2}\!\cap\! b_{j2}\ \cup\ a_{i2}\!\cap\! c_{ij2}\ \cup\ b_{j2}\!\cap\! c_{ij2}
  =a_{i2}\!\cap\! b_{j2}\ \cup\ a_{i2}\!\cap\! a_{i1}\!\cap\! b_{j1}\ \cup\ b_{j2}\!\cap\! a_{i1}\!\cap\! b_{j1},}\\
  {\ldots,}
$$

in which we suppose disjunctives take precedence over conjunctives and exclusive-ors for notational convenience, to
prevent the equations from being cluttered. The difficulty arises in the fact that we use a mixture of $\oplus$ and
$\cup$ operators, which could not be further simplified. However, we can proof, by mathematical induction, a useful
property that $\cup$ can be superseded by $\oplus$ without affecting the result in the expressions for $c_{ijk}$. With
this property, we rewrite the aforementioned recurrences:

$$
  {c_{ij1}=0,}\\
  {c_{ij2}=a_{i1}\!\cap\! b_{j1},}\\
  {c_{ij3}=a_{i2}\!\cap\! b_{j2}\ \cup\ a_{i2}\!\cap\! a_{i1}\!\cap\! b_{j1}\ \cup\ b_{j2}\!\cap\! a_{i1}\!\cap\! b_{j1}
  =a_{i2}\!\cap\! b_{j2}\ \oplus\ a_{i2}\!\cap\! a_{i1}\!\cap\! b_{j1}\ \oplus\ b_{j2}\!\cap\! a_{i1}\!\cap\! b_{j1},}\\
  {\ldots,}
$$

until we arrive at $c_{ij(k+1)}$, the carry into the next bit higher than the MSB. This recursive procedure suggests
creating a tree-shaped memory hierarchy to keep track of products such as $a_{i1}a_{i2}$. Concretely, the tree is
constructed for the $i$-th integer $a_i$ in $\mathbf{a}$ and the $j$-th integer $b_j$ in $\mathbf{b}$, like this,

<center>
    <img src="{{ "images/20200120-1.png" }}" alt="tree for a_i"/>
    <I>Figure 1: tree for</I> $a_i$
</center>

<center>
    <img src="{{ "images/20200120-2.png" }}" alt="tree for b_j"/>
    <I>Figure 2: tree for</I> $b_j$
</center>

(note that $\text{tree}\_\text{b}$ is created like $\text{tree}\_\text{a}$, plus a series of iterative and recursive
node swappings, as illustrated), where $a_{ik}$ is the $k$-th digit of integer $a_i$,  $b_{jk}$ is the $k$-th digit of
integer $b_j$, and $c_{ijk}$ is the xor of products of the corresponding nodes in the $k$-th layer of both trees, except
for the last nodes which always equal $1$, like this:

$$
  {c_{ijk}=\bigoplus_{d=1}^{2^{k-1}-1}\text{tree}\_\text{a}_{i(k-1),d}\cdot\text{tree}\_\text{b}_{j(k-1),d}.}
$$

## The Algorithm in Vector Form
<center>
    <img src="{{ "images/20200120-3.png" }}" alt="tree for a_i vector"/>
    <I>Figure 3: tree for</I> $\mathbf{a}$
</center>

<center>
    <img src="{{ "images/20200120-4.png" }}" alt="tree for b_j vector"/>
    <I>Figure 4: tree for</I> $\mathbf{b}$
</center>

This formula extends naturally to vectors $\mathbf{a}_k\in\mathbf{R}^m$ and $\mathbf{b}_k\in\mathbf{R}^n$, denoting the
vectors for $\mathbf{a}$'s $k$-th digits and $\mathbf{b}$'s $k$-th digits, respectively, where $\mathbf{c}_k$
$\in\mathbf{R}^{m\times n}$, the matrix of the $k$-th carries, is given by

$$
  {\mathbf{c}_k=\bigoplus_{d=1}^{2^{k-1}-1}\mathbf{tree\_a}_{(k-1),d}\otimes\mathbf{tree\_b}_{(k-1),d}.}
$$

As with the formula for vectors, the tree-shaped memory hierarchy also extends to hold element-wise vector products such
as $\mathbf{a}_1\odot\mathbf{a}_2$. Note that $\mathbf{tree\_b}$ is created like $\mathbf{tree\_a}$, plus a series of
iterative and recursive node swappings, as illustrated. The matrix of the $k$-th digits in the sum, $\mathbf{s}$, is
given by

$$
  {\mathbf{s}_k=(\mathbf{a}_k\otimes\mathbf{1}_n\oplus\mathbf{1}_m\otimes\mathbf{b}_k\oplus\mathbf{c}_k)\ \text{mod}\ 2}
$$

and the $k$-th digit of the required result is
    
$$
  {\text{sum}(\mathbf{s}_k)\ \text{mod}\ 2=\text{sum}(\mathbf{a}_k)\cdot n\ \text{mod}\ 2\oplus
  \text{sum}(\mathbf{b}_k)\cdot m\ \text{mod}\ 2\oplus
  \text{sum}(\mathbf{c}_k)\ \text{mod}\ 2}\\
  {=\text{sum}(\mathbf{a}_k)\cdot n\ \text{mod}\ 2\oplus\text{sum}(\mathbf{b}_k)\cdot m\ \text{mod}\ 2
  \oplus\bigoplus_{d=1}^{2^{k-1}-1}\text{sum}(\mathbf{tree\_a}_{(k-1),d})\cdot\text{sum}(\mathbf{tree\_b}_{(k-1),d})\ \text{mod}\ 2,}
$$

and do remember to include that for the $(k+1)$-th digit

$$
  {\text{sum}(\mathbf{s}_{k+1})\ \text{mod}\ 2=\bigoplus_{d=1}^{2^k-1}\text{sum}(\mathbf{tree\_a}_{k,d})
  \cdot\text{sum}(\mathbf{tree\_b}_{k,d})\ \text{mod}\ 2.}
$$

This is because $\mathbf{a_{k+1}}=\mathbf{0}$ and $\mathbf{b_{k+1}}=\mathbf{0}$. The required result is finally given by
(in binary form)

$$
  {s=[\underbrace{\text{sum}(\mathbf{s}_{k+1})\ \text{mod}\ 2}_{(k+1)\text{-th digit}}
  \ \underbrace{\text{sum}(\mathbf{s}_k)\ \text{mod}\ 2}_{k\text{-th digit}}\ \ldots
  \ \underbrace{\text{sum}(\mathbf{s}_1)\ \text{mod}\ 2}_{1\text{st digit}}].}
$$

## Complexity and Analysis
This algorithm demands a time complexity $O(2^d(m+n))$ and a space complexity $O(2^d(m+n))$. To make a fair comparison
at the bit level, the brute-force for-loop algorithm demands a time complexity of $O(dmn)$ and a space complexity of
$O(d)$. We can make a bold assumption that $d$ is constant, and then the algorithmic gain is roughly between
$O(\min(m,n))$ and $O(\max(m,n))$, i.e., an improvement by the magnitude of the array lengths. This is indeed a
significant improvement over the naive, straight-forward solution.

## Simulation with Python
We write a Python 3.0 programs to implement this recursive algorithm.

```python
import numpy as np

# define tree node class
class Node(object):
    def __init__(self, v):
        self.v = v
        self.l = None
        self.r = None

# create a vector tree with depth k
def tree(nd, arr, cl):
    if cl >= arr.shape[1]:
        return
    nd.l = Node(np.multiply(nd.v, arr[:, cl]))
    nd.r = Node(nd.v)
    tree(nd.l, arr, cl+1)
    tree(nd.r, arr, cl+1)

# traverse layer-wise down the trees
def compute(ta, tb):
    res = [(sum(A[:, 0]) * n + sum(B[:, 0]) * m) % 2]
    la = [ta.l, ta.r]
    lb = [tb.l, tb.r]
    cl = 0
    while len(la) > 0:
        cl += 1
        tmp_res = 0
        tmp_la, tmp_lb = [], []
        for na, nb in zip(la, lb):
            tmp_res += sum(na.v) * sum(nb.v)
            if na.l is not None:
                tmp_la.extend([na.l, na.r])
                tmp_lb.extend([nb.l, nb.r])
        la, lb = tmp_la, tmp_lb
        tmp_res -= m * n    # minus product of the last all-1 nodes
        # cl == k corresponds to carry into the (k + 1)-th digit
        if cl < k:
            tmp_res += sum(A[:, cl]) * n + sum(B[:, cl]) * m
        res.append(tmp_res % 2)
    return list(reversed(res))
 
# helper for tree nodes swapping
def swap(nd):
    if nd.l is None and nd.r is None:
        return
    nd.l, nd.r = nd.r, nd.l
    swap(nd.l)
    swap(nd.r)

# helper for tree nodes swapping
def treeswap(nd):
    while nd.l is not None:
        swap(nd.l)
        nd = nd.r

# helper for validation
def validate(va, vb):
    res = 0
    for i in va:
        for j in vb:
            res ^= i + j
    res = bin(res)[2:]
    res = '0' * (k + 1 - len(res)) + res
    return list(map(int, res))

# test case
if __name__ == '__main__':
    k = 6  # number of digits
    m = 5  # length of a
    n = 6  # length of b
    # randomly generate a and b
    a = np.random.randint(0, 2 ** k, (m,), dtype=np.int8)
    b = np.random.randint(0, 2 ** k, (n,), dtype=np.int8)
    A = np.zeros((m, k), dtype=np.int8)
    B = np.zeros((n, k), dtype=np.int8)
    ma = np.array([1] * m, dtype=np.int8)
    mb = np.array([1] * n, dtype=np.int8)
    for i in range(k):
        A[:, i] = np.bitwise_and(a, ma) >> i
        B[:, i] = np.bitwise_and(b, mb) >> i
        ma <<= 1
        mb <<= 1
    ta = Node(np.array([1] * m, dtype=np.int8))
    tb = Node(np.array([1] * n, dtype=np.int8))
    tree(ta, A, 0)
    tree(tb, B, 0)
    treeswap(tb)
    # check that it should generate identical results with the naive algorithm
    print(compute(ta, tb))
    print(validate(a, b))
```

The Python 3.0 program yields the sums computed with both algorithms so that we can compare them easily:

```python
[0, 1, 0, 1, 1, 0, 0]
[0, 1, 0, 1, 1, 0, 0]
```

{% include links.html %}
