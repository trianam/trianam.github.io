---
layout: post
title: "RNN generators"
date:   2018-03-24 14:00:00 +0100
mathjax: true
---
{% assign figrnn = 1 %}

A *Recurrent Neural Network* (RNN) is a neural network that exibits an internal state. RNN are suitable to process sequential data, e.g. text.

{% include image.html url="/assets/tikz/rnn.svg" label=figrnn description="A RNN." width="50%" %}

In {% include ref.html label=figrnn %} is visible a compact representation of a RNN. The elaboration is performed in an iterative fashion. At each step an input $\vec{x}$ is presented to the network, together with the state at previous iteration. The state at previous iteration is represented by the black box that is a delay of one iteration.
