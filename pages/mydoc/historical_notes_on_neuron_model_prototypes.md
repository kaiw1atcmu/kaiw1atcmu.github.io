---
title: "Historical Notes on Neuron Model Prototypes [more to come]"
keywords: neuron model
date: 2020-01-14 14:12:31 -0800
last_updated: December 26, 2020
tags: [machine_learning,deep_learning]
summary: "Today we will discuss basic feedforward network structures and their learning or training algorithms. Although
their structures seemed primitive at first glance, the philosophies behind were not. Since their initial paradigms
obtained a lot of inspiration from advances in neuroscience, researchers then tried to mathematically simulate the
interactions between neurons or perceptrons when networked together, and achieved amazing success."
sidebar: none
permalink: historical_notes_on_neuron_model_prototypes.html
folder: mydoc
---

Today we will discuss basic feedforward network structures and their learning or training algorithms. Although their
structures seemed primitive at first glance, the philosophies behind were not. Since their initial paradigms obtained
a lot of inspiration from advances in neuroscience, researchers then tried to mathematically simulate the interactions
between neurons or perceptrons when networked together, and achieved amazing success.

## The McCulloch-Pitts Neuron
The McCulloch-Pitts Neuron was published in the seminal neurological paper as an early model of brain function by
[McCulloch and Pitts (1943)](#references). This linear model could recognize two different categories of inputs by
testing whether $f(\mathbf{x},\mathbf{w})$ is positive or negative. However, the weights could be set by the human
operator instead of being trained. Sometimes a neuron with a hard limiter device is referred to as a McCulloch-Pitts
neuron.

## The Adaptive Linear Element (ADALINE)
The adaptive linear element (ADALINE) and its training algorithm known as the least mean squares (LMS) or **Widrow–Hoff
algorithm**, named after those who suggested it, was proposed in the early 1960s. Given a vector $\mathbf{x}$, the
output of the classifier will be $\mathbf{w}^t\mathbf{x}$ using offset feature vectors, i.e., feature vectors
$\mathbf{x}$ padded with a trailing $1$ corresponding to the bias in $\mathbf{w}$. The desired output will be denoted as
$y(\mathbf{x})=\pm1$. The weight vector will be computed so as to minimize the mean square error (MSE) between the
desired and true outputs, that is,

$$
  {\mathbf{J}=\mathbb{E}[\vert y(\mathbf{x})-\mathbf{w}^t\mathbf{x}\vert^2],}\\
  {\hat{\mathbf{w}}=\arg\min_{\mathbf{w}}\mathbf{J}(\mathbf{w}).}
$$

Then

$$
  {\hat{\mathbf{w}}=\mathbf{R}_{\mathbf{x}}^{-1}\mathbb{E}[\mathbf{x}y(\mathbf{x})].}
$$
    
Without presupposing knowledge of the underlying distributions, the answer has been provided by 
[Robbins and Monro (1951)](#references) in the more general context of stochastic approximation theory. Applying the
**Robbins-Monro iteration**, it becomes

$$
  {\hat{\mathbf{w}}(k)=\hat{\mathbf{w}}(k-1)+\rho_k\mathbf{x}_k(y_k-\hat{\mathbf{w}}(k-1)^t\mathbf{x}_k)}
$$

where the training samples $(\mathbf{x_k},y_k)$ are successively presented to the algorithm. The algorithm converges
asymptotically to the MSE solution, provided that the sequence of $\rho_k$ satisfies $\sum_{k=1}^{\infty}\rho_k=\infty$
and $\sum_{k=1}^{\infty}\rho_k^2<\infty$. More precisely, we have that $\hat{\mathbf{w}}(k)$ converges in probability to
the solution $\hat{\mathbf{w}}$

$$
  {\lim_{k\to\infty}p(\hat{\mathbf{w}}_k=\hat{\mathbf{w}})=1,}
$$

and the stronger, in the mean square sense, convergence is also true

$$
  {\lim_{k\to\infty}\mathbb{E}[\vert\hat{\mathbf{w}}(k)-\hat{\mathbf{w}}\vert^2]=0.}
$$

This type of training, which neglects the nonlinearity during training and applies the desired response just after the
adder of the linear combiner part of the neuron, was used by Widrow and Hoff. The resulting neuron architecture is known
as adaptive linear element (ADALINE). After training and once the weights have been fixed, the model is then applied
with the hard limiter following the linear combiner. In other words, the adaline is a neuron that is trained according
to the LMS instead of the perceptron algorithm. During inference, it applies a hard limiter to the output of the linear
combiner like perceptrons, yielding a $\pm1$ prediction.

Generalization to the multi-class case follows the same concept as before, and it is easily shown that it reduces to $M$
equivalent problems of scalar desired responses, one for each discriminant function.

## The Perceptron
The perceptron algorithm was originally proposed by Rosenblatt in the late 1950s. The algorithm was developed for
training the perceptron, the basic unit used for modeling neurons of the brain. This was considered central in
developing powerful models for machine learning.

### Two Classes and Linearly Separable
Suppose the samples in the training set are linearly separable, that is, there exists a hyperplane parameterized by a
weight vector $\mathbf{w}$ and a bias scalar $b$ such that $\mathbf{w}^t\mathbf{x}+b>0,\forall\mathbf{x}\in w_1$ and
$\mathbf{w}^t\mathbf{x}+b<0,\forall\mathbf{x}\in w_2$. The perceptron algorithms use a standard gradient descent method
for the iterative minimization of the cost function.

> **algorithm**: The Perceptron Learning Algorithm (primal form)  
> **function**: $f(\mathbf{x})=\text{sign}(\mathbf{w}^t\mathbf{x}+b)$  
> **input**: training set $T=\{(\mathbf{x}_1,y_1),\ldots,(\mathbf{x}_n,y_n)\}$  
> &emsp;&emsp; learning rate $\eta>0$  
> **output**: weight vector $\mathbf{w}\in\mathbf{R}^l$ and bias scalar $b\in\mathbf{R}$  
> // algorithm initializes  
> pick an arbitrary $\mathbf{w}$ and an arbitrary $b$  
> // algorithm starts  
> **while** not all samples in $T$ are checked against the most recent $\mathbf{w}$ and $b$  
> select an unchecked sample $\mathbf{x},y$  
> &emsp;&emsp; **if** $y(\mathbf{w}^t\mathbf{x}+b)\leq 0$  
> &emsp;&emsp;&emsp;&emsp; $\mathbf{w}\leftarrow\mathbf{w}+\eta y\mathbf{x}$  
> &emsp;&emsp;&emsp;&emsp; $b\leftarrow b+\eta y$  
> &emsp;&emsp; **end if**  
> **end while**  
> **return** $\mathbf{w}$ and $b$  

> **algorithm**: The Perceptron Learning Algorithm (dual form)  
> **function**: $f(\mathbf{x})=\text{sign}(\sum_{i=1}^M\alpha_iy_i\mathbf{x_i}^t\mathbf{x}+b)$  
> **input**: training set $T=\{(\mathbf{x_1},y_1),\ldots,(\mathbf{x_n},y_n)\}$  
> &emsp;&emsp; learning rate $\eta>0$  
> **output**: weight scalars $\alpha_i,i=1,\ldots,M$ and bias scalar $b$  
> // algorithm initializes  
> pick an arbitrary $\alpha_i,i=1,\ldots,M$ and an arbitrary $b$  
> // algorithm starts  
> **while** not all samples in $T$ are checked against the most recent $\alpha_i,i=1,\ldots,M$ and $b$  
> &emsp;&emsp; select an unchecked sample $\mathbf{x},y$  
> &emsp;&emsp; **if** $y(\sum_{i=1}^M\alpha_iy_i\mathbf{x}_i^t\mathbf{x}+b)\leq 0$  
> &emsp;&emsp;&emsp;&emsp; $\alpha_i\leftarrow\alpha_i+\eta$  
> &emsp;&emsp;&emsp;&emsp; $b\leftarrow b+\eta y$  
> &emsp;&emsp; **end if**  
> **end while**  
> **return** $\alpha_i,i=1,\ldots,M$ and $b$  

For the dual form, inner products $\mathbf{x_i}^t\mathbf{x}$ could be computed and stored in a lookup table prior to
running the algorithm, <I>i.e</I>, trading space complexity for computational complexity. Note that in order for the
algorithms to terminate and achieve this optimal vector weight $\mathbf{w}$ and scalar bias $b$, it may take more than
one iteration step, partly depending on the values of learning rate. It is easy to show that the perceptron algorithm
converges to a solution in a finite number of iteration steps with non-negative and varying $\eta_t$, provided that the
sequence of $\eta_t$ satisfies $\sum_{t=1}^{\infty}\eta_t=\infty$ and $\sum_{t=1}^{\infty}\eta_t^2<\infty$. Following
similar arguments, it is easy to show that the algorithm also converges in a finite number of iteration steps, for
properly chosen $\eta$ which is constant and bounded above. In addition, Novikoff explored an alternative condition
under which convergence is guaranteed with a finite number of iterations, for any positive and constant learning rate
$\eta$. Interested readers may refer to this [Hang Li (2011)](#references) for details.

### Two Classes and Not Linearly Separable
If the basic requirement of linear separability in the training data is not satisfied, as is usually the case in
practice, the perceptron algorithm does not converge. In this case, a variant of the perceptron algorithm called the
**pocket algorithm** ensures convergence to an optimal solution with probability one. The pocket algorithm consists of
the following two steps:

> Initialize the weight vector $\mathbf{w}$ randomly. Define a stored (in the pocket!) vector $\mathbf{w}_s$.
>
> Set a history counter $h_s$ of the $\mathbf{w}_s$ to zero.
>
> At the $t$-th iteration step compute the update $\mathbf{w}$, according to the perceptron rule. Use the updated
> weight vector to test the number $h$ of training vectors that are classified correctly. If $h>h_s$ replace
> $\mathbf{w}_s$ with $\mathbf{w}$ and $h_s$ with $h$. Continue the iterations.  

It can be shown that this algorithm converges with probability one to the optimal solution, that is, the one that
produces the minimum number of misclassifications.

### Multi-Class and Linearly Separable
The **Kesler's construction** is a straightforward generalization from two-class perceptrons to multi-class case. As was
mentioned a couple of paragraphs above, an $l+1$ dimensional offset feature vector $\mathbf{x}$ is obtained by padding
the feature vector with a trailing $1$ which corresponds to the scalar bias $b$. Then this $\mathbf{x}$ is classified in
class $w_i$ using offset features and offset linear discriminants $\mathbf{w}$, if
$\mathbf{w}_i^t\mathbf{x}>\mathbf{w}_j^t\mathbf{x},\forall j\not=i$, leading to the so-called Kesler's construction:

$$
  {\mathbf{x}_{ij}=[\mathbf{0}^t,\ldots,\underbrace{\mathbf{x}^t}_{i-\text{th}},\ldots,
  -\underbrace{\mathbf{x}^t}_{j-\text{th}},\ldots,\mathbf{0}^t]^t\in \mathbf{R}^{(l+1)M},}\\
  {\mathbf{w}=[\mathbf{w}_1^t,\ldots,\mathbf{w}_M^t]\in \mathbf{R}^{(l+1)M}.}
$$

If $\mathbf{x}\in w_i$, this imposes the requirement that $\mathbf{w}^t\mathbf{x}_{ij}>0,\forall j\not=i$. The task is
now to design a linear classifier in the extended $(l+1)M$ dimensional space, so that each of the $(M-1)n$ training
vectors lie in its positive side. The perceptron algorithm will have no difficulty in solving this problem for
$\mathbf{w}$, provided that such a solution is possible, that is, if all the training vectors can be correctly
classified using a set of linear discriminant functions $\mathbf{w}_i$.

An alternative setup to Kesler's construction is to define an offset feature for each individual class $w_i$,

$$
  {\mathbf{x}_i=[\mathbf{0}^t,\ldots,\underbrace{\mathbf{x}^t}_{i-\text{th}},\ldots,
  \mathbf{0}^t]^t\in\mathbf{R}^{(l+1)M},}
$$

and a global weight vector $\mathbf{w}\in\mathbf{R}^{(l+1)M}$. For any feature vector $\mathbf{x}$, we predict the class
$\hat{y}=\arg\max_{i}\mathbf{w}^t\mathbf{x}_i$.

> **algorithm**: The Multi-Class Perceptron Learning Algorithm  
> **function**: $f(\mathbf{x})=\arg\max_{i}\mathbf{w}^t\mathbf{x_i}$  
> **input**: training set $T=\{(\mathbf{x_1},y_1),\ldots,(\mathbf{x_n},y_n)\}$  
> &emsp;&emsp; learning rate $\eta>0$  
> **output**: weight vector $\mathbf{w}\in\mathbf{R}^{(l+1)M}$  
> // algorithm initializes  
> pick an arbitrary $\mathbf{w}$  
> // algorithm starts  
> **while** not all samples in $T$ are checked against the most recent $\mathbf{w}$  
> &emsp;&emsp; select an unchecked sample $\mathbf{x},y$  
> &emsp;&emsp; compute $\hat{y}=\arg\max_i\mathbf{w}^t\mathbf{x}_i$  
> &emsp;&emsp; **if** $\hat{y}\not=y$  
> &emsp;&emsp;&emsp;&emsp; $\mathbf{w}\leftarrow\mathbf{w} +\eta(\mathbf{x}y-\mathbf{x}{\bar{y}})$  
> &emsp;&emsp; **end if**  
> **end while**  
> **return** $\mathbf{w}$  

### Multi-Class and Not Linearly Separable
If the data is not linearly separable, the perceptron can thrash between two or more weight settings, never converging.
In this case, an effective practical solution is to average the perceptron weights across all iterations. One possible
stopping criterion is to check whether the difference between the averaged weight vectors falls below some predefined
threshold. Another stopping criterion is to hold out a validation set, and stop training once the accuracy on the
validation set starts to decrease.

## References
Warren S. McCulloch, Walter Pitts. 1943. A Logical Calculus of the Ideas Immanent in Nervous Activity. 

Robbins, H. and S. Monro (1951). A stochastic Approximation Method. Annals of Mathematical Statistics 22, 400–407.

Bernard Widrow, and Michael A. Lehr. 1990. 30 Years of Adaptive Neural Networks: Perceptron, Madaline, and
Backpropagation. Proceedings of the IEEE, Vol. 78, No. 9.

Sergios Theodoridis and Konstantinos Koutroumbas. 2009. Pattern Recognition. Chapter 3.

Hang Li. 2011. Statistical Learning Methods. Chapter 2. (In Simplified Chinese)

{% include links.html %}
