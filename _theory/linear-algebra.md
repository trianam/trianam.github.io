---
layout: post
title:  Linear Algebra
date:   2018-04-10 19:00:00 +0100
author: Stefano Martina
mathjax: true
---
{% assign deeplearning = "Goodfellow2016" %}

## Notation
### Vectors
A vector is a point in a multi-dimensional space. It can be viewed as an ordered array of coordinates respect to a base[^fn1]. A vector can be arranged in a column or a row, the former when no otherwise specified.

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
A bidimensional vector is called a matrix. Matrices are denoted by bold upper-case letters, and its elements with the same letter in italic with indices. A matrix with $n$ rows and $m$ columns $\mat{A}\in\mR^{n\times m}$ is written as:

$$
\begin{equation*}
\mat{A}=
\begin{bmatrix}
A_{1,1} & A_{1,2} & \cdots & A_{1,m} \\
A_{2,1} & \ddots & & A_{2,m}\\
\vdots & & \ddots & \vdots \\
A_{n,1} & A_{n,2} & \dots & A_{n,m}
\end{bmatrix}
\end{equation*}
$$

### Tensors
Tensors are matrix generalization to multiple dimensions. They are multidimensional arrays of numbers arranged on a regular grid. Tensors are denoted with bold upper-case sans-serif letters, and its elements with lower-case letters.

A tensor $\ten{A}\in\mR^{n\times m\times\cdots\times p}$ has $n\times m\times\cdots\times p$ elements identified by $\tenE{A}_{i,j,\dots,k}$, with $i\in[1,n]$, $j\in[1,m]$, $\dots$ , $k\in[1,p]$.

## Operations
### Transposition
Vectors and matrices can be transposed. The transposed matrix of $\mat{A}\in\mR^{n\times m}$ is the matrix $\mat{A}^T\in\mR^{m\times n}$ obtained reflecting it along the main axis. For each element:

$$
\begin{equation*}
A^T_{i,j}=A_{j,i},\quad\forall i\in[1,m],\ \forall j\in[1,n].
\end{equation*}
$$

The transpose of a vector $\vec{x}$ transform it in a row vector if it was a column vector and vice versa.

### Matrix product
It is possible to multiply two matrices $\mat{A}\in\mR^{m\times n}$ and $\mat{B}\in\mR^{n\times p}$. The result is the matrix $\mat{C}\in\mR^{m\times p}$:

$$
\begin{equation*}
\mat{C}=\mat{A}\mat{B}
\end{equation*}
$$

where each element is defined as:

$$
\begin{equation*}
C_{i,j}=\sum_{k=1}^{n}A_{i,k}B_{k,j},\quad\forall i\in[1,m],\ \forall j\in[1,p].
\end{equation*}
$$

Matrix multiplication has distributive property

$$
\begin{equation*}
\mat{A}(\mat{B}+\mat{C}) = \mat{A}\mat{B}+\mat{A}\mat{C},
\end{equation*}
$$

and associative property

$$
\begin{equation*}
\mat{A}(\mat{B}\mat{C}) = (\mat{A}\mat{B})\mat{C},
\end{equation*}
$$

but it does not have commutative property. $\mat{A}\mat{B}=\mat{B}\mat{A}$ does not always hold.

The transpose of a matrix product has the property:

$$
\begin{equation*}
(\mat{A}\mat{B})^T = \mat{B}^T\mat{A}^T.
\end{equation*}
$$


### Hadamard product
It is possible also to define an element-wise product of same-dimensionality matrices. The result of the Hadamard product of matrices $\mat{A}\in\mR^{n\times m}$ and $\mat{B}\in\mR^{n\times m}$ is the matrix $\mat{C}\in\mR^{n\times m}$:

$$
\begin{equation*}
\mat{C}=\mat{A}\odot\mat{B}
\end{equation*}
$$

where each element is defined as:

$$
\begin{equation*}
C_{i,j}=A_{i,j}B_{i,j},\quad\forall i\in[1,m],\ \forall j\in[1,p].
\end{equation*}
$$

### Vector dot product
The dot product (or scalar product) between two same-dimensionality vectors $\vec{x}\in\mR^n$ and $\vec{y}\in\mR^n$ is defined as the matrix product $\vec{x}^T\vec{y}$.

Formally it is a scalar defined as:

$$
\begin{equation*}
\vec{x}^T\vec{y}=\sum_{i=1}^{n}x_{i}y_{i}.
\end{equation*}
$$

Unlike matrix product, dot product is commutative:

$$
\begin{equation*}
\vec{x}^T\vec{y} = \vec{y}^T\vec{x}.
\end{equation*}
$$

<br>

---

## References

{% include bib.html label=deeplearning authors="Goodfellow, I.; Bengio, Y.; Courville, A." year=2016 title="Deep Learning" venue="MIT Press" %}

<br>

---

## Footnotes

[^fn1]: E.g. the canonical base with all ones.

