---
layout: post
title: "Test!"
date:   2018-03-22 17:30:00 +0100
mathjax: true
---

\begin{equation}
	\frac{1}{e^x} \tag{1}
	\label{eq:test}
\end{equation}

In equation \eqref{eq:test} is visible a test, it is also possible to
write inline formulas like $y=x^2$. Note that is also possible to use
footnotes[^fn1] in the text. To quote some pubblication write
something like [Huang2001](#huang2001). The previous id link is created automatically with markdown header for Huang2001.

I add some other stuff just to see if the reference link is working.
I need some space to fill.
{% include image.html url="/assets/theory/1-test-RNN-rolled.png" description="A testing image." width="20%" %}

It is possible also to include vectorial images
{% include image.html url="/assets/theory/1-test-circle.svg" description="A testing svg image." width="20%" %}

If we omit the description what appens?
{% include image.html url="/assets/theory/1-test-circle.svg" description="" width="20%" %}

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

<br>

---

## References

### Huang2001:
E, W. & Huang, Z., 2001. Matching Conditions in Atomistic-Continuum Modeling of Materials. _arXiv.org_, (13), p.135501. Available at: [http://arxiv.org/abs/cond-mat/0106615v1](http://arxiv.org/abs/cond-mat/0106615v1).

<br>

---

## Footnotes

[^fn1]: A footnote is something like that.
