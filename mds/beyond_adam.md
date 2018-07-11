# [On the Convergence of Adam and Beyond](https://openreview.net/pdf?id=ryQu7f-RZ) 

by: **Sashank J. Reddi, Satyen Kale, Sanjiv Kumar (Google)**

## tl;dr

Proves mathematically that Adam and other adaptive learning methods suffer from a flaw resulting from their short-size memory of past gradients that lead to suboptimal solutions on several optimization problems.
They propose a correction for Adam that solves this issue and a new optimization scheme (AMSGrad) that has better empirical performance.

## Notes

A very simple 1-dimensional online (1-sized minibatch) optimization problem where Adam finds a suboptimal solution, given particular values for Beta 1 and 2 :

* l(f(x)) = C.x if step % 3 == 1 else -x     (C > 2)
* optimal : x = -1

Adam estimates x_hat = +1 because the influence of the informative gradient (grad(C.x)) is divided by almost C by its second-order adaptive scheme.

**Correction of Adam (AdamNc)**

Use increasing sequences for Beta 1 and 2 to extend the size of the memory of past gradients.

**AMSGrad**

Guarantees a non-increasing step size (alpha / sqrt(g ^ 2)) by normalizing with the maximum running average encountered instead of the current running average.