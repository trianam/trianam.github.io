---
layout: post
title:  Hyperparameters optimization
date:   2018-04-13 20:00:00 +0200
author: Stefano Martina
mathjax: true
---
{% assign figRandomSearch = 1 %}
{% assign figSmbo = 2 %}

{% assign hutter2015 = "Hutter2015" %}
{% assign bengio2000 = "Bengio2000" %}
{% assign guo2006 = "Guo2006" %}
{% assign maron1994 = "Maron1994" %}
{% assign bergstra2012 = "Bergstra2012" %}
{% assign hutter2011 = "Hutter2011" %}
{% assign snoek2012 = "Snoek2012" %}
{% assign bergstra2011 = "Bergstra2011" %}

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

{% include image.html url="/assets/randomSearch.svg" label=figRandomSearch description="Grid search versus random search in $\Lambda$ with two dimensions." width="90%" %}

This improvement is visible in {% include ref.html label=figRandomSearch %} where an example bidimensional parameter space is shown. The green area identifies an important parameter, the yellow one an unimportant one. Thus the effective dimensionality of the example is closer to one respect to two. In both grid and random search settings we use a cardinality of nine for $V\subset\Lambda$, but in the grid layout we test the important parameter only on three values instead of nine.

Recently more sophisticated *Sequential Model-based Bayesian Optimization* (SMBO) {% include cite.html label=hutter2011 %} methods have been developed. Those methods grant better results on \eqref{eq:problem} and are capable of optimize when $\Lambda$ has far more dimensions respect to other methods.

#### Sequential Model-based Bayesian Optimization (SMBO)
SMBO are generic framework for optimizing blackbox functions. It uses a probabilistic model $\model$ to model $f$ based on point evaluation and any prior. It uses $\model$ to evaluate promising inputs and update $\model$ itself.

{% include image.html url="/assets/smbo.svg" label=figSmbo description="SMBO algorithm." width="90%" %}

In {% include ref.html label=figSmbo %} is visible the algorithm that resume the optimization. In order to determine the candidate $\vec{\lambda}$ in line 3, SMBO uses an *acquisition function* $a_\model:\Lambda\rightarrow\mR$. The purpose of this function is to quantify the usefulness of chosing a configuration $\vec{\lambda}\in\Lambda$. The objective is to maximise:

$$
\begin{equation*}
\argmax_{\vec{\lambda}\in\Lambda}\ a_\model(\vec{\lambda}).
\end{equation*}
$$

The function $a_\model$ must be selected according to the tradeof between the exploitation (concentrate in regions where the performances are good) and exploration (trying unexplored regions) of the hyperparameters space $\Lambda$. The most used function is the *Expected Improvement* (EI):

$$
\begin{equation}
EI(\vec{\lambda}) = \int_{-\infty}^{f_{min}}\max\{f_{min}-f, 0\}\cdot p_\model(f\mid\vec{\lambda})\ df
\label{eq:ei}
\end{equation}
$$

that at each value $\vec{\lambda}$ integrates the improvements over $f_{min}$ accordingly to a posterior $$p_\model(f\mid\vec{\lambda})$$ over the model $\model$.

Using a gaussian posterior with mean $\mu_\vec{\lambda}$ and variance $\sigma_\vec{\lambda}^2$, \eqref{eq:ei} simplifies to:

$$
\begin{equation*}
EI(\vec{\lambda}) = \sigma_\vec{\lambda}\cdot(u\cdot\Phi(u)+\phi(u)),
\end{equation*}
$$

where $u=\frac{f_{min}-\mu_\vec{\lambda}}{\sigma_\vec{\lambda}}$ and $\phi$ and $\Phi$ are the PDF[^fn1] and CDF[^fn2] of the standard normal distribution.

The following bayesian optimization packages have been used in literature:
- **Sequential Model-based Algorithm Configuration (SMAC)** {% include cite.html label=hutter2011 %} uses random forests to model $p_\model(f\mid\vec{\lambda})$ as a gaussian distribution whose mean and variance are the empirical mean and variance over predictions of the forest's tree.
- **Spearmint** {% include cite.html label=snoek2012 %} uses a *Gaussian Process* to model $p_\model(f\mid\vec{\lambda})$.
- **Tree Parzen Estimator (TPE)** {% include cite.html label=bergstra2011 %} models $p(\vec{\lambda}\mid f)$ and $p(f)$ instead of $p(f\mid\vec{\lambda})$. That is:

  $$
  \begin{equation*}
  p(\vec{\lambda}\mid f) =
  \left\{
  \begin{array}{ll}
      l(\vec{\lambda})&\mathrm{if}\ \ f<f^*\\
      g(\vec{\lambda})&\mathrm{otherwise}
  \end{array}
  \right.
  \end{equation*}
  $$

  where $l(\vec{\lambda})$ is the PDF formed by using observations $$\{\vec{\lambda}^{(i)}\}$$ such that $$f(\vec{\lambda}^{(i)})<f^*$$ and $g(\vec{\lambda})$ using the remaining observations. $$f^*$$ is chosen to be a quantile $\gamma$ of the observed $f$ such that $p(f<f^*)=\gamma$.

Spearmint works for problems with few hyperparameters. 


<br>

---

# References

{% include bib.html label=hutter2015 authors="Hutter F., Lücke J., Schmidt-Thieme L." year=2015 title="Beyond Manual Tuning of Hyperparameters" venue="KI - Künstliche Intelligenz" pages="Volume 29, Issue 4, pp 329–337" doi="https://doi.org/10.1007/s13218-015-0381-0" %}
{% include bib.html label=bengio2000 authors="Bengio, Y" year=2000 title="Gradient-Based Optimization of Hyperparameters" venue="Neural Computation" pages="Volume 12, Issue 8, p.1889-1900" doi="https://doi.org/10.1162/089976600300015187" %}
{% include bib.html label=guo2006 authors="Guo X.C., Liang Y.C., Wu C.G., Wang C.Y." year=2006 title="PSO-Based Hyper-Parameters Selection for LS-SVM Classifiers" venue="Neural Information Processing. ICONIP 2006" doi="https://doi.org/10.1007/11893257_124" %}
{% include bib.html label=maron1994 authors="Maron O., Moore A. W." year=1994 title="Hoeffding Races: Accelerating Model Selection Search for Classification and Function Approximation" venue="Advances in Neural Information Processing Systems 6" pages="59--66" link="https://papers.nips.cc/paper/841-hoeffding-races-accelerating-model-selection-search-for-classification-and-function-approximation" %}
{% include bib.html label=bergstra2012 authors="Bergstra J, Bengio Y." year=2012 title="Random search for hyper-parameter optimization" venue="The Journal of Machine Learning Research" pages="Volume 13 Issue 1, January 2012, Pages 281-305" link="http://www.jmlr.org/papers/v13/bergstra12a.html" %}
{% include bib.html label=hutter2011 authors="Hutter F., Hoos H.H., Leyton-Brown K." year=2011 title="Sequential Model-Based Optimization for General Algorithm Configuration" venue="Learning and Intelligent Optimization. LION 2011" doi="https://doi.org/10.1007/978-3-642-25566-3_40" %}
{% include bib.html label=snoek2012 authors="Snoek J., Larochelle H., Adams R. P." year=2012 title="Practical Bayesian optimization of machine learning algorithms" venue="NIPS'12 Proceedings of the 25th International Conference on Neural Information Processing Systems - Volume 2" pages="Pages 2951-2959" link="https://papers.nips.cc/paper/4522-practical-bayesian-optimization-of-machine-learning-algorithms" %}
{% include bib.html label=bergstra2011 authors="Bergstra J., Bardenet R., Bengio Y., Kégl B." year=2011 title="Algorithms for hyper-parameter optimization" venue="NIPS'11 Proceedings of the 24th International Conference on Neural Information Processing Systems" pages="Pages 2546-2554" link="https://papers.nips.cc/paper/4443-algorithms-for-hyper-parameter-optimization" %}

<br>

---

# Footnotes
[^fn1]: Probability Density Function
[^fn2]: Cumulative Density Function


