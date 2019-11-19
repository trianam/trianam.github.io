---
layout: post
title:  PAC learning
date:   2019-11-19 09:00:00 +0100
author: Stefano Martina
mathjax: true
---

{% assign shai = "Shalev-shwartz2014" %}

## Statistical learning framework {#sec:supervisedTheory}

In supervised learning we posses a dataset $S$ of samples $\vect{x}_i$
each one labeled with $\vect{y}_i$:

$$
\begin{equation}
S=\left((\vect{x}_1,\vect{y}_1),\dots,(\vect{x}_n,\vect{y}_n)\right),
\end{equation}
$$

where each $\vect{x}_i\in\XSet$ and $\vect{y}_i\in\YSet$. Usually the
data is prepared such that $\XSet$ is a space of integer or real tensors
of certain dimensionality, and $\YSet$ is a space of integer vectors for
classification tasks and real vectors for regression tasks. In binary
classification tasks $\YSet={0,1}$, in multilabel classification
$\YSet={0,1}^k$ with $k$ possible labels, and in multiclass
classification $\YSet={0,1}^k$ with only one value equal to $1$ and the
rest $0$. The samples $\vect{x}_i$ of a dataset must be drawn from the
same unknown distribution $\vect{x}_i\sim\dist{D}$. The relationship
between the samples and the labels is defined by an unknown labeling
function $f:\XSet\rightarrow\YSet$ such that for each sample
$\vect{y}_i=f(\vect{x}_i)$.

A learning algorithm receives as input a training set $S$, and should
output a predictor $h_S:\XSet\rightarrow\YSet$ that minimize the
*prediction error*. For a generic predictor $h$, the prediction error
is: 

$$
\begin{equation}
\label{predictionError}
  L_{\dist{D},f}(h)\equiv\prob_{\vect{x}\sim\dist{D}}[h(\vect{x})\neq
  f(\vect{x})]\equiv\dist{D}(\{\vect{x}:h(\vect{x})\neq
  f(\vect{x})\}),
\end{equation}$$

where for $A\subset\XSet$, the probability
$\dist{D}$ assign a likelihood $\dist{D}(A)$ of observing a value
$\vect{x}\in A$. Given that both $\dist{D}$ and $f$ are unknown, to find
$h_S$ the learning algorithm minimizes the *empirical prediction error*
(or empirical risk): 

$$
\begin{equation}
\label{empiricalError}
  L_S(h)\equiv\frac{|i\in\{1,\dots,n\}:h(\vect{x}_i)\neq\vect{y}_i|}{n}.
\end{equation}$$

This learning paradigm of finding $h_S$ is called Empirical Risk Minimization (ERM).

ERM rule might lead
to *overfitting* if not restricted. A predictor can performs well over
the training set $S$ --- having a low empirical error --- but it can
performs badly over the entire distribution $\dist{D}$ with an high
prediction error. A solution is to apply ERM over a restricted search space. We denote
with $\HSet$ the *hypothesis class* of the possible predictors
$h\in{HSet}:\XSet\rightarrow\YSet$. For a given class $\HSet$ and a
training sample $S$, the $ERM_\HSet$ learner uses the
ERM rule to choose
a predictor $h_S\in\HSet$ with the lowest possible empirical error over
$S$: 

$$
\begin{equation}
h_S=ERM_\HSet(S)\in \argmin_{h\in\HSet} L_S(h).
\end{equation}
$$

We induce an *inductive bias* introducing restrictions over $\HSet$.
Intuitively, choosing a more restricted hypothesis class better protect
us against overfitting but at the same time might cause a stronger bias.

## Finite hypothesis classes

The simplest kind of restriction on $\HSet$ is imposing an upper bound
on its dimension. We make the two following assumptions:

(Realizability
assumption). $$\exists h^*\in\HSet : L_{(\dist{D},f)}(h^*)=0$$.

(i.i.d. examples).
$S\sim\dist{D}^m$, with $m=|S|$ and $\dist{D}^m$ the probability over
$m$-tuples induced by applying $\dist{D}$ to pick each element of the
tuple independently from the others elements of the same tuple.

$L_{(\dist{D},f)}(h_S)$ depends on the choice of $S$, that is a random
variable. To address the probability to sample $S$ such that the
prediction error is not too large, we denote with $\delta$ the
probability of getting a nonrepresentative sample, and $(1-\delta)$ the
*confidence parameter* of the prediction. Furthermore we introduce the
*accuracy parameter* $\varepsilon$ of the quality of the prediction.
$L_{(\dist{D},f)}(h_S)>\varepsilon$ represents a failure of the learner.
We are interested in upper bounding the probability to sample $m$-tuple
that lead to the learner failure. If
$$S|_x=(\vect{x}_1,\dots,\vect{x}_m)$$ are instances of the training set,
we want to upper bound
$$\dist{D}^m(\{S|_x:L_{(\dist{D},f)}(h_S)>\varepsilon\}).$$ It is
possible to prove that for a finite hypothesis class $\HSet$:
$$\dist{D}^m(\{S|_x:L_{(\dist{D},f)}(h_S)>\varepsilon\}) \leq
  |\HSet|e^{-\varepsilon m},$$ and for $m$ an integer that satisfies
$$m\geq\frac{\log(|\HSet|/\delta)}{\varepsilon},$$ then for any $f$ and
$\dist{D}$, with probability at least $1-\sigma$, for every $h_S$ holds:
$$L_{(\dist{D},f)}(h_S)\leq\varepsilon.$$ This means that for
sufficiently large $m$, the $ERM_\HSet$ rule over a finite hypothesis
class will be probably (with confidence $1-\delta$) approximately (up to
an error $\varepsilon$) correct.

## PAC learning

A
hypothesis class $\HSet$ is Probably Approximately Correct (PAC) learnable if there exist a function
$m_\HSet:(0,1)^2\rightarrow\NSet$ and a learning algorithm with the
following property: for every $\varepsilon,\sigma\in(0,1)$, for every
$\dist{D}$ and $f$, if
realizability assumption holds, then when running the algorithm on
$m\geq m_\HSet(\varepsilon,\sigma)$ i.i.d. examples, it returns a hypothesis $h$
such that $L_{(\dist{D},f)}(h)\leq\varepsilon$, with probability of at
least $1-\sigma$ over the choice of the examples.

The function $m_\HSet$ determines the *sample complexity* of learning
$\HSet$, that is the number of samples to guarantee a
PAC solution. The
finite hypothesis class of is PAC learnable with sample complexity

$$
\begin{equation}
m_\HSet(\varepsilon,\delta)\leq\left\lceil\frac{\log(|\HSet|/\delta}{\varepsilon}\right\rceil
\end{equation}
$$

## Agnostic PAC learning

To develop a more realistic model, we relax the
realizability assumption and we substitute the notion of $\dist{D}$
over $\XSet$ and $f:\XSet\rightarrow\YSet$ with a single data generating
distribution. From now, $\dist{D}$ is defined as a probability
distribution over $\XSet\times\YSet$, and can be seen as being composed
by a *marginal distribution* $\dist{D}_x$ over $\XSet$ and a
*conditional probability* $\dist{D}((\vect{x},\vect{y})|\vect{x})$ over
label for each example. The true error
\eqref{predictionError} is redefined as:

$$
\begin{equation}
L_{\dist{D},f}(h)\equiv\prob_{(\vect{x},\vect{y})\sim\dist{D}}[h(\vect{x})\neq
  f(\vect{x})]\equiv\dist{D}(\{(\vect{x},\vect{y}):h(\vect{x})\neq
  f(\vect{x})\}),
\end{equation}
$$ 
  
while the empirical error
\eqref{empiricalError} remains the same.

We introduce the concept of *loss function* as a function
$\loss:\HSet\times(\XSet\times\YSet)\rightarrow\RSet^+$, and the true
and empirical risks are defined in term of $\loss$ as: 

$$
\begin{equation}
\begin{aligned}
  L_\dist{D}(h)&\equiv\expect_{(\vect{x},\vect{y})\sim\dist{D}}[\loss(h,(\vect{x},\vect{y}))],\\
  L_S(h)&\equiv\frac{1}{m}\sum_{i=2}^m\loss(h,(\vect{x}_i,\vect{y}_i))].\end{aligned}
\end{equation}
$$

Two common losses are:

**0-1 loss**

$$
\begin{equation}
\loss_{0-1}(h,(\vect{x},\vect{y}))\equiv\begin{cases}
    0\quad\mathrm{if}\ h(\vect{x}) = \vect{y},\\
    1\quad\mathrm{if}\ h(\vect{x}) \neq \vect{y}.
  \end{cases}
\end{equation}
$$

**Square loss**

$$
\begin{equation}
\loss_{sq}(h,(\vect{x},\vect{y}))\equiv(h(\vect{x})-\vect{y})^2.
\end{equation}
$$

Is it possible now to define the concept of agnostic
PAC learnability.

A hypothesis class $\HSet$ is agnostic
PAC learnable,
respect $(\XSet\times\YSet)$ and $\loss$, if there exist a function
$m_\HSet:(0,1)^2\rightarrow\NSet$ and a learning algorithm with the
following property: for every $\varepsilon,\delta\in(0,1)$ and for every
$\dist{D}$, when running the algorithm on $m\geq
m_\HSet(\varepsilon,\delta)$ i.i.d. examples, it returns $h$ such that
$L_\dist{D}(h)\leq\min_{h'\in\HSet}L_\dist{D}(h')+\varepsilon$.

Contrarily to PAC learnability,
Agnostic PAC learnability cannot guarantee an arbitrarily
small error. It still guarantee that a learner can have success if its
error is not too much larger than the one of the best achievable
predictor for class $\HSet$.

ERM learning
paradigm works by finding an hypothesis that minimize the empirical
risk. This means that an $h$ that minimizes the empirical risk needs to
be a good true risk minimizer also. We need that uniformly over all
hypothesis, the empirical risk needs to be close to the real risk. To
asses this we introduce the concept of *$\varepsilon$-representative
sample*:

($\varepsilon$-representative sample). A training set $S$ is called
$\varepsilon$-representative with regard to $\HSet$, $\loss$, and
$\dist{D}$, if

$$
\begin{equation}
\forall h\in\HSet,\quad |L_S(h)-L_\dist{D}(h)|\leq\varepsilon.
\end{equation}
$$

It is possible to prove that if the sample is
$\frac{\varepsilon}{2}$-representative, then the
ERM learning rule
is guaranteed to return a good hypothesis. Formally:

Assuming $S$ is
$\frac{\varepsilon}{2}$-representative, then any output
$ERM_\HSet(S)=h_S\in\argmin_{h\in\HSet}L_S(h)$ satisfy:

$$
\begin{equation}
L_\dist{D}(h_S)\leq\min_{h\in\HSet}L_\dist{D}(h)+\varepsilon.
\end{equation}
$$

To ensure that the ERM rule is an agnostic
PAC learner, it
suffices to show that with probability of at least $1-\delta$ over the
choice of $S$, it will be an $\varepsilon$-representative training set.
Formally, given the property:

(Uniform Convergence). $\HSet$ has the uniform convergence property if
there exists a function $m_\HSet^{UC}:(0,1)^2\rightarrow\NSet$ such that
for every $\varepsilon,\delta\in(0,1)$ and for every $\dist{D}$, if $S$
is a sample of $m\geq
  m_\HSet^{UC}(\varepsilon,\delta)$ examples drawn
i.i.d. from
$\dist{D}$, then $S$ is $\varepsilon$-representative with probability of
at least $1-\delta$.

We have the corollary:

If a class $\HSet$ has the uniform convergence property, then the class
is agnostically PAC
learnable with sample complexity $m_\HSet(\varepsilon,\delta)\leq
  m_\HSet^{UC}(\varepsilon/2,\delta)$. In that case the $ERM_\HSet$
paradigm is a successful agnostic PAC learner for $\HSet$.

It is possible to prove (we skip the proof here) that the uniform
convergence holds for a finite hypothesis class, and then that every
finite hypothesis class is agnostic PAC learnable using ERM algorithm with sample complexity

$$
\begin{equation}
m_\HSet(\varepsilon, \delta)\leq
  m_\HSet^{UC}\left(\frac{\varepsilon}{2},\delta\right)\leq\left\lceil\frac{2\log\left(\frac{2|\HSet|}{\delta}\right)}{\varepsilon^2}\right\rceil.
\end{equation}
  $$

## Bias-complexity trade-off

Choosing a hypothesis class $\HSet$ represents a prior knowledge related
to the data distribution $\dist{D}$. One can argue if this prior
knowledge is really necessary, or if it is possible to have a learning
algorithm $A$ and a size $m$ such that for every $\dist{D}$, if $A$
receives $m$ i.i.d.
samples from $\dist{D}$, it outputs a predictor $h$ that have a low risk
with high chance. The *no-free-lunch* theorem proves that having such a
universal learner is impossible:

One can avoid the limits imposed by the no-free-lunch theorem using the
prior knowledge of the learning task to restrict the hypothesis class
$\HSet$. To choose a good hypothesis class we need to set a large enough
class such that it includes the hypothesis with no error (for
PAC learnability),
or at least that the smallest error achievable by a hypothesis from the
class is small enough (for agnostic PAC learnability). On the other hand we
cannot choose the richest class. We can decompose the error of an
$ERM_\HSet$ hypothesis $h_S$ into the component:
$$L_\dist{D}(h_S)=\varepsilon_{app}+\varepsilon_{est}.$$ where
$$\varepsilon_{app}=\min_{h\in\HSet}L_\dist{D}(h)$$ is the
*approximation error* that measures how much inductive bias we introduce
restricting $\HSet$ and do not depends on the sample size.
$$\varepsilon_{est}=L_\dist{D}(h_S)-\varepsilon_{app}$$ is the
*estimation error* that depends on the training set size and on the
complexity of $\HSet$.

To minimize the total risk, we face a *bias-complexity tradeoff*.
Choosing $\HSet$ to be rich, decreases the approximation error, but
increases the estimation error because it may lead to overfitting.
Choosing a small $\HSet$ reduces the estimation error, but can increases
the approximation error, in other words it might lead to *underfitting*.

## VC-dimension

The finitess of $\HSet$ is a sufficient condition for learnability, but
it is not a necessary condition. A property called Vapnik-Chervonenkis dimension (VC-dimension)
 gives a correct
characterization of learnability. Considering only binary classifiers
where $\YSet=\{0,1\}$, to define VC-dimension, we must define before the concept of
*class restriction* and *shattering*:

(Restriction of $\HSet$ to $C$). $\HSet$ is a class of function
$h:\XSet\rightarrow\{0,1\}$, and $C=\{c_1,\dots,c_m\}\subset\XSet$. The
restriction of $\HSet$ to $C$ is the set of function from $C$ to
$\{0,1\}$ that can be derived from $\HSet$:
$$\HSet_C=\{(h(c_1),\dots,h(c_m)):h\in\HSet\},$$ where each function is
represented as a vector in $\{0,1\}^{|C|}$.

(Shattering). A hypothesis class $\HSet$ shatters a finite set
$C\subset\XSet$, if $\HSet_C$ is the set of all functions from $C$ to
$\{0,1\}$: 

$$
\begin{equation}
|\HSet_C|=2^{|C|}.
\end{equation}
$$

Relating to the free-lunch-theorem, whenever some set $C$ is shattered
by $\HSet$, an adversary can construct a distribution over $C$ based on
any function from $C$ to $\{0,1\}$ maintaining the realizability
assumption. Thus, for any learning algorithm $A$ there exist a
distribution $\dist{D}$ and a predictor $h$ (both constructed from an
adversary) such that $L_D(h)=0$, but with a sufficiently high
probability over the choice of $S$, $L_\dist{D}(A(S))\geq\varepsilon$.
The VC-dimension is defined:

(VC-dimension). The
VC-dimension $VCdim(\HSet)$
of an hypothesis class $\HSet$, is the maximal size of a set
$C\subset\XSet$ that can be shattered by $\HSet$. If $\HSet$ can shatter
arbitrarily large sets, then $VCdim(\HSet)=\infty$.

If a certain hypothesis class $\HSet$ has infinite
VC-dimension, then $\HSet$
is not PAC
learnable. Conversely, a finite VC-dimension guarantees learnability. To prove that
$VCdim(\HSet)=d$ is finite, we need to show

- $$\exists C$$ such that $$\rvert C\rvert=d$$ and it is shattered by $$\HSet$$;

- $$\forall C$$ such that $$\rvert C\rvert=d+1$$, $C$ is not shattered by $$\HSet$$.

## Structural Risk Minimization (SRM)

Until now the prior knowledge was encoded specifying a hypothesis class
$\HSet$ that is believed to include a good predictor for the learning
task. Another way to express the prior knowledge is by specifying
preferences over hypotheses within $\HSet$. The
Structural Risk Minimization (SRM) implement this
assuming $\HSet=\bigcup_{n\in\NSet}\HSet_n$ and defining a weight
function $w:\NSet\rightarrow[0,1]$ that assign a preference for each
class $\HSet_n$. SRM rule follows a *bound minimization*
approach. The goal is to find a hypothesis that minimizes a certain
upper bound on the true risk.

<br>

---

# References

{% include bib.html label=shai authors="Shalev-Shwartz, S., & Ben-David, S." year=2014 title="Understanding machine learning: From theory to algorithms" venue="Cambridge university press" pages="1--72" link="https://www.cse.huji.ac.il/~shais/UnderstandingMachineLearning/understanding-machine-learning-theory-algorithms.pdf" %}

<br>

---
