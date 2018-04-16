---
layout: post
title:  Hyperparameters optimization
date:   2018-04-13 20:00:00 +0200
author: Stefano Martina
mathjax: true
---

The construction of machine learning models models demands big effort on hyperparameters tuning. Usually the hyperparameters are manually engineered and are the product of time-consuming experimentations. Hyperparameters choose is really important in model performance.

We can use two methods in order to overcome this manual intervention:
1. automatic hyperparameters optimization;
2. development of models with reduced hyperparameters number.

## Hyperparameters optimization

### Problem definition
Given a machine learning algorithm $A$ with hyperparameters $\lambda_1,\dots,\lambda_n$ in $\Lambda_1,\dots,\Lambda_n$, the hyperparameters space is defined as $\Lambda=\Lambda_1\times\cdots\times\Lambda_n$. For each hyperparameter setting $\vec{\lambda}\in\Lambda$, we use $A_\vec{\lambda}$ to indicate $A$ using this setting. We use $\loss(A_\vec{\lambda}, \data_{train}, \data_{valid})$ to denote the validation loss of $A_{\lambda}$ using $\data_{valid}$ validation set, when is trained using $\data_{train}$ training set.

Using $k$-fold cross validation we divide the dataset in $k$ splits $\data_{train}^{(1)},\dots,\data_{train}^{(k)}$ and $\data_{valid}^{(1)},\dots,\data_{valid}^{(k)}$. The hyperparameter optimization problem is:

$$
\begin{equation*}
\argmin_{\vec{\lambda}}\ f(\vec{\lambda}),
\end{equation*}
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
* define a grid $V\subset\Lambda$ on hyperparameters space, respect to certain ranges;
* evaluate \eqref{eq:loss} for all $\vec{\lambda}\in V$;
* take the $\vec{\lambda}$ with the smaller value of loss.

<br>

---

## References

{% include bib.html label="Hutter2015" authors="Hutter, F.; Lücke, J.; Schmidt-Thieme, L." year=2015 title="Beyond Manual Tuning of Hyperparameters" venue="KI - Künstliche Intelligenz" pages="Volume 29, Issue 4, pp 329–337" doi="https://doi.org/10.1007/s13218-015-0381-0" %}

<br>

---

## Footnotes



