# [HOGWILD! : a Lock-Free Approach to Parallelizing SGD](https://papers.nips.cc/paper/4390-hogwild-a-lock-free-approach-to-parallelizing-stochastic-gradient-descent.pdf)

by: **Feng Niu, Benjamin Recht, Christopher Ré, Stephen J. Wright (University of Wisconsin)**

## tl;dr

Schemes to parallelize SGD require memory locking and synchronization. HOGWILD! is asynchronous and lock-free, meaning that processors access a shared memory and are able to overwrite each other’s work. This method achieves nearly optimal convergence rate if gradient updates modify small parts of the decision variable.

## Notes 

#### Parallelizing SGD ?

SGD is not embarrassingly parallel by construction, which is too bad because multicore setups are inexpensive.

**1. Multicore computing**

* reading and writing in shared memory = ***~12Gb/s***
* data from RAID disk to main memory = ***~1Gb/s***

Bottleneck = synchronization of processes

**2. Cluster computing**

* reading incoming data = ***~10Mo/s*** (due to checkpointing so that if a node fails, the node’s job can be rescheduled to another node without information loss)

HOGWILD! was designed with the goal in mind to bring the best out of a multicore setup for a SGD task. 

It is doomed to fail in general as processes overwrite each other’s progress, but not in a sparse context, where it is rare that processes write over the same memory portion.

Almost linear speedups in sparse learning tasks !

*In practice, it is used for general ML tasks that are often not sparse at all, but does not provide the same speedups neither.*

**Sparse separable cost functions**

These are functions (entry = vector in Rn) to minimize that can be written as the sum of subfunctions taking as entry a subvector : each subfunction only deals with a small amount of components among 1, 2 ... n.

For sparse SVM (in which setup examples are known to be sparse), for each example, the subset of components considered is its set of non-zero components !

For a given example, we introduce the notion of hypergraph. A hypergraph is made of nodes (components / weights) and hyperedges (collections of components). In the sparse SVM setup, hyperedges are a combination of components for which several examples have nonzero values.

A task in considered sparse if three quantities are small :

* the max size of the hyperedges
* the max fraction of hyperedges that intersect a node
* the max fraction of hyperedges that intersect a hyperedge

#### HOGWILD!

**Hogwild! algorithm**

For each thread, asynchronously :

* sample an input uniformly
* evaluate function G on that input (according to corresponding hyperedge)
* update nodes (weights) belonging to hyperedge, as in standard SGD
software side

Parameters are stored in shared memory.
The update operation is a compare-and-exchange operation on multicore processors.

Stepsize (aka learning rate) is supposedly constant.

xj is defined as the state of x after j updates.

It is calculated using a stale gradient, that is a gradient based on a value quite anterior to xj-1 in practice.

**Fast rates for lock-free parallelism**

Hypotheses : 

* Lipschitz continuous differentiability
* strong convexity
* learning rate is chosen small enough (lr * modulus of strong convexity < 1, otherwise diverges)
* norm of gradients inferior to constant (almost surely)

implies :

* converges with a rate of 1/k
(which means that expected value of error is inferior to const/k after k updates)

It also works with decreasing learning rate, the protocol is :

* choose discounting param B in ]0, 1[
* run K updates
* multiply learning rate by B
* run K/B updates
* ...

In practice, they run the same amount of updates after each discounting !
