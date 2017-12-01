# **[A Tough to Beat Baseline for Sentence Embeddings](https://openreview.net/pdf?id=SyK00v5xx)**

by: **Sanjeev Arora, Yingyu Liang, Tengyu Ma (Princeton University)**

## tl;dr
Rend basic sentence embeddings (weighted average of word embeddings) smarter using PCA/SVD

## Notes

Word weights = a / (a + p(w)) where a is a fixed param and p(w) the estimated word frequency
called Smooth Inverse Frequency (SIF).

The a param is to be learned but a wide range of values outperform the weighted average baseline.

Common component removal : remove the projections of the average vectors on their 1st principal component

![algo](imgs/attbbfse.png)

Related work :

1. unweighted averaging works well for small sentences
2. coordinate-wise multiplication of word vectors works well !