---
title: "A Property Of Continuous Attributes In Decision Trees"
keywords: continuous attribute, decision tree
date: 2020-01-16 18:25:32 -0800
last_updated: December 26, 2020
tags: [machine_learning]
summary: "This post discussed an important, yet seldom mentioned property of continuous attributes in decision trees."
sidebar: none
permalink: a_property_of_continuous_attributes_in_decision_trees.html
folder: mydoc
---

A variant of basic decision tree algorithms, <font face="Lora">C4.5</font> by [Quinlan (1993)](#references),
incorporates the ability to deal with continuous attribute values. For continuous attributes values, we would like to
pick a threshold $a_i$ that maximizes the Information Gain.

## The Property
The point we make is that we don't need to check the midway value between every pair of adjacent attribute values in the
training samples. [Fayyad (1991)](#references) showed that the value of $a_i$ that maximizes <font face="Lora">
Information Gain</font> must always lie at a "boundary", which means any value (<font face="Lora">not necessarily the
midway value</font>) between the attribute values of two adjacent samples that differ in their targets. Note that even
some textbooks (such as [Zhihua Zhou (2016)](#references)) didn't recognize this property, and went through the
procedure by iteratively checking every possible midway. Let us consider an example from
[Tom Mitchell (1997)](#references) for clarification.

```
 attribute: temperature | 40 | 48 | 60  | 72  | 80  | 90 |
 target: play tennis    | no | no | yes | yes | yes | no |
```
<center><font face="Lora">Table 1: Finding The Best Threshold For Learning Target From Attribute</font></center><br/>

In the current example, there are two candidate thresholds (boundaries), corresponding to the values of temperature at
which the value of play tennis changes: $(48+60)/2$, and $(80+90)/2$. Note that although a boundary is valid anywhere
between the pair of adjacent attributes whose targets differ and makes no difference in training, the midway value is
more often considered as a potential choice for better generalization. Let us show a concise proof of this property.

## The Proof
Suppose we partition all samples at a node by a given threshold midway between two adjacent attribute values. Its left
child consists of $n_{11}$ positive samples and $n_{12}$ negative samples, and its right child consists of $n_{21}$
positive samples and $n_{22}$ negative samples, respectively. We want to find a splitting threshold which yields the
maximum Information Gain, or equivalently, the minimum conditional entropy. Denoting the entropy of the Bernoulli
distribution $\text{bin}(p,1-p)$ by $H(p)$, the conditional entropy corresponding to this splitting threshold is
formulated as

$$
  {\frac{n_{11}\!+\!n_{12}}{n}H(\frac{n_{11}}{n_{11}\!+\!n_{12}})\!+\!\frac{n_{21}\!+\!n_{22}}{n}H(\frac{n_{21}}{n_{21}\!+\!n_{22}}),}
$$

where $n=n_{11}+n_{12}+n_{21}+n_{22}$. Without losing generality, suppose the splitting threshold coincides with a
boundary, such that its closest left neighbor is a positive sample, and its closest right neighbors are exactly $m$
negative samples in a successive manner:

$$
  {\ldots\quad\text{negative}\quad\Vert\quad\text{positive 1}\quad\text{positive 2}\quad\ldots\quad\text{positive m}\quad\Vert\quad\text{negative}\quad\ldots.}
$$

Consider a function $f(x)$ which calculates the conditional entropy if we move the splitting threshold by $x$ samples to
the right, then

$$
  {f(x)=\frac{n_{11}\!+\!n_{12}\!+\!x}{n}H(\frac{n_{11}}{n_{11}\!+\!n_{12}\!+\!x})+\frac{n_{21}\!+\!n_{22}\!-\!x}{n}H(\frac{n_{21}}{n_{21}\!+\!n_{22}\!-\!x}).}
$$

With a little bit maths we arrive at

$$
  {f'(x)=\frac{1}{n}\left(\ln\frac{n_{11}\!+\!n_{12}\!+\!x}{n_{12}\!+\!x}\!-\!\ln\frac{n_{21}\!+\!n_{22}\!-\!x}{n_{22}\!-\!x}\right).}
$$

It is easy and straight-forward to check that $f'(x),x\in(-n_{12},n_{22})$ is monotonically increasing, and that
$\lim_{x\to-n_{12}^+}f'(x)\to\infty$, $\lim_{x\to n_{22}^-}f'(x)\to-\infty$, which means $f(x)$ is a concave function
attaining its maximum at $x\in(-n_{12},n_{22})$ where

$$
  {\frac{n_{11}\!+\!n_{12}\!+\!x}{n_{12}\!+\!x}=\frac{n_{21}\!+\!n_{22}\!-\!x}{n_{22}\!-\!x}\ (=\!\frac{n}{n_{12}\!+\!n_{22}}),\quad \text{that is},
  \quad x\!=\!\frac{n_{11}n_{22}\!-\!n_{12}n_{21}}{n_{11}\!+\!n_{21}}.}
$$

Note that $x$ may be non-integer or even negative. But wherever $x$ occurs, it's obvious by visualizing with a simple
graph that $f(x)$ can only attain its minimum at boundaries of this $m$-sample interval $x\in[0,m]$, which completes the
proof.

{% include note.html content="Remember something special about continuous attributes. Being different from discrete
attributes, the continuous attributes can be reused for subsequent splittings in its descendant nodes." %}

## References
Ross Quinlan. 1993. C4.5: programs for machine learning.

Usama M. Fayyad. 1991. On the Handling of Continuous-Valued Attributes in Decision Tree Generation. 

Zhihua Zhou. 2016. Machine Learning. Chapter 4. (In Simplified Chinese)

Tom M. Mitchell. 1997. Machine Learning. Chapter 3.

{% include links.html %}
