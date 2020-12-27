---
title: "The Defective Calculator"
keywords: defective calculator, broken calculator
date: 2020-01-06 04:04:23 -0800
last_updated: December 26, 2020
tags: [hackers_delight,algorithm]
summary: "Suppose you are provided with a defective calculator, whose numerical and functional keys are all out of order
except. Then what should you do if you get asked to produce a certain result with this calculator?"
sidebar: mydoc_sidebar
permalink: the_defective_calculator.html
folder: mydoc
---

Suppose you are provided with a defective calculator, whose numerical and functional keys are all out of order except
$\tan$, $\arctan$, $\sin$, $\arcsin$, $\cos$ and $\arccos$. In addition, you have an initial $0$ on the screen when the
calculator is powered on. The question is: starting with the initial $0$, how can you return $\frac{113}{355}$ (actually
any positive rational number) with a finite sequence of operations allowed on this calculator? The situation here is not
ungrounded. As heavy calculator users (accountants, stockbrokers, cashiers, etc.) use the trigonometric function keys
relatively infrequently, so their mean time to failure should be longer in response. (An interesting history of the
fractional number $\frac{355}{113}$ dates back to the 5th century A.D. when the famous Chinese mathematician 祖冲之
approximated $\pi$ with it.)

<img src="{{ "images/20200116-1.png" }}" alt="a profile of 祖冲之"/>
_Figure 1: A profile of the famous Chinese mathematician 祖冲之 who appeared on a Chinese postal stamp._

## Function Compositions
To kickoff, let us make clear the domains and ranges of frequently used inverse trigonometric functions for our cases:

$$
\begin{array}{l}
  {\arcsin:\ [0,1]\mapsto[0,\pi/2],}\\
  {\arccos:\ [0,1]\mapsto[0,\pi/2],}\\
  {\arctan:\ [0,\infty)\mapsto[0,\pi/2).}
\end{array}
$$

Let $\text{funcname}^k(x)$ denote the $k$-folded composition of function $\text{funcname}$, not the $k$-th order
derivative of it. Bear in mind some useful recurrences, which might be instrumental in solving our problem:

$$
\begin{array}{l}
  {(\sin\arctan)^k(x)=\text{sqrt}\left(\frac{\displaystyle x^2}{\displaystyle 1+kx^2}\right),\ \forall k\geq0,\ 0\leq x<\infty,}\\
  {(\tan\arcsin)^k(x)=\text{sqrt}\left(\frac{\displaystyle x^2}{\displaystyle 1-kx^2}\right),\ \forall k\geq0,\ 0\leq kx^2<1\ \text{for each run of iteration},}\\
  {(\cos\arctan)^k(x)=\text{sqrt}\left(\frac{\displaystyle f_k+f_{k-1}x^2}{\displaystyle f_{k+1}+f_kx^2}\right),\ \forall k\geq1,\ 0\leq x<\infty,}\\
  {(\tan\arccos)^k(x)=\text{sqrt}\left(\frac{\displaystyle f_k-f_{k+1}x^2}{\displaystyle f_kx^2-f_{k-1}}\right),\ \forall k\geq1,\ 0< x\leq1\ \text{for each run of iteration},}
\end{array}
$$

where $f_k$ denotes the $k$-th **Fibonacci number** starting from $f_0=0$. Make use of these identities to compute
$\frac{1}{x}$, the reciprocal of $x$, for $x\in(0,\infty)$:

$$
\begin{array}{l}
  {\tan\arcsin\cos\arctan(x)=\frac{\displaystyle 1}{\displaystyle x},}\\
  {\tan\arccos\sin\arctan(x)=\frac{\displaystyle 1}{\displaystyle x}.}
\end{array}
$$

In addition, recall the basic $\sin\sim\cos$ relations for $x\in[0,1]$:

$$
\begin{array}{l}
  {\sin\arccos(x)=\text{sqrt}\left(1-x^2\right),}\\
  {\cos\arcsin(x)=\text{sqrt}\left(1-x^2\right).}
\end{array}
$$

## Generating Procedures
We can generate any rational numbers by calling $\sin\arccos(0)=1$ once, and calling two kinds of function compositions
mentioned above, perhaps multiple times:

$$
\begin{array}{l}
  {(\sin\arctan)^k(x)=\text{sqrt}\left(\frac{\displaystyle x^2}{\displaystyle 1+kx^2}\right),\ \forall k\geq0,\ 0\leq x<\infty,}\\
  {\tan\arcsin\cos\arctan(x)=\frac{\displaystyle 1}{\displaystyle x},\ \text{or}\ \tan\arccos\sin\arctan(x)=\frac{\displaystyle 1}{\displaystyle x}.}
\end{array}
$$

With these identities, we start with an initial $0$ when the calculator is powered on,

$$
\begin{array}{l}
  {\sin\arccos(0)=1,}\\
  {(\sin\arctan)^{n-1}(1)=\text{sqrt}\left(\frac{\displaystyle 1}{\displaystyle 1+n-1}\right)=\text{sqrt}\left(\frac{\displaystyle 1}{\displaystyle n}\right),}\\
  {\tan\arcsin\cos\arctan(\text{sqrt}(\frac{\displaystyle 1}{\displaystyle n}))=\text{sqrt}(n),}
\end{array}
$$

and we can generate numbers in forms of $\text{sqrt}(n)$ and $\text{sqrt}(\frac{1}{n})$, which obviously comprise all
numbers in forms of $n$ and $\frac{1}{n}$.

### The Case When $m>n$
Consider the case $m>n$. In fact, we have already solved the problem of
$\text{sqrt}(\frac{n}{m})$ when $m\text{ mod }n=0$. To generate numbers in the form of
$\text{sqrt}(\frac{n}{m})$ when $m\text{ mod }n\not=0$, we need to generate
$\text{sqrt}\left(\frac{m\text{ mod }n}{n}\right)$, by applying the identity $\tan\arcsin\cos\arctan(x)=\frac{1}{x}$
once

$$
\begin{array}{l}
  {\tan\arcsin\cos\arctan(\text{sqrt}\left(\frac{\displaystyle m\text{ mod }n}{\displaystyle n}\right))=\text{sqrt}\left(\frac{\displaystyle n}{\displaystyle m\text{ mod }n}\right),}
\end{array}
$$

(note that $m\text{ mod }n\not=0$) and applying the recurrence $\sin\arctan(x)$ $k=\lfloor m/n\rfloor$ times

$$
\begin{array}{l}
  {(\sin\arctan)^k(\text{sqrt}\left(\frac{\displaystyle n}{\displaystyle m\text{ mod }n}\right))
  =\text{sqrt}\left(\frac{\displaystyle n}{\displaystyle m}\right)}
\end{array}
$$

In this way, we can reduce the problem to generating $\text{sqrt}\left(\frac{n}{m\text{ mod }n}\right)$ and decrease the
denominator from $m$ to $n$, just like the **Euclidean algorithm**. Iteratively run this procedure until
$m\text{ mod }n=0$ (which would occur in no more than $n$ steps), a problem we have already solved.

### The Case When $m<n$
Consider the case $m<n$. We should solve for $\text{sqrt}(\frac{m}{n})$ first and then apply the identity for
reciprocal once

$$
\begin{array}{l}
  {\tan\arcsin\cos\arctan(\text{sqrt}\left(\frac{m}{n}\right))=\text{sqrt}\left(\frac{n}{m}\right).}
\end{array}
$$

As a result, we can generate any numbers in the form of $\text{sqrt}(\frac{n}{m})$, including
$\frac{113}{355}=\text{sqrt}\left(\frac{113\cdot113}{355\cdot355}\right)$.

## Simulation with Python
We write a Python 3.0 programs to implement this iterative algorithm.

```python
# exchange den and num so that num/den is in (0, 1].
def reciprocate(num, den, ans):
    ans += "tan arccos sin arctan "
    return den, num, ans

# print k-fold composition.
def compose(num, den, ans, funs="sin arctan "):
    if den // num > 1:
        ans += "(" + funs + ") ^ " + str(den // num) + " "
    elif den // num == 1:
        ans += funs
    return num, den % num, ans

if __name__ == "__main__":
    num, den = 113 ** 2, 355 ** 2   # we desire sqrt(num/den).
    ans = ""

    if num == 0:
        print("0")
        exit(0)

    while den % num: # suppose num/den can not reduce to 1/(den//num).
        num, den, ans = compose(num, den, ans)
        num, den, ans = reciprocate(num, den, ans)

    _, _, ans = compose(1, den // num - 1, ans)

    ans += "sin arccos 0" # start with 1.
    print(ans)
```

The Python 3.0 program yields the complete and lengthy sequence of function compositions given by

```python
(sin arctan ) ^ 9 tan arccos sin arctan sin arctan tan arccos sin arctan (sin arctan ) ^ 6 tan arccos sin arctan
sin arctan tan arccos sin arctan (sin arctan ) ^ 2 tan arccos sin arctan (sin arctan ) ^ 45 tan arccos sin arctan sin
arctan tan arccos sin arctan (sin arctan ) ^ 10 sin arccos 0
```

## Remarks
As a concluding remark, we should emphasize that we can generate all numbers in the form of
$\text{sqrt}\left(\frac{n}{m}\right)$, which obviously comprise all numbers in the form of $\frac{n}{m}$. Provoked by
the fact that $\frac{\pi}{2}=\arccos0$ and $\frac{\pi}{4}=\arctan\sin\arccos0$, can we raise a further question
regarding the ability of this defective calculator to generate numbers in the form of $\frac{n}{m}\pi$ or even
$\text{sqrt}\left(\frac{n}{m}\right)\pi$?

{% include links.html %}
