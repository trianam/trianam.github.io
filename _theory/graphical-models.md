---
layout: post
title:  Graphical Models
date:   2019-02-21 16:00:00 +0100
author: Stefano Martina
mathjax: true
---
{% assign figchain = 1 %}
{% assign bibbishop = "Bishop2006" %}

Probabilistic graphical models are a way to express probability models
using graph. Each node represents a random variable, while an arc
denotes probabilistic relationship. The graph denomination changes
if the graph is directed or not:
* **Bayesian networks** are directed graphical models;
* **Markov random fields** are undirected graphical models.

## Bayesian Networks
To introduce Bayesan networks, consider the *probability chain rule*
where the joint distribution over variables is decomposed as product
of conditioned probabilities:

$$
\begin{equation}
	p(a,b,c) = p(c|a,b)\ p(a,b) = p(c|a,b)\ p(b|a)\ p(a)
	\label{eq:chain}
\end{equation}
$$

The corresponding graphical model of the rightest term of
\eqref{eq:chain} is depicted in
{% include ref.html label=figchain %}.
{% include image.html url="/assets/tikz/chainBayesianNetwork.svg"
label=figchain description="Bayesian network of joint probability
distribution over three variables." width="50%" %}
The arcs represents conditional probabilities, e.g. the random
variable $c$ has two incoming arcs because is conditioned by the other
two random variables. Note that the ordering of the random variables
in the leftmost term of \eqref{eq:chain} is irrelevant, thus we can
choose different graphic representation for the same model.

Bayesian networks are only *directed acyclic graphs*, thus they are
oriented graph. For each node $n$, the other nodes that have an
outgoing edge to $n$ are denoted as $parents$.

For a generic graph with $K$ nodes $$\{x_1,\dots,x_K\}$$, the joint
distribution is given by:

$$
\begin{equation*}
	p(\vec{x}) = \prod_{k=1}^{K}p(x_k|pa_k)
\end{equation*}
$$

where $\vec{x}$ is $$\{x_1, \dots, x_K\}$$ and $pa_k$ is the set of
parents of $x_k$. 

### Polynomial regression


<br>

---

# References

{% include bib.html label=bibbishop authors="Bishop, Christopher M."
year=2006 title="Pattern Recognition and Machine Learning"
venue="Springer" pages="359-422" link="https://www.microsoft.com/en-us/research/wp-content/uploads/2016/05/Bishop-PRML-sample.pdf" %}

<br>

---

# Footnotes

[^fn1]: A footnote is something like that.

