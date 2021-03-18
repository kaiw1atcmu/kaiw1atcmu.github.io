---
title: "Maths On RNNs, LSTMs, and GRUs"
keywords: rnn, lstm, gru
date: 2021-03-10 00:45:02 -0800
last_updated: March 10, 2021
tags: [deep_learning, machine_learning, applied_mathematics]
summary: "The exact maths of Recurrent Neural Networks (RNNs), especially their gradients of error with respect to the
network parameters via the BPTT formulas, are too involved to analyze. In addition, RNNs in their vanilla form suffer
from the notorious vanishing/exploding gradients problems. Luckily, RNN variants (e.g. LSTMs, GRUs) come to rescue."
sidebar: none
permalink: maths_on_rnns_lstms_and_grus.html
folder: mydoc
---

The exact maths of Recurrent Neural Networks (RNNs), especially their gradients of error with respect to the network
parameters via the BPTT formulas, are extremely too involved to analyze. In addition, RNNs in their vanilla form suffer
from the notorious vanishing/exploding gradients problems. Luckily, RNN variants (e.g. LSTMs, GRUs) come to rescue. In
this post, let's explore the mathematical details of them!

## Warm-up
Although it might seem obvious, still, we start by emphasizing that gradients of a quantity with respect to a variable
should always be well-defined. Usually, we could naturally define gradients

$$
    {\frac{\partial f}{\partial x^T}\in R^{1\times n},\ \text{if }f\in R,\ x\in R^n\ \text{(scalar w.r.t. row vector)}} \\
    {\frac{\partial u}{\partial v^T}\in R^{n\times m},\ \text{if }u\in R^n,\ v\in R^m\ \text{(column vector w.r.t. row vector)}} \\
    {\frac{\partial f}{\partial W}\in R^{n\times m},\ \text{if }f\in R,\ W\in R^{n\times m}\ \text{(scalar w.r.t. matrix)},}
$$
 
and basic gradient rules as building blocks of our calculation

$$
\begin{align\*}
    {\frac{\partial f\!\odot\! g}{\partial \theta^T} &= \text{diag}(f)\frac{\partial g}{\partial \theta^T}+\text{diag}(g)\frac{\partial f}{\partial \theta^T},\ f,g\in R,\ \theta\in R^n} \\
    {\frac{\partial Wu}{\partial u^T} &= W,\ W\in R^{m\times n},\ u\in R^n} \\
    {\frac{\partial f(Wu)}{\partial u^T} &= \text{diag}(f'(Wu))W,\ f\in R^m,\ W\in R^{m\times n},\ u\in R^n.}
\end{align\*}
$$

One merit associated with our definition is the handy, transpose-free matrix-form Chain Rule

$$
    {\frac{\partial f}{\partial \theta^T}=\frac{\partial f}{\partial v^T}\frac{\partial v}{\partial \theta^T}.}
$$

## Maths On RNNs
### RNN Forward Propagation Formulas
Supposing input vectors $x_t\in R^m$ and hidden vectors $h_t\in R^n$, the RNN forward propagation formulas take the form
of (for $t>0$)

$$
    {h_t=W_{rec}\sigma(h_{t-1})+W_{in}x_t+b,\ W_{rec}\in R^{n\times n},\ W_{in}\in R^{n\times m},\ b\in R^n}
$$

with the boundary condition that $h_0$ is a constant (using zero or random initialization).

### Solve For Recurrences

$$
    {\frac{\partial h_t}{\partial h^T_{t-1}}=\text{diag}(\sigma'(h_{t-1}))W_{rec},\ \text{for } t>0.}
$$

### RNN Back-Propagation-Through-Time (BPTT) Formulas
As per notations in [On the difficulty of training Recurrent Neural Networks](#references) (with minor modifications to
fit in with our above-mentioned definitions of gradients), in order to better highlight the exploding gradients problem,
the error terms are obtained by writing the gradients in a sum-of-products form (where $\partial^+$ refers to the
"immediate" partial derivative with respect to $\theta^T$, i.e. where the quantity following $\partial^+$ is taken as a
constant with respect to $\theta^T$)

$$
    {\frac{\partial \varepsilon}{\partial \theta^T}=\sum_{1\le t\le T}\ \frac{\partial \varepsilon_t}{\partial \theta^T}} \\
    {\frac{\partial \varepsilon_t}{\partial \theta^T}=\sum_{1\le k\le t}\ \frac{\partial \varepsilon_t}{\partial h^T_t}
    \frac{\partial h_t}{\partial h^T_k}\frac{\partial^+ h_k}{\partial \theta^T}} \\
    {\frac{\partial h_t}{\partial h^T_k}=\prod_{t\ge i\gt k}\frac{\partial h_i}{\partial h^T_{i-1}}
    =\prod_{t\ge i\gt k}\text{diag}(\sigma'(h_{i-1}))W_{rec}.}
$$

{% include note.html content="Note that $\theta$ is a non-empty subset of $[W_{rec},W_{in},b]$ as a column vector." %}

### Analysis For Vanishing/Exploding Gradients
The authors further loosely distinguish between <font face="Lora">long term</font>
and <font face="Lora">short term</font> contributions, where long term refers to components for which $k\ll t$ and short
term to everything else. As introduced in
[Learning long-term dependencies with gradient descent is difficult](#references),
the <font face="Lora">vanishing gradients</font> and <font face="Lora">exploding gradients</font> problems are defined.

> The vanishing gradients problem refers to the opposite behaviour, when long term components go exponentially fast to
> norm 0, making it impossible for the model to learn correlation between temporally distant events.
>
> The exploding gradients problem refers to the large increase in the norm of the gradient during training. Such events
> are caused by the explosion of the long term components, which can grow exponentially more then short term ones.

{% include warning.html content="Bear in mind the very definition of the vanishing gradients problem. This should not be
(mis)understood simply as gradients could vanish such that parameters would essentially not be updated. Indeed, it means
the long-term gradient components could vanish such that updates to parameters would not adequately reflect long-term
dependencies." %}

More to come...

## Maths On LSTMs
### LSTM Forward Propagation Formulas
Supposing input vectors $x_t\in R^m$, hidden vectors $h_t\in R^n$, and channel vectors $C_t\in R^n$, the LSTM forward
propagation formulas take the form of (for $t>0$)

$$
    {f_t=\sigma(W_f x_t+U_f h_{t-1}+b_f),\ f_t,b_f\in R^n,\ W_f\in R^{n\times m},\ U_f\in R^{n\times n}} \\
    {i_t=\sigma(W_i x_t+U_i h_{t-1}+b_i),\ i_t,b_i\in R^n,\ W_i\in R^{n\times m},\ U_i\in R^{n\times n}} \\
    {o_t=\sigma(W_o x_t+U_o h_{t-1}+b_o),\ o_t,b_o\in R^n,\ W_o\in R^{n\times m},\ U_o\in R^{n\times n}} \\
    {\tilde{C_t}=\text{tanh}(W_c x_t+U_c h_{t-1}+b_c),\ \tilde{C_t},b_c\in R^n,\ W_c\in R^{n\times m},\ U_c\in R^{n\times n}} \\
    {C_t=f_t\odot C_{t-1}+i_t\odot \tilde{C_t}} \\
    {h_t=o_t\odot \text{tanh}(C_t)}
$$

### Solve For Recurrences
Imagine network parameters $\theta$ as column vectors, so that gradients of columns vectors with respect to row vectors
are conveniently defined as matrices. Denoting diagonal matrices constructed from element-wise products of several
column vectors $v_1,\ldots,v_i$ as $\text{diag}(v_1,\ldots,v_i)$ for brevity, the gradients of hidden units $h_t$ with
respect to $\theta^T$ take the form of (for $t>0$)

$$
    {\frac{\partial h_t}{\partial \theta^T}
    =\text{diag}(o_t,1-\text{tanh}^2C_t)\frac{\partial C_t}{\partial \theta^T}+\text{diag}(\text{tanh}C_t)\frac{\partial o_t}{\partial \theta^T}} \\
    {=\text{diag}(o_t,1-\text{tanh}^2C_t)[\text{diag}(f_t)\frac{\partial C_{t-1}}{\partial \theta^T}+\text{diag}(C_{t-1})\frac{\partial f_t}{\partial \theta^T}
    +\text{diag}(i_t)\frac{\partial \tilde{C_t}}{\partial \theta^T}+\text{diag}(\tilde{C_t})\frac{\partial i_t}{\partial \theta^T}]
    +\text{diag}(\text{tanh}C_t)\frac{\partial o_t}{\partial \theta^T}}
$$

Expanding, that is

$$
    {\frac{\partial h_t}{\partial \theta^T}=\text{diag}(o_t,1-\text{tanh}^2C_t)[\text{diag}(f_t)\frac{\partial C_{t-1}}{\partial \theta^T}
    +\text{diag}(C_{t-1})\frac{\partial^+ f_t}{\partial \theta^T}+\text{diag}(C_{t-1},f_t,1-f_t)U_f\frac{\partial h_{t-1}}{\partial \theta^T}} \\
    {+\text{diag}(i_t)\frac{\partial^+ \tilde{C_t}}{\partial \theta^T}+\text{diag}(i_t,1-\tilde{C_t}^2)U_c\frac{\partial h_{t-1}}{\partial \theta^T}
    +\text{diag}(\tilde{C_t})\frac{\partial^+ i_t}{\partial \theta^T}+\text{diag}(\tilde{C}_t,i_t,1-i_t)U_i\frac{\partial h_{t-1}}{\partial \theta^T}}] \\
    {+\text{diag}(\text{tanh}C_t)\frac{\partial^+ o_t}{\partial \theta^T}+\text{diag}(\text{tanh}C_t,o_t,1-o_t)U_o\frac{\partial h_{t-1}}{\partial \theta^T}}
$$

In a more compact form, we have

$$
    {\frac{\partial h_t}{\partial \theta^T}=A_{1,t}\frac{\partial h_{t-1}}{\partial \theta^T}+A_{2,t}\frac{\partial C_{t-1}}{\partial \theta^T}+A_{0,t}}
$$

where

$$
    {A_{1,t}=\text{diag}(o_t,1-\text{tanh}^2C_t)[\text{diag}(C_{t-1},f_t,1-f_t)U_f+\text{diag}(i_t,1-\tilde{C_t}^2)U_c+\text{diag}(\tilde{C}_t,i_t,1-i_t)U_i]+\text{diag}(C_t,o_t,1-o_t)U_o} \\
    {A_{2,t}=\text{diag}(o_t,1-\text{tanh}^2C_t,f_t)} \\
    {A_{0,t}=\text{diag}(o_t,1-\text{tanh}^2C_t)[\text{diag}(C_{t-1})\frac{\partial^+ f_t}{\partial \theta^T}+\text{diag}(i_t)\frac{\partial^+ \tilde{C_t}}{\partial \theta^T}
    +\text{diag}(\tilde{C_t})\frac{\partial^+ i_t}{\partial \theta^T}]+\text{diag}(\text{tanh}C_t)\frac{\partial^+ o_t}{\partial \theta^T}}
$$

Repeat similar procedures to obtain $\partial C_t/\partial \theta^T$. That is

$$
    {\frac{\partial C_t}{\partial \theta^T}=\text{diag}(f_t)\frac{\partial C_{t-1}}{\partial \theta^T}+\text{diag}(C_{t-1})\frac{\partial f_t}{\partial \theta^T}+\text{diag}(i_t)\frac{\partial \tilde{C_t}}{\partial \theta^T}+\text{diag}(\tilde{C_t})\frac{\partial i_t}{\partial \theta^T}} \\
    {=\text{diag}(f_t)\frac{\partial C_{t-1}}{\partial \theta^T}+\text{diag}(C_{t-1})[\frac{\partial^+ f_t}{\partial \theta^T}+\text{diag}(f_t,1-f_t)U_f\frac{\partial h_{t-1}}{\partial \theta^T}]} \\
    {+\text{diag}(i_t)[\frac{\partial^+ \tilde{C_t}}{\partial \theta^T}+\text{diag}(1-\tilde{C_t}^2)U_c\frac{\partial h_{t-1}}{\partial \theta^T}]
    +\text{diag}(\tilde{C_t})[\frac{\partial^+ i_t}{\partial \theta^T}+\text{diag}(i_t,1-i_t)U_i\frac{\partial h_{t-1}}{\partial \theta^T}]}
$$

In a more compact form, we have

$$
    {\frac{\partial C_t}{\partial \theta^T}=B_{1,t}\frac{\partial h_{t-1}}{\partial \theta^T}+B_{2,t}\frac{\partial C_{t-1}}{\partial \theta^T}+B_{0,t}}
$$

where

$$
    {B_{1,t}=\text{diag}(C_{t-1},f_t,1-f_t)U_f+\text{diag}(i_t,1-\tilde{C_t}^2)U_c+\text{diag}(\tilde{C_t},i_t,1-i_t)U_i} \\
    {B_{2,t}=\text{diag}(f_t)} \\
    {B_{0,t}=\text{diag}(C_{t-1})\frac{\partial^+ f_t}{\partial \theta^T}+\text{diag}(i_t)\frac{\partial^+ \tilde{C_t}}{\partial \theta^T}+\text{diag}(\tilde{C_t})\frac{\partial^+ i_t}{\partial \theta^T}}
$$

As a result, we find useful recurrences for BPTT (for $t>0$). That is

$$
    {\frac{\partial h_t}{\partial \theta^T}=A_{1,t}\frac{\partial h_{t-1}}{\partial \theta^T}+A_{2,t}\frac{\partial C_{t-1}}{\partial \theta^T}+A_{0,t}} \\
    {\frac{\partial C_t}{\partial \theta^T}=B_{1,t}\frac{\partial h_{t-1}}{\partial \theta^T}+B_{2,t}\frac{\partial C_{t-1}}{\partial \theta^T}+B_{0,t}}
$$

with boundary conditions that ${\partial h_0}/{\partial \theta^T}=0$ and ${\partial C_0}/{\partial \theta^T}=0$, since
by assumption input vectors $x_t$ start at $t=1$. Or simply (for $t>0$)

$$
    {\frac{\partial hC_t}{\partial \theta^T}=D_t\frac{\partial hC_{t-1}}{\partial \theta^T}+E_t}
$$

where ${\partial hC_t}/{\partial \theta^T}$ is the vertically concatenated matrices ${\partial h_t}/{\partial \theta^T}$
and ${\partial C_t}/{\partial\theta^T}$, $D_t$ is the partitioned matrix formed by $A_{1,t}, A_{2,t}, B_{1,t}, B_{2,t}$,
and $E_t$ is the vertically concatenated matrices $A_{0,t}$ and $B_{0,t}$, respectively. The boundary conditions should
be ${\partial hC_0}/{\partial \theta^T}=0$.

### LSTM Back-Propagation-Through-Time (BPTT) Formulas
Interestingly, as with RNNs, the gradients of error $\varepsilon$ with respect to $\theta^T$ again take the almost
identical form of:

$$
    {\frac{\partial \varepsilon}{\partial \theta^T}=\sum_{1\le t\le T}\ \frac{\partial \varepsilon_t}{\partial \theta^T}} \\
    {\frac{\partial \varepsilon_t}{\partial \theta^T}=\sum_{1\le k\le t}\ \frac{\partial \varepsilon_t}{\partial hC^T_t}
    \frac{\partial hC_t}{\partial hC^T_k}\frac{\partial^+ hC_k}{\partial \theta^T}
    =\sum_{1\le k\le t}\ \frac{\partial \varepsilon_t}{\partial hC^T_t}\frac{\partial hC_t}{\partial hC^T_k}E_k} \\
    {\frac{\partial hC_t}{\partial hC^T_k}=\prod_{t\ge i\gt k}\frac{\partial hC_i}{\partial hC^T_{i-1}}=\prod_{t\ge i\gt k}D_i}
$$

### Analysis For Non-Vanishing/Exploding Gradients
The proof for avoidance of the vanishing gradients problem is tantamount to showing that $\prod_{t\ge i\gt k}D_i$ won't
converge in norm to 0 for $k\ll t$, at least asymptotically or almost surely. Now let us revisit $D_i$ by partitioning
it into several $n\times n$ diagonal matrices $P_{1-5,i}$ and $Q_{1-5,i}$ which are easily found by straight-forward
computing and comparing weights of $U_f,U_c,U_i,U_o$. Let us define

$$
    {J_i=[P_{1,i}\ P_{2,i}\ P_{3,i}\ P_{4,i}\ P_{5,i};Q_{1,i}\ Q_{2,i}\ Q_{3,i}\ Q_{4,i}\ Q_{5,i}] \in R^{2n\times5n},} \\
    {U=\text{diag}([U_f;U_c;U_i;U_o;I]) \in R^{5n\times5n},} \\
    {V=[I,I,I,I,O;O,O,O,O,I]^T \in R^{5n\times2n}.}
$$

Since $D_i=J_iUV$ and $D_i$'s are multiplied sequentially, we could equivalently analyze

$$
    {\tilde{D_i}=VJ_iU \text{ or } \bar{D_i}=UVJ_i,}
$$

except for the leading $D_t$ and the trailing $D_{k+1}$. In doing so, we managed to decouple the variables (i.e. ones
that contain $h_t$ or $C_t$) and the parameters (i.e. otherwise). As long as $\prod_{t-1\ge i\gt k+1}\tilde{D_i}$ or
$\prod_{t-1\ge i\gt k+1}\bar{D_i}$ won't converge in norm to 0 for $k\ll t$, the vanishing gradients problem is avoided.

More to come...

## Maths On GRUs
### GRU Forward Propagation Formulas
Supposing input vectors $x_t\in R^m$, and hidden vectors $h_t\in R^n$, the GRU forward propagation formulas take the
form of (for $t>0$)

$$
    {r_t=\sigma(W_r x_t+U_r h_{t-1}+b_r),\ r_t,b_r\in R^n,\ W_r\in R^{n\times m},\ U_r\in R^{n\times n}} \\
    {z_t=\sigma(W_z x_t+U_z h_{t-1}+b_z),\ z_t,b_z\in R^n,\ W_z\in R^{n\times m},\ U_z\in R^{n\times n}} \\
    {\tilde{h_t}=\text{tanh}(W_h x_t+U_h (r_t\odot h_{t-1})+b_h),\ \tilde{h_t},b_h\in R^n,\ W_h\in R^{n\times m},\ U_h\in R^{n\times n}} \\
    {h_t=z_t\odot h_{t-1}+(1-z_t)\odot \tilde{h_t}}
$$

### Solve For Recurrences

### GRU Back-Propagation-Through-Time (BPTT) Formulas

### Analysis For Non-Vanishing/Exploding Gradients
More to come...

## References
Razvan Pascanu, Tomas Mikolov, and Yoshua Bengio. 2013. On the difficulty of training Recurrent Neural Networks.

Bengio, Y., Simard, P., and Frasconi, P. 1994. Learning long-term dependencies with gradient descent is difficult. IEEE
Transactions on Neural Networks, 5(2), 157-166.
