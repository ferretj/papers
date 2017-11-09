# [Understanding Deep Learning requires rethinking generalization](https://arxiv.org/pdf/1611.03530.pdf) 

by: **Chiyuan Zhang, Samy Bengio, Moritz Hardt, Benjamin Recht, Oriol Vinyals (MIT, Google Brain, Berkeley, Google DeepMind)**

## tl;dr

Deep Learning systems exhibit small difference between train and test performance. Regularization and generalization capability are misleadingly invoked to explain this phenomenon. The experiments of the paper show that ConvNets easily fit a random labeling of the data, with no impact of explicit regularization, even when replacing images from dataset with random noise. Shows theoretically that 2-deep NNs have perfect expressivity if number of parameters exceeds number of data points (which is the case in practice).

## Notes

VC dimension and Rademacher complexity -> statistical learning theory objects to measure generalization capability
(Rademacher complexity measures the ability of a model class to fit random binary labeling)

In practice, fail to distinguish NNs with antipodal generalization performance..

*Randomization tests*

Deep NNs are able to achieve 0 train error on randomly labeled data (and are unable to generalize, obviously, on test) -> without changing the hyperparams of a model we can influence the generalization error by randomizing labels

Capacity of NNs allow memorization of whole dataset
optimization is still easy on random labels, just takes a bit more time to train (small constant factor increase) 
            ≠
            intuition : training should be slowed down a lot or should not converge ! 

This is still valid with random permutations on the pixels!

*Explicit regularization*

(aparté : model architecture is a form of regularization itself !)

Explicit regularization is neither necessary nor sufficient to control generalization error

* models without regularization still perform well
* very different from classical convex optimization where necessary to rule out trivial solutions
* nonetheless, properly tuned regularization helps generalization increase

*Finite sample expressivity*

2-deep NN with p = 2n + d parameters can express any labeling of n-sized d-dimensional sample

* different from universal approximation theorem which defines if mathematical functions can be expressed over whole domain, here only working with a finite n-sized sample
* implies Rademacher complexity of ~1 for NNs

*Implicit regularization*

analysis of how SGD is an implicit regularizer
(f.i. for linear models SGD always converges to a model with a small norm)

there exists an upper bound on generalization error of a SGD-trained model in terms of number of steps (?)

Optimization remains easy for NN models even when not generalizing, which shows that the reason why optimization is easy and the true cause for generalization must be different.
