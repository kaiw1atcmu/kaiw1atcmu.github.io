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
\begin{array}{l}
    {f_t=\sigma(W_f x_t+U_f h_{t-1}+b_f)} \\
    {i_t=\sigma(W_i x_t+U_i h_{t-1}+b_i)} \\
    {o_t=\sigma(W_o x_t+U_o h_{t-1}+b_o)} \\
    {\tilde{C_t}=\text{tanh}(W_c x_t+U_c h_{t-1}+b_c)} \\
    {C_t=f_t\odot C_{t-1}+i_t\odot \tilde{C_t}} \\
    {h_t=o_t\odot \text{tanh}(C_t)}
\end{array}
$$

As per notations in [On the difficulty of training Recurrent Neural Networks](#references), in order to better highlight
the exploding gradients problem, the error terms are obtained by writing the gradients in a sum-of-products form:

$$
\begin{array}{l}
    {\partial \varepsilon/\partial \theta=\sum_{1\le t\le T}\ \partial \varepsilon_t/\partial \theta
    =\sum_{1\le t\le T}\ (\partial \varepsilon_t/\partial h^T_t)(\partial h_t/\partial \theta)} \\
    {\partial \varepsilon_t/\partial \theta=\sum_{1\le k\le t}\ (\partial \varepsilon_t/\partial h^T_t)
    (\partial h_t/\partial h^T_k)(\partial^+ h_k/\partial \theta)} \\
    {\partial h_t/\partial h^T_k=\prod_{t\le i\lt k}\partial h_i/\partial h^T_{i-1}=
    \prod_{t\le i\lt k}W^T_{rec}\text{diag}\sigma'(h_{i-1})}
\end{array}
$$

Imagine network parameters $\theta$ as row vectors (so that gradients of columns vectors with respect to row vectors are
conveniently defined as matrices), and the gradients of hidden units $h_t$ with respect to $\theta$ are given by:

$$
\begin{array}{l}
    {\partial h_t/\partial \theta} \\
    {=\text{diag}(o_t,1-\text{tanh}^2C_t)\partial C_t/\partial \theta+\text{diag}(\text{tanh}C_t)\partial o_t/\partial \theta} \\
    {=\text{diag}(o_t,1-\text{tanh}^2C_t)} \\
    {(\text{diag}(f_t)\partial C_{t-1}/\partial \theta+\text{diag}(C_{t-1})\partial f_t/\partial \theta
    +\text{diag}(i_t)\partial \tilde{C_t}/\partial \theta+\text{diag}(\tilde{C_t})\partial i_t/\partial \theta)+\text{diag}(\text{tanh}C_t)\partial o_t/\partial \theta}
\end{array}
$$

$$
\begin{array}{l}
    {\partial h_t/\partial \theta=\text{diag}(o_t,1-\text{tanh}^2C_t)[\text{diag}(f_t)\partial C_{t-1}/\partial \theta} \\
    {+\text{diag}(C_{t-1})\partial^+ f_t/\partial \theta+\text{diag}(C_{t-1},f_t,1-f_t)U_f\partial h_{t-1}/\partial \theta} \\
    {+\text{diag}(i_t)\partial^+ \tilde{C_t}/\partial \theta+\text{diag}(i_t,1-\tilde{C_t}^2)U_c\partial h_{t-1}/\partial \theta} \\
    {+\text{diag}(\tilde{C_t})\partial^+ i_t/\partial\theta+\text{diag}(\tilde{C}_t,i_t,1-i_t)U_i\partial h_{t-1}/\partial\theta}] \\
    {+\text{diag}(1-\text{tanh}^2C_t)\partial^+o_t/\partial\theta+\text{diag}(1-\text{tanh}^2C_t,o_t,1-o_t)U_o\partial h_{t-1}/\partial \theta}
\end{array}
$$

We have

$$
\begin{array}{l}
    {\partial h_t/\partial \theta=A_{1,t}\partial h_{t-1}/\partial \theta+A_{2,t}\partial C_{t-1}/\partial \theta+A_{0,t}}
\end{array}
$$

where

$$
\begin{array}{l}
    {A_{1,t}=\text{diag}(o_t,1-\text{tanh}^2C_t)[\text{diag}(C_{t-1},f_t,1-f_t)U_f+\text{diag}(i_t,1-\tilde{C_t}^2)U_c+\text{diag}(\tilde{C}_t,i_t,1-i_t)U_i]+\text{diag}(C_t,o_t,1-o_t)U_o} \\
    {A_{2,t}=\text{diag}(o_t,1-\text{tanh}^2C_t)\text{diag}(f_t)} \\
    {A_{0,t}=\text{diag}(o_t,1-\text{tanh}^2C_t)[\text{diag}(C_{t-1})\partial^+ f_t/\partial \theta+\text{diag}(i_t)\partial^+ \tilde{C_t}/\partial \theta+\text{diag}(\tilde{C_t})\partial^+ i_t/\partial\theta]+\text{diag}(1-\text{tanh}^2C_t)\partial^+o_t/\partial\theta}
\end{array}
$$

Use the fact that

$$
\begin{array}{l}
    {\partial C_t/\partial \theta=\text{diag}(f_t)\partial C_{t-1}/\partial \theta+\text{diag}(C_{t-1},f_t,1-f_t)U_f\partial h_{t-1}/\partial \theta+\text{diag}(i_t,1-\tilde{C_t}^2)U_c\partial h_{t-1}/\partial \theta+\text{diag}(\tilde{C_t},i_t,1-i_t)U_i\partial h_{t-1}/\partial \theta} \\
\end{array}
$$

We have

$$
\begin{array}{l}
    {\partial C_t/\partial \theta=B_{1,t}\partial h_{t-1}/\partial \theta+B_{2,t}\partial C_{t-1}/\partial \theta+B_{0,t}}
\end{array}
$$

where

$$
\begin{array}{l}
    {B_{1,t}=\text{diag}(C_{t-1},f_t,1-f_t)U_f+\text{diag}(i_t,1-\tilde{C_t}^2)U_c+\text{diag}(\tilde{C_t},i_t,1-i_t)U_i} \\
    {B_{2,t}=\text{diag}(f_t)} \\
    {B_{0,t}=0}
\end{array}
$$

We have a set of iterative equations

$$
\begin{array}{l}
    {\partial h_t/\partial \theta=A_{1,t}\partial h_{t-1}/\partial \theta+A_{2,t}\partial C_{t-1}/\partial \theta+A_{0,t}} \\
    {\partial C_t/\partial \theta=B_{1,t}\partial h_{t-1}/\partial \theta+B_{2,t}\partial C_{t-1}/\partial \theta+B_{0,t}}
\end{array}
$$

## References
Razvan Pascanu, Tomas Mikolov, and Yoshua Bengio. 2013. On the difficulty of training Recurrent Neural Networks.
