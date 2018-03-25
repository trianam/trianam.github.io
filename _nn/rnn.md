---
layout: post
title: "Recurrent Neural Networks (RNN)"
date:   2018-03-24 14:00:00 +0100
mathjax: true
---
{% assign figrnn = 1 %}
{% assign figrnnu = 2 %}

A *Recurrent Neural Network* (RNN) is a neural network that exibits an internal state. RNN are suitable to process sequential data, e.g. text.

{% include image.html url="/assets/tikz/rnn.svg" label=figrnn description="A RNN." width="30%" %}

In {% include ref.html label=figrnn %} is visible a compact representation of a RNN. The elaboration is performed in an iterative fashion. At each step an input $\vec{x}$ is presented to the network, together with the state at previous iteration. The state at previous iteration is represented by the black box that is a delay of one iteration.

In this formalization[^fn1] $R$ and $O$ represent two functions with shared parameters $\theta$. Those functions control the network, $R$ the internal state and $O$ the output.

In detail $\vec{x}$, $\vec{y}$ and $\vec{s}$ are sequences of vectors. We use the notation $$\vec{x}_i$$ to denote the $i$-th vector of the sequence, and $$\vec{x}_{i:j}$$ the subsequence from $i$ to $j$. If the sequence is composed from $n$ vectors, we can write in this notation: $$\vec{x} = \vec{x}_{1:n}$$.

The RNN model is espressed with:

$$
\begin{eqnarray*}
    \vec{y}_{1:n} &=& RNN^*(\vec{x}_{1:n};\vec{s}_0,\theta)\\
    \vec{y}_i &=& RNN(\vec{x}_{1:i};\vec{s}_0,\theta)\\
    \vec{y}_i &=& O(\vec{s}_i;\theta) \\
    \vec{s}_i &=& R(\vec{s}_{i-1},\vec{x}_i;\theta)
\end{eqnarray*}
$$

where $$\vec{s}_0$$ is the initialization state and $\theta$ are the parameters for the training. $R$ is a recursive function that updates the internal state based on the current input $$\vec{x}_i$$, and $O$ is the output function that elaborate the output $$\vec{y}_i$$ based on the current status.

{% include image.html url="/assets/tikz/rnnUnfolded.svg" label=figrnnu description="An unfolded RNN." width="100%" %}

It can be useful to visualize the model unfolded as in {% include ref.html label=figrnnu %} in order to better understand the concepts.

<br>

---

## References
{% include bib.html label="Goldberg2017" authors="Goldberg, Yoav" year=2017 title="Neural Network Methods for Natural Language Processing" venue="Morgan & Claypool Publishers" %}

<br>

---

## Footnotes

[^fn1]: Note that in literature you can find different notations.
