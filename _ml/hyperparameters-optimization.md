---
layout: post
title:  Hyperparameters optimization
date:   2018-04-13 20:00:00 +0200
author: Stefano Martina
mathjax: true
---
{% assign figRandomSearch = 1 %}

{% assign hutter2015 = "Hutter2015" %}
{% assign bengio2000 = "Bengio2000" %}
{% assign guo2006 = "Guo2006" %}
{% assign maron1994 = "Maron1994" %}
{% assign bergstra2012 = "Bergstra2012" %}

The construction of machine learning models models demands big effort on hyperparameters tuning. Usually the hyperparameters are manually engineered and are the product of time-consuming experimentations. Hyperparameters choose is really important in model performance.

We can use two methods in order to overcome this manual intervention:
1. automatic hyperparameters optimization;
2. development of models with reduced hyperparameters number.

## Hyperparameters optimization

### Problem definition
Given a machine learning algorithm $A$ with hyperparameters $\lambda_1,\dots,\lambda_n$ in $\Lambda_1,\dots,\Lambda_n$, the hyperparameters space is defined as $\Lambda=\Lambda_1\times\cdots\times\Lambda_n$. For each hyperparameter setting $\vec{\lambda}\in\Lambda$, we use $A_\vec{\lambda}$ to indicate $A$ using this setting. We use $\loss(A_\vec{\lambda}, \data_{train}, \data_{valid})$ to denote the validation loss of $A_{\lambda}$ using $\data_{valid}$ validation set, when is trained using $\data_{train}$ training set.

Using $k$-fold cross validation we divide the dataset in $k$ splits $\data_{train}^{(1)},\dots,\data_{train}^{(k)}$ and $\data_{valid}^{(1)},\dots,\data_{valid}^{(k)}$. The hyperparameter optimization problem is:

$$
\begin{equation}
\argmin_{\vec{\lambda}\in\Lambda}\ f(\vec{\lambda}),
\label{eq:problem}
\end{equation}
$$

where $f$ is the loss:

$$
\begin{equation}
f(\vec{\lambda}) = \frac{1}{k}\sum_{i=1}^k\loss(A_{\vec{\lambda}},\data_{train}^{(i)},\data_{valid}^{(i)}).
\label{eq:loss}
\end{equation}
$$

Hyperparameters can be continuous, integer or categorical. An hyperparameter $\lambda_i$ is *conditional* on another hyperparameter $\lambda_j$ if $\lambda_i$ is active only when $\lambda_j$ takes values in a specific set $V_i(j)\subset\Lambda_j$.

### Review
Historically the standard in hyperparameters optimization was grid search. This method consists in:
1. define a grid $V\subset\Lambda$ on hyperparameters space, respect to certain ranges;
2. evaluate \eqref{eq:loss} for all $\vec{\lambda}\in V$;
3. take the $\vec{\lambda}$ with the smaller value of loss.

A more advanced approach consists in starting with a coarse grid and refining the best result with a fine one around it.

other approaches include:
* gradient search when some differentiability and continuity conditions of the training criterion are satisfied {% include cite.html label=bengio2000 %};
* evolutionary computation techniques {% include cite.html label=guo2006 %};
* racing algorithms {% include cite.html label=maron1994 %}.

It has been proven that a random search is more effective than grid search on models with low effective dimensionality {% include cite.html label=bergstra2012 %}.

{% include image.html url="/assets/randomSearch.svg" label=figRandomSearch description="Grid search versus random search on $\Lambda$ with two dimensions." width="90%" %}

This improvement is visible in {% include ref.html label=figRandomSearch %} where an example bidimensional parameter space is shown. The green area identifies an important parameter, the yellow one an unimportant one. Thus the effective dimensionality of the example is closer to one respect to two. In both grid and random search settings we use a cardinality of nine for $V\subset\Lambda$, but in the grid layout we test the important parameter only on three values instead of nine.

Recently more sophisticated *Sequential Model-based Bayesian Optimization* (SMBO) methods have been developed. Those methods grant better results on \eqref{eq:problem} and are capable of optimize when $\Lambda$ has far more dimensions respect to other methods.

#### Sequential Model-based Bayesian Optimization (SMBO)

<br>

---

## References

{% include bib.html label=hutter2015 authors="Hutter F., Lücke J., Schmidt-Thieme L." year=2015 title="Beyond Manual Tuning of Hyperparameters" venue="KI - Künstliche Intelligenz" pages="Volume 29, Issue 4, pp 329–337" doi="https://doi.org/10.1007/s13218-015-0381-0" %}
{% include bib.html label=bengio2000 authors="Bengio, Y" year=2000 title="Gradient-Based Optimization of Hyperparameters" venue="Neural Computation" pages="Volume 12, Issue 8, p.1889-1900" doi="https://doi.org/10.1162/089976600300015187" %}
{% include bib.html label=guo2006 authors="Guo X.C., Liang Y.C., Wu C.G., Wang C.Y." year=2006 title="PSO-Based Hyper-Parameters Selection for LS-SVM Classifiers" venue="Neural Information Processing. ICONIP 2006" doi="https://doi.org/10.1007/11893257_124" %}
{% include bib.html label=maron1994 authors="Maron O., Moore A. W." year=1994 title="Hoeffding Races: Accelerating Model Selection Search for Classification and Function Approximation" venue="Advances in Neural Information Processing Systems 6" pages="59--66" link="https://papers.nips.cc/paper/841-hoeffding-races-accelerating-model-selection-search-for-classification-and-function-approximation" %}
{% include bib.html label=bergstra2012 authors="Bergstra J, Bengio Y." year=2012 title="Random search for hyper-parameter optimization" venue="The Journal of Machine Learning Research" pages="Volume 13 Issue 1, January 2012, Pages 281-305" link="http://www.jmlr.org/papers/v13/bergstra12a.html" %}

<br>

---

## Footnotes



