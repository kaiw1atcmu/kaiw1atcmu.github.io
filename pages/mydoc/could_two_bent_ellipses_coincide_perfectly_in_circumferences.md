---
title: "Could Two Bent Ellipses Coincide Perfectly in Circumferences?"
keywords: 3D geometry
date: 2019-11-26 14:23:38 -0800
last_updated: December 26, 2020
tags: [mathematics]
summary: "Could two ellipses coincide perfectly in circumferences when bent along the major axis? In this post, let us
try to approach this problem in a step-by-step, mathematically rigid manner."
sidebar: none
permalink: could_two_bent_ellipses_coincide_perfectly_in_circumferences.html
folder: mydoc
---

Recently [a fairly interesting mathematical problem](https://www.zhihu.com/question/333858081) concerning differential
geometry is raised by maths geeks in *Zhihu (知乎)*, a.k.a. the Chinese *Quora*. The problem goes plainly enough to be
accessible to the general public (i.e., people outside of the mathematical community): can two ellipses perfectly
coincide in circumference when bent along the major axis?

<center>
    <img src="{{ "images/20191126-1.jpg" }}" alt="image1 simulation"/>
    <font face="Lora">Figure 1: Simulation. By courtesy of users from Zhihu (知乎).</font>
</center>

## At First Glance
At first glance, we should recourse to differential geometry to approach this problem. Even worse, the elliptic
integrals are generally intractable where a closed-form solution is desirable. However, a natural way to re-parameterize
its bent circumference is to use arc length $s$. By careful analysis, I think the answer is simply "yes". And there is
only one way to bend the ellipses.

<center>
    <img src="{{ "images/20191126-2.jpg" }}" alt="image2"/>
    <font face="Lora">Figure 2: Coordinates of bent ellipses re-parameterized by arc length</font> $s$.
</center>

## An Assumption on $dx(s)/ds$
Let us consider only quarter ellipses. Suppose the circumference of one bent ellipse is re-parameterized by
$(x,y,z)=(x(s),y(s),z(s)),\ s\in[0,l]$. As illustrated by Figure 2, if $P=(x(s),y(s),z(s))$, then it follows
$z(s)=y(l-s)$ since in the other ellipse, $P$ corresponds to arc length $l-s$ and $z(s)$ is the height in that ellipse.
Now it is clear that $y(s)$ and $z(s)$ are fixed functions of $s$ irrespective of how we choose $x(s)$. This fixedness
does not require "symmetry" between the two ellipses, although the latter is a natural consequence of the former. And
obviously, both $y(s)$ and $z(s)$ are monotonically increasing. We also make an assumption that $x(s)$ is either
monotonically increasing or monotonically decreasing, i.e., precluding the scenario of "warping back and forth".

Recall the basic differential equality concerning arc length in the 3-dimensional space:

$$
  {(\frac{dx(s)}{ds})^2+(\frac{dy(s)}{ds})^2+(\frac{dz(s)}{ds})^2=1,}
$$

Substituting $l-s$ for $s$:

$$
  {(\frac{dx(l-s)}{ds})^2+(\frac{dy(l-s)}{ds})^2+(\frac{dz(l-s)}{ds})^2=1.}
$$

From $z(s)=y(l-s)$ it directly follows that $dx(l-s)/ds=\pm dx(s)/ds$, and by the assumption on $x(s)$ we have
$dx(l-s)/ds=dx(s)/ds$. Integrating on the interval $[0,s]$ gives:

$$
  {\int_0^s\frac{dx(s)}{ds}ds=\int_0^s\frac{dx(l-s)}{ds}ds,}\\
  {x(s)=x(l)-x(l-s),\ s\in[0,l].}
$$

With this equality on $x(s)$, it follows immediately that the ellipses are symmetrically positioned. In fact, by turning
around the other bent ellipse according to symmetry, its circumference becomes re-parameterized with $s$ by

$$
  {(\bar{x}(s),\bar{y}(s),\bar{z}(s))=(x(l)-x(l-s),z(l-s),y(l-s)),\ s\in[0,l],}
$$

and we are ready to find that

$$
  {(x(s),y(s),z(s))=(x(l)-x(l-s),z(l-s),y(l-s)),\ s\in[0,l].}
$$

That is, the circumferences of two ellipses coincide perfectly. **Therefore, the symmetry property between the two
ellipses' positioning is automatically satisfied!**

But is there something missing, since we have not so far dived into the essential property of an ellipse? Indeed, it is
tempting to neglect the verification that the assignment of $x(s)$ is realizable. Denote the projection of arc $s_2-s_1$
onto plane $\mathcal{XOZ}$ by $t(s_2)-t(s_1)$, this is equivalent to showing that

$$
  {z(s_1)-z(s_2)\leq t(s_2)-t(s_1),\ s_1,s_2\in[0,l],\ s_1\lt s_2.}
$$

That is,

$$
  {\int_{s_1}^{s_2}\frac{1}{\sqrt{1+(dy/dx)^2}}ds\geq\int_{l-s_2}^{l-s_1}\frac{1}{\sqrt{1+(dx/dy)^2}}ds,\ s_1,s_2\in[0,l],\ s_1\lt s_2.}
$$

which is in turn equivalent to showing that

$$
  {\vert\frac{dy}{dx}\vert\bigg\vert_{s=s_2}\leq\vert\frac{dx}{dy}\vert\bigg\vert_{s=s_2}.}
$$

In an ellipse parameterized by polar coordinate $\theta$, $x=acos\theta$ and $y=bsin\theta$. It is easy to verify

$$
  {\vert\frac{dy}{dx}\vert\bigg\vert_{\theta}=\vert\frac{dx}{dy}\vert\bigg\vert_{\frac{\pi}{2}-\theta}.}
$$

And the arc length from $0$ to $\theta$ is given by

$$
  {\int_0^{\theta}\sqrt{(dx/ds)^2+(dy/ds)^2}ds=\int_0^{\theta}\sqrt{a^2-(a^2-b^2)cos^2\theta}ds,}
$$

which is greater than the arc length from $\frac{\pi}{2}-\theta$ to $\frac{\pi}{2}$ given by

$$
  {\int_{\frac{\pi}{2}-\theta}^{\frac{\pi}{2}}\sqrt{(dx/ds)^2+(dy/ds)^2}ds=\int_{\frac{\pi}{2}-\theta}^{\frac{\pi}{2}}\sqrt{a^2-(a^2-b^2)cos^2\theta}ds.}
$$

When arc lengths are equal, the corresponding $\theta$ has the relation $\theta_1\gt\frac{\pi}{2}-\theta_2$, by the
monotonic decreasing property of $\vert dy/dx\vert$ on an ellipse, we have

$$
  {\vert\frac{dy}{dx}\vert\bigg\vert_{s=s_2}\leq\vert\frac{dx}{dy}\vert\bigg\vert_{s=s_2}.}
$$

which is exactly the desired inequality. **This is also instrumental in explaining why two ellipses can perfectly
coincide in circumference when bent along the major axis, not along the minor axis!**

<center>
    <img src="{{ "images/20191126-3.jpg" }}" alt="image3"/>
    <font face="Lora">Figure 3: Condition under which bending towards the origin along the opposite direction of</font> $\mathcal{Z}$ <I>is feasible.</I>
</center>

## How Do We Render It?
This proof actually shows a constructive way of rendering this bending. Since $y(s)$ is fixed, we can position an end of
an ellipse's major axis at coordinate $(0,0,z(l))=(0,0,b)$, and then bend the ellipse according to $z(s)$ (also fixed)
towards the origin (the opposite direction of $\mathcal{Z}$). In this way we have constructed an implicit function
$y=y(z)$. Since we have verified the inequality
$z(s_1)-z(s_2)\leq \text{projection of arc $s_2-s_1$ onto plane $\mathcal{XOZ}$},\ 0\leq s_1\lt s_2 \leq l$ holds, this
bending is always feasible. By doing this successively from $s=0$ to $s=l$, we are finished bending one ellipse. The
other ellipse is bent identically and positioned symmetrically. As we have already verified, the two circumferences
coincide.

<center>
    <img src="{{ "images/20191126-4.jpg" }}" alt="image4"/>
    <font face="Lora">Figure 4: Illustration of how bending towards the origin along the opposite direction of</font> $\mathcal{Z}$ <I>is performed.</I>
</center>

## Without the Assumption on $dx(s)/ds$
Without the assumption on $x(s)$, we can at best make a conclusion that $dx(l-s)/ds=\pm dx(s)/ds$, an equality of little
avail. Following the same line of argumentation, it is easy to show that

$$
  {y(s_2)-y(s_1)\leq \text{projection of arc $s_2-s_1$ onto plane $\mathcal{XOY}$},\ 0\leq s_1\lt s_2 \leq l}
$$

holds true, i.e., bending the other ellipse is always feasible. Thus, we have arrived at two procedures which dictates
how the bending of both ellipses are rendered. But we need to show the two bent ellipses can be joined on the shared
circumferences. Unfortunately, this is impossible if the assumption on $x(s)$ is violated. That is to show the ellipses
intercept each other, or equivalently, besides the circumferences they shared at least on point in common.

The two ellipses are

$$
  {(x(s),y(s)\!\cdot\!\alpha,z(s)),\ s\!\in\![0,l],\ \alpha\!\in\![0,1],}\\
  {(x(s'),y(s'),z(s')\!\cdot\!\beta),\ s\!\in\![0,l],\ \beta\!\in\![0,1],}  
$$

respectively. Note the circumference correspond to $\alpha=1$ and $\beta=1$. If the ellipses share at least one point
besides the circumference, we must have

$$
  {x(s)=x(s'),\ y(s)\!\cdot\!\alpha=y(s'),\ z(s)=z(s')\!\cdot\!\beta,\ s\!\in\![0,l],\ \alpha\!\lt\!1\text{ or }\beta\!\lt\!1,}
$$

that is,

$$
  {x(s)=x(s'),\ y(s)\!\cdot\!\alpha=y(s'),\ y(l-s)=y(l-s')\!\cdot\!\beta,\ s\!\in\![0,l],\ \alpha\!\lt\!1\text{ or }\beta\!\lt\!1.}
$$

$y(s)\cdot\alpha=y(s'),\ \alpha\in[0,1]$ is equivalent to $y(s)\geq y(s')$, and
$y(l-s)=y(l-s')\cdot\beta,\ \beta\in[0,1]$ is equivalent to $y(l-s)\leq y(l-s')$. Recall $y(s)$ is fixed and
monotonically increasing, then the condition $\alpha<1\text{ or }\beta<1$ is equivalent to saying that $s>s'$ or
$l-s<l-s'$, i.e., $s>s'$. Then the ellipses intercept if in addition, there exist $s,s'$ such that $x(s)=x(s'),\ s>s'$.
It happens if and only if the foregoing assumption on $x(s)$, that $x(s)$ is either monotonically increasing or
monotonically decreasing, is violated. Whenever it happens, although the ellipses share circumferences, they intercept
into each other and thus can not make a solution to the problem.

<center>
    <img src="{{ "images/20191126-5.jpg" }}" alt="image5"/>
    <font face="Lora">Figure 5: Illustration of</font> $x(s)$ <I>"warping around".</I>
</center>

## Concluding Remarks
### The Need For Positive Derivative Assumption
Now let us review what we have done. If the assumption specifying the positiveness of $ds(x)/ds$ is violated, then
**virtually any circumferences** can be made for both ellipses to share it. However, if the assumption is violated, then
such a solution is actually infeasible since the ellipses intercept into each other.

### The Reason Not To Bend Along Minor Axis
Furthermore, the pivot of this proof is to pinpoint the condition below, and this is exactly why bending along the minor
axis doesn't work!

$$
  {z(s_2)-z(s_1)\leq \text{projection of arc $s_2-s_1$ onto plane $\mathcal{XOZ}$},\ 0\leq s_2\lt s_1 \leq l,}
$$

### The Volume Surrounded By The Ellipses 
It is not cumbersome to show that the volume enclosed by the two bent ellipses is given by

$$
  {4\int_0^ly(s)y(l-s)\sqrt{1-(\frac{dy(s)}{ds})^2-(\frac{dy(l-s)}{ds})^2}ds}
$$

### Open Questions
In addition, there may as well be other planar shapes besides ellipses satisfying the condition

$$
  {z(s_1)-z(s_2)\leq \text{projection of arc $s_2-s_1$ onto plane $\mathcal{XOZ}$},\ 0\leq s_1\lt s_2 \leq l.}
$$

Of them the diamond $\vert x\vert/a+\vert y\vert/b=1$ is obviously an example. It remains to show if other planar shapes
in forms of $(\vert x\vert/a)^p+(\vert y\vert/b)^p=1,\ p>0$ exist that can be bent along a direction to coincide
perfectly in circumference?

## Acknowledgements
Some insightful discussions following the original post provided clues to re-parameterizing the bent ellipse by arc
length $s$, which indeed gave us a nice and neat solution. For better visualization, I also reproduced and annotated
some images from the post discussions. Thanks for all of them.

{% include links.html %}
