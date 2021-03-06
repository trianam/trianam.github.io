---
layout: post
title:  Test
date:   2018-03-22 17:30:00 +0100
author: Stefano Martina
mathjax: true
---
{% assign figcat = 1 %}
{% assign figcircle1 = 2 %}
{% assign figcircle2 = 3 %}

{% assign bibzongker = "Zongker2006" %}

It is possible to write equations like

$$
\begin{equation}
	\vec{y} = \frac{1}{e^x} \tag{1}
	\label{eq:test}
\end{equation}
$$

In equation \eqref{eq:test} is visible a test, it is also possible to
write inline formulas like $y=x^2$. In some cases is necessary to use the doble dollar also inline, for instance in $$x_i$$. Note that is also possible to use
footnotes[^fn1] in the text. To quote some pubblication write
something like {% include cite.html label=bibzongker %}.

I add some other stuff just to see if the reference link is working.
I need some space to fill. In {% include ref.html label=figcat %} is visible a nice cat figure.
{% include image.html url="/assets/test/test-cat.png" label=figcat description="A testing image." width="20%" %}

It is possible also to include vectorial images
{% include image.html url="/assets/test/test-circle.svg" label=figcircle1 description="A testing svg image." width="20%" %}

If we omit the label what happens?
{% include image.html url="/assets/test/test-circle.svg" description="A testing svg image." width="20%" %}

If we omit the description what happens?
{% include image.html url="/assets/test/test-circle.svg" label=figcircle2 width="20%" %}

And if we omit both label and description what happens?
{% include image.html url="/assets/test/test-circle.svg" width="20%" %}

# Section
Lorem **ipsum** dolor sit *amet*, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Subsection
I put here another reference to {% include ref.html label=figcat %} just to se if the link is working.

### Subsubsection
This is how to make a bullet list:
* first item;
* second item.

This is how to make a numbered list:
1. first item;
2. second item.

<br>

---

# References

{% include bib.html label=bibzongker authors="Zongker, D" year=2006 title="Chicken chicken chicken: Chicken chicken" venue="Annals of Improbable Research" pages="12, 16--21" link="http://www.google.com" doi="https://doi.org/10.1007/11893257_124" %}

<br>

---

# Footnotes

[^fn1]: A footnote is something like that.

