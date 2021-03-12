---
title: "RNN Maths"
keywords: rnn, maths
date: 2021-03-10 00:45:02 -0800
last_updated: March 10, 2021
tags: [deep_learning, machine_learning, applied_mathematics]
summary: "The exact maths of Recurrent Neural Networks (RNNs), especially their gradients of error with respect to the
network parameters via the BPTT formulas, are too involved to analyze. In addition, RNNs in their vanilla form suffer
from the notorious vanishing/exploding gradients problems. Luckily, RNN variants (e.g. LSTMs, GRUs) come to rescue."
sidebar: none
permalink: rnn_maths.html
folder: mydoc
---

The exact maths of Recurrent Neural Networks (RNNs), especially their gradients of error with respect to the network
parameters via the BPTT formulas, are extremely too involved to analyze. In addition, RNNs in their vanilla form suffer
from the notorious vanishing/exploding gradients problems. Luckily, RNN variants (e.g. LSTMs, GRUs) come to rescue. In
this post, let's explore the mathematical details of them!

## Basic RNN Formulas

## Basic LSTM Formulas
The basic LSTM forward propagation formulas of are given by

$$
    {f_t=\sigma(W_f x_t+U_f h_{t-1}+b_f)} \\
    {i_t=\sigma(W_i x_t+U_i h_{t-1}+b_i)} \\
    {o_t=\sigma(W_o x_t+U_o h_{t-1}+b_o)} \\
    {\tilde{C_t}=\text{tanh}(W_c x_t+U_c h_{t-1}+b_c)} \\
    {C_t=f_t\odot C_{t-1}+i_t\odot \tilde{C_t}} \\
    {h_t=o_t\odot \text{tanh}(C_t)}
$$

As per notations in [On the difficulty of training Recurrent Neural Networks](#references), in order to better highlight
the exploding gradients problem, the error terms are obtained by writing the gradients in a sum-of-products form:

$$
    {\frac{\partial \varepsilon}{\partial \theta}=\sum_{1\le t\le T}\ \frac{\partial \varepsilon_t}{\partial \theta}
    =\sum_{1\le t\le T}\ \frac{\partial \varepsilon_t}{\partial h^T_t}\frac{\partial h_t}{\partial \theta}} \\
    {\frac{\partial \varepsilon_t}{\partial \theta}=\sum_{1\le k\le t}\ \frac{\partial \varepsilon_t}{\partial h^T_t}
    \frac{\partial h_t}{\partial h^T_k}\frac{\partial^+ h_k}{\partial \theta}} \\
    {\frac{\partial h_t}{\partial h^T_k}=\prod_{t\le i\lt k}\frac{\partial h_i}{\partial h^T_{i-1}}=
    \prod_{t\le i\lt k}W^T_{rec}\ \text{diag}(\sigma'(h_{i-1}))}
$$

Imagine network parameters $\theta$ as row vectors (so that gradients of columns vectors with respect to row vectors are
conveniently defined as matrices), and the gradients of hidden units $h_t$ with respect to $\theta$ are given by:

$$
    {\frac{\partial h_t}{\partial \theta}
    =\text{diag}(o_t,1-\text{tanh}^2C_t)\frac{\partial C_t}{\partial \theta}+\text{diag}(\text{tanh}C_t)\frac{\partial o_t}{\partial \theta}} \\
    {=\text{diag}(o_t,1-\text{tanh}^2C_t)
    [\text{diag}(f_t)\frac{\partial C_{t-1}}{\partial \theta}+\text{diag}(C_{t-1})\frac{\partial f_t}{\partial \theta}
    +\text{diag}(i_t)\frac{\partial \tilde{C_t}}{\partial \theta}+\text{diag}(\tilde{C_t})\frac{\partial i_t}{\partial \theta}]+\text{diag}(\text{tanh}C_t)\frac{\partial o_t}{\partial \theta}}
$$

That is

$$
    {\frac{\partial h_t}{\partial \theta}=\text{diag}(o_t,1-\text{tanh}^2C_t)[\text{diag}(f_t)\frac{\partial C_{t-1}}{\partial \theta}
    +\text{diag}(C_{t-1})\frac{\partial^+ f_t}{\partial \theta}+\text{diag}(C_{t-1},f_t,1-f_t)U_f\frac{\partial h_{t-1}}{\partial \theta}} \\
    {+\text{diag}(i_t)\frac{\partial^+ \tilde{C_t}}{\partial \theta}+\text{diag}(i_t,1-\tilde{C_t}^2)U_c\frac{\partial h_{t-1}}{\partial \theta}
    +\text{diag}(\tilde{C_t})\frac{\partial^+ i_t}{\partial\theta}+\text{diag}(\tilde{C}_t,i_t,1-i_t)U_i\frac{\partial h_{t-1}}{\partial\theta}}] \\
    {+\text{diag}(1-\text{tanh}^2C_t)\frac{\partial^+o_t}{\partial\theta}+\text{diag}(1-\text{tanh}^2C_t,o_t,1-o_t)U_o\frac{\partial h_{t-1}}{\partial \theta}}
$$

We have

$$
    {\frac{\partial h_t}{\partial \theta}=A_{1,t}\frac{\partial h_{t-1}}{\partial \theta}+A_{2,t}\frac{\partial C_{t-1}}{\partial \theta}+A_{0,t}}
$$

where

$$
    {A_{1,t}=\text{diag}(o_t,1-\text{tanh}^2C_t)[\text{diag}(C_{t-1},f_t,1-f_t)U_f+\text{diag}(i_t,1-\tilde{C_t}^2)U_c+\text{diag}(\tilde{C}_t,i_t,1-i_t)U_i]+\text{diag}(C_t,o_t,1-o_t)U_o} \\
    {A_{2,t}=\text{diag}(o_t,1-\text{tanh}^2C_t)\text{diag}(f_t)} \\
    {A_{0,t}=\text{diag}(o_t,1-\text{tanh}^2C_t)[\text{diag}(C_{t-1})\frac{\partial^+ f_t}{\partial \theta}+\text{diag}(i_t)\frac{\partial^+ \tilde{C_t}}{\partial \theta}+\text{diag}(\tilde{C_t})\frac{\partial^+ i_t}{\partial\theta}]+\text{diag}(1-\text{tanh}^2C_t)\frac{\partial^+o_t}{\partial\theta}}
$$

Use the fact that

$$
    {\frac{\partial C_t}{\partial \theta}=\text{diag}(f_t)\frac{\partial C_{t-1}}{\partial \theta}+\text{diag}(C_{t-1},f_t,1-f_t)U_f\frac{\partial h_{t-1}}{\partial \theta}+\text{diag}(i_t,1-\tilde{C_t}^2)U_c\frac{\partial h_{t-1}}{\partial \theta}+\text{diag}(\tilde{C_t},i_t,1-i_t)U_i\frac{\partial h_{t-1}}{\partial \theta}} \\
$$

We have

$$
    {\frac{\partial C_t}{\partial \theta}=B_{1,t}\frac{\partial h_{t-1}}{\partial \theta}+B_{2,t}\frac{\partial C_{t-1}}{\partial \theta}+B_{0,t}}
$$

where

$$
    {B_{1,t}=\text{diag}(C_{t-1},f_t,1-f_t)U_f+\text{diag}(i_t,1-\tilde{C_t}^2)U_c+\text{diag}(\tilde{C_t},i_t,1-i_t)U_i} \\
    {B_{2,t}=\text{diag}(f_t)} \\
    {B_{0,t}=0}
$$

We have a set of iterative equations

$$
    {\frac{\partial h_t}{\partial \theta}=A_{1,t}\frac{\partial h_{t-1}}{\partial \theta}+A_{2,t}\frac{\partial C_{t-1}}{\partial \theta}+A_{0,t}} \\
    {\frac{\partial C_t}{\partial \theta}=B_{1,t}\frac{\partial h_{t-1}}{\partial \theta}+B_{2,t}\frac{\partial C_{t-1}}{\partial \theta}+B_{0,t}}
$$

with boundary conditions ${\partial h_0}/{\partial \theta}=0$ and ${\partial c_0}/{\partial \theta}=0$. Or simply

$$
    {\frac{\partial hC_t}{\partial \theta}=D_t\frac{\partial hC_{t-1}}{\partial \theta}+E_t}
$$

where ${\partial hc_t}/{\partial \theta}$ is constructed by vertically stacking ${\partial h_t}/{\partial \theta}$ and
${\partial C_t}/{\partial \theta}$, respectively. $D_t$ and $E_t$ are the corresponding partitioned matrices constructed
by $A_{1,t}, A_{2,t}, B_{1,t}, B_{2,t}$, and $A_{0,t}, B_{0,t}$, respectively. The boundary conditions are
${\partial hc_0}/{\partial \theta}=0$.

## References
Razvan Pascanu, Tomas Mikolov, and Yoshua Bengio. 2013. On the difficulty of training Recurrent Neural Networks.
