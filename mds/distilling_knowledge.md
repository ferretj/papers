# [Distilling the Knowledge in a Neural Network](https://arxiv.org/pdf/1503.02531.pdf) 

by: **Geoff Hinton, Oriol Vinyals, Jeff Dean (Google)**

##tl;dr

A technique to distill the knowledge of a high-performance ensemble of models into a smaller model that still performs well and can be used for inference and deployment. 

##Notes

Insects have a larval state optimized to get resources and an adult state that is optimized to interact with the world. 

By analogy, the requirements of a model aren't the same in training than in production !

**Distillation = training student on soft targets**

We can train a distilled network to reproduce the soft targets formed by the geometric or harmonic mean of the post-softmax outputs of many bigger networks.

Soft targets have a clear advantage on hard targets in the sense that they contain a lot more information. 

Additionally, it makes the task of the distilled network harder and prevents from overfitting.

**Temperature**

A temperature factor is added in the softmax expressions. 

Temperature controls the smoothness of the softmax distribution, and its optimal value depends on the size and capacity of the distilled network.

Typically, temperatures > to 1 are used, which means that probability distributions are softened before being fed to distilled nets.

**Results**

On a huge image classification proprietary dataset (100M, 15000 classes), the authors find that training specialist models initialized with the weights of a generalist model (trained for 6 months with asynchronous SGD !!) is fast and helps a lot to improve the global performance. 

Specialists are trained with a focus on classes the generalist does not handle very well, and deal with oversampled copies of the original train set. 

More precisely, each specialist focuses on a group of classes put together via clustering on the covariance matrix of the generalist predictions, and have a dustbin class that encompasses all the remaining classes together. 

The training of the specialist can be parallelized easily.