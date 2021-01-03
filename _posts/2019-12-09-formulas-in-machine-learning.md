---
title: Formulas In Machine Learning [more to come]
permalink: formulas_in_machine_learning.html
date: 2019-12-09
tags: [machine_learning]
published: false
---

Despite machine learning has always been a fast-developing research field with ever improving state-of-the-art results,
a couple of fundamental equalities and inequalities concerning machine learning remain unchanged.

## Data-Processing Inequality
This inequality is borrowed from [Elements of Information Theory](#references). If $X\rightarrow Y\rightarrow Z forms a
Markov chain, then $I(X,Z)\leq I(X,Y)$. The data-processing inequality can be used to show that no clever manipulation
of the data can improve the inferences that can be made from the data.

## Bayes Error
From the Bayes perspective, the Bayes decision rule classifies a sample $\mathbf{x}$ by evaluating the posterior
probabilities $p(w_i|\mathbf{x})$ and assigning it the most probably class corresponding to
$\arg\max_{j}p(w_j|\mathbf{x})$ as a prediction. Due to overlapping class conditional densities, this error is an
inherent property of the problem and can never be eliminated. Theoretically, the Bayes error is proven to achieve the
minimum error rate for any classification tasks. Suppose $p(w_i|\mathbf{x})$ denotes the true posterior probabilities,
and our decision rule randomly classifies $\mathbf{x}$ to $w_i$ with probabilities $q(w_i|\mathbf{x})$. For general
multi-class tasks, it is more comfortable to work with the "correct rate", not the error rate:
$$
    {p_{\text{correct}}=\sum_{i=1}^Mp(w_i|\mathbf{x})q(w_i|\mathbf{x}),}
$$
where the maximum is achieved if and only if
$$
    {q(w_i|\mathbf{x})=1,\ w_i=\arg\max_{j}p(w_j|\mathbf{x}),}\\
    {q(w_i|\mathbf{x})=0,\ \text{otherwise}.}
$$
That is to say, the maximum "correct rate", i.e. the minimum error rate, is achieved only when our decision rule
coincides with the Bayes decision rule.

<img src="{{ "images/20191209-2.png" }}" alt="bayes error"/>
_Figure 1: By courtesy of [Pattern Classification](#references) Figure 2.17._

Obviously, the Bayes decision principle is equivalent to evaluating $\arg\max_{i}p(\mathbf{x}|w_i)p(w_i)$. This figure
illustrates components of the probability of error for non-optimal and optimal decision points, $x^*$ and $x_B$,
respectively. The pink area corresponds to the probability of errors for deciding $x\in w_1$ when in fact $x\in w_2$;
the gray area represents the converse. If the decision boundary is instead at the point of equal joint probabilities,
$x_B$, then this reducible error is eliminated and the total shaded area is the minimum possible. This is the Bayes
decision rule and gives the Bayes error rate.

## Mapping to Low-Dimensional Space Never Improves Bayes Error
We can envision this inequality as the converse of the coin, i.e. the Data-Processing inequality. Suppose we know class
conditional distributions $p(\mathbf{x}|w_i)$ in a $d$-dimensional feature vector space, and their priors $p(w_i)$. We
will show that the true error (<I>i.e.</I>, Bayes error) cannot decrease by projecting the feature vectors onto a lower
dimensional space before classifying them. To this end, we should be advised that a direct implication of mapping to a
lower dimensional space could be that multiple vectors $\mathbf{x}_1,\ldots,\mathbf{x}_n$ in the original space
correspond to the same mapped vector $\mathbf{y}$ in the lower dimensional space. The "correct rate" (according to the
Bayes rule) is given by:
$$
    {\sum_{j=1}^np_{\text{correct}}(\mathbf{x}_j)=\sum_{j=1}^n\max_{i}p(w_i|\mathbf{x}_j)p(\mathbf{x}_j) =\sum_{j=1}^n\max_{i}p(\mathbf{x}_j|w_i)p(w_i).}
$$
Recall the fact that $\mathbf{y}(\mathbf{x}_1)=\ldots=\mathbf{y}(\mathbf{x}_n)$, the correct rate in the lower
dimensional space is given by:
$$
    {p_{\text{correct}}(\mathbf{y})=\max_{i}p(w_i|\mathbf{y}(\mathbf{x}_j))p(\mathbf{y}(\mathbf{x}_j))
    =\max_{i}p(\mathbf{y}(\mathbf{x}_j)|w_i)p(w_i)=\max_{i}\sum_{j=1}^np(\mathbf{x}_j|w_i)p(w_i)
    \leq \sum_{j=1}^n\max_{i}p(\mathbf{x}_j|w_i)p(w_i)=\sum_{j=1}^np_{\text{correct}}(\mathbf{x}_j).}
$$
Because the "correct rate" is $\int_{\mathbf{X}}p_{\text{correct}}(\mathbf{x})d\mathbf{x}$ for the original space and
$\int_{\mathbf{Y}}p_{\text{correct}}(\mathbf{y})d\mathbf{y}$ for the mapped space, respectively, we are actually
integrating throughout the entire spaces. This completes the proof. Despite this fact, in an actual machine learning
application we might not want to include an arbitrarily high number of feature dimensions. One reason that accounts for
this might be "the curse of dimensionality". This inequality is left an exercise in Duda and Hart's
[Pattern Classification](#references).

## Chernoff Bound
The Chernoff bound for the two-class Bayes error rate is derived using the following inequality:
$$
    {\min[a,b]\leq a^{\beta}b^{1-\beta}\quad \text{for $a,b\geq 0$ and $0\leq \beta \leq 1$}.}
$$
We apply this inequality to the equation
$$
    {p_{\text{CB}}=\int\min\{p(w_1|\mathbf{x}),p(w_2|\mathbf{x})\}p(\mathbf{x})d\mathbf{x},}
$$
and get the upper bound
$$
    {p_{\text{CB}}\leq p(w_1)^\beta p(w_2)^{1-\beta}\int p(\mathbf{x}|w_1)^\beta p(\mathbf{x}|w_2)^{1-\beta}d\mathbf{x}.}
$$
The minimum upper bound can be computed by minimizing $p_{\text{CB}}$ with respect to $\beta$. Note especially that this
integral is done over all feature space, and we do not need to impose integration limits corresponding to decision
boundaries. Fortunately, for Gaussian class conditional probabilities $p(\mathbf{x}|w_i)$ in the two-class case, the
Chernoff bound is given in closed form:
$$
    {p_{\text{CB}}=\sqrt{p(w_1)p(w_2)}\exp(-B(\beta)),}
$$
where
$$
    {B(\beta)=\frac{\beta(1-\beta)}{2}(\mu_1-\mu_2)^t\big(\beta\Sigma_1+(1-\beta)\Sigma_2)^{-1}(\mu_1-\mu_2)
    +\frac{1}{2}\ln\lvert\frac{\beta\Sigma_1+(1-\beta)\Sigma_2}{|\Sigma_1|^{\beta}|\Sigma_2|^{1-\beta}}\rvert.}
$$

## Bhattacharyya Bound
While the precise value of the optimal $\beta$ depends upon the parameters of the distributions and the prior
probabilities, a computationally simpler, but slightly less tight bound can be derived by merely using the results for
$\beta=1/2$. This result is the so-called Bhattacharyya bound on the Bayes error, where the bound has the form:
$$
    {p_{\text{error}}\leq p(w_1)^{1/2} p(w_2)^{1/2}\int \sqrt{p(\mathbf{x}|w_1)p(\mathbf{x}|w_2)}d\mathbf{x}.}
$$
Fortunately, for Gaussian class conditional probabilities $p(\mathbf{x}|w_i)$ in the two-class case, the Chernoff bound
is given in closed form:
$$
    {p_{\text{BB}}=\sqrt{p(w_1)p(w_2)}\exp(-B),}
$$
where
$$
    {B=\frac{1}{8}(\mu_1-\mu_2)^t\big(\frac{\Sigma_1+\Sigma_2}{2})^{-1}(\mu_1-\mu_2)
    +\frac{1}{2}\ln\lvert\frac{(\Sigma_1+\Sigma_2)/2}{\sqrt{|\Sigma_1||\Sigma_2|}}\rvert.}
$$
The term $B$ is known as the Bhattacharyya distance and is a class separability measure, which corresponds to the
optimum Chernoff bound for equal covariance matrices, that is, when $\Sigma_1=\Sigma_2$.

<img src="{{ "images/20191209-3.png" }}" alt="two bounds"/>
_Figure 2: By courtesy of [Pattern Classification](#references) Figure 2.18._

The Chernoff and Bhatacharyya bounds may still be used even if the underlying distributions are not Gaussian. However,
for distributions that deviate markedly from a Gaussian, the bounds will not be informative.

## References
Cover, Thomas M. and Thomas, Joy A. 2006. Elements of Information Theory. p60.

Duda, Richard O. and Hart, Peter E. and Stork, David G. 2001. Pattern Classification. p82.

{% include links.html %}
