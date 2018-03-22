# [Semi-Supervised Learning with Ladder Networks](https://arxiv.org/pdf/1507.02672.pdf)

by: **Antti Rasmus, Harri Valpola, Mikko Honkala, Mathias Berglund, Tapani Raiko (The CuriousAI Company)**

## tl;dr

Combination of supervised and unsupervised elements to allow for semi-supervised learning as well as supervised segmentation. No layer-wise pretraining needed. Gets 1.06% error rate on MNIST with only 100 labeled examples.

## Notes

Semi-supervised learning : setting in which a only a few number of examples have observed targets and the rest is used in an unsupervised way to increase performance.

Auxiliary tasks : intermediate tasks for layers.

#### Ladder Networks

![](../imgs/sslln.png)

One unsupervised component and one supervised component, whose encoders share the same weights.

Clean encoder : its hidden layers' activations serve as a comparison.

Noisy encoder/decoder : 

* noise is added to input and every hidden layer
* noisy output is used as classification during training
* a denoising function is applied to each hidden representation and compared to clean one
* a special Batch Normalization scheme is applied : normalization, adding noise and then reshifting + rescaling
* skip connections between encoder and decoder : makes problem easier by leaving some of the complex modelling to the lower layers (higher layers do not have to encapsulate all info)

Denoising function : g is a function that is applied after Batch Norm and merges normalized reconstruction with noisy encoding from forward.

Cost is sum of mean squared error for each hidden layer + classic supervision cost (when supervised).

They're like nested denoising autoencoders : the main task is splitted into intermediate denoising subtasks.

Encoder/decoder correspondence is incentivized via loss function (does not come naturally).

Perks of using Ladder Networks :

* can do supervised learning with this architecture
* suitable for deep architectures thanks to local losses
* multiplies computational cost by a fixed factor wrt to supervised (~3)

#### Experiments

Labels are balanced (via oversampling ?).

All samples in the dataset are used to train Ladder Networks.

Denoising cost multipliers are hyperparameters to tune (first ones are huge, last ones are microscopic).

#### Other

This [link](https://www.reddit.com/r/MachineLearning/comments/3w3egh/the_ladder_network_results_extremely_impressive/) could indicate that paper results are difficult to reproduce and not really generalizable to more complicated datasets.

[Pezeshki et al.](https://arxiv.org/pdf/1511.06430.pdf) (2015) have explored in detail the Ladder architecture. They conclude that the combinator function can be improved using an MLP-like scheme. They also show that both reconstruction cost and lateral connections are crucial ; and that adding noise in layers helps generalization.

