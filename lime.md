# [Explaining the Predictions of Any Classifier (2016)](https://arxiv.org/pdf/1602.04938.pdf) 

##tl;dr

Machine learning models lack interpretability, mostly locally (around one example). Interpretability is needed to build trust in cases where model predictions are used to take actions. 
LIME is an attempt at providing local explainability, by fitting an interpretable model around a prediction. 

##Notes
 
*intro*

real world data can sometimes have an evaluation metric that is not representative of the product goal
local interpretability is a good way to assess viability of the modelization

2 things needed to bring trust :

* local interpretability to “trust a prediction” (in particular) == LIME
* guidance towards which examples to inspect to be able to “trust a model” (as a whole) == SPLIME

*general setting*

explanation, for a given example, gives an amplitude and a color to some variables
amplitude == relative importance
color == class prediction it contributes to

criteria for interpretations :

* interpretable (reasoning can be followed by a user)
* local fidelity : an interpretation of the model behaviour cannot be as rich as the model itself, but at least can have local faithfulness

**LIME**

***1- interpretability***

idea : turn original example in binary vector

examples :

* use presence / absence of a word for text classif.
* superpixels for image classification

***2 - fidelity / interpretability trade-off***

(potentially) interpretable models :

* linear models
* decision trees
* falling rule lists (lists of rules where the first rule applying defines the probability of the positive class. rules are ordered based on the highest probability induced first)

an explanation is an interpretable model, which should be simple enough for the user

complexity is measured differently for each model (e.g. depth for RFs, non-zero components for a linear model)

optim problem is : 
minimize approximation of f (model) by g (interpretability model) in a proximity zone of x (weighted by actual proximity), while keeping a low complexity for g

**focus of paper is on sparse linear models, with search performed using perturbations**

***3 - sampling for local exploration***

examples used to fit interpretable model are defined this way :

* original example is turned into binary vector (see 1-)
* random perturbations are applied to binary vector
* take closer example from binary vector obtained
* model prediction on that example is used as label

***4 - sparse linear explanations***

Proximity measure is an exponential kernel defined with a suitable distance function (e.g. cosine sim for text, L2 norm for images)
To select relevant interpretable features, use *regularization path* of LASSO (K-LASSO here because we only select K features), it means we use several lambdas and find one that gives K nonzero (or close) params
Then we fit the linear model on the remaining vars using least squares

**SPLIME**

SP stands for submodular pick, aim is to select relevant instances to assess model quality using LIME interpretations.
core idea is that, under a budget of n instances to explore, we want to maximize the cumulative importance of features with globally-defined importance (for the instances chosen)

* done greedily
* (non-greedy is NP-hard)

 
 
