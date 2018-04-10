---
layout: post
title: "Linear Algebra"
date:   2018-04-10 19:00:00 +0100
mathjax: true
---
{% assign deeplearning = "Goodfellow2016" %}

## Notation
### Vectors
A vector is a point in a multi-dimensional space . It can be viewed as an ordered array of coordinates respect to a base[^fn1]. A vector can be arranged in a column or a row, the former when no otherwise specified.

Vectors are usually denoted by bold lower-case letters, and its elements with the same letter in italics with indices.
A vector of a $n$-dimensional space $\vec{x}\in \mR^n$ is written as:

$$
\begin{equation*}
\vec{x}=\left[
\begin{array}{c}
x_1\\
x_2\\
\vdots\\
x_n\\
\end{array}
\right]
\end{equation*}
$$

where $x_1$ is the coordinate of the first dimension, $x_2$ of the second one, and so on.

### Matrices
A bidimensional vector is called a matrix. Matrices are denoted by bold upper-case letters, and its elements with the same letter in italic with indices. A matrix with $m$ rows and $n$ columns $\mat{A}\in\mR^{m\times n}$ is written as:

$$
\begin{equation*}
\mat{A}=
\begin{bmatrix}
A_{1,1} & A_{1,2} & \cdots & A_{1,n} \\
A_{2,1} & \ddots & & A_{2,n}\\
\vdots & & \ddots & \vdots \\
A_{m,1} & A_{m,2} & \dots & A_{m,n}
\end{bmatrix}
\end{equation*}
$$

### Tensors
Tensors are matrix generalization to multiple dimensions. They are multidimensional arrays of numbers arranged on a regular grid. Tensors are denoted with bold upper-case sans-serif letters, and its elements with lower-case letters.

A tensor $\ten{A}\in\mR^{m\times n\times\cdots\times q}$ has $m\times n\times\cdots\times q$ elements identified by $\tenE{A}_{j,k,\dots,r}$, with $j\in[1,m]$, $k\in[1,n]$, $\dots$ , $r\in[1,q]$.

<br>

---

## References

{% include bib.html label=deeplearning authors="Goodfellow, I.; Bengio, Y.; Courville, A." year=2016 title="Deep Learning" venue="MIT Press" %}

<br>

---

## Footnotes

[^fn1]: E.g. the canonical base with all ones.

