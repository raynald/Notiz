
Generalized Linear Model

ROC (Receiver operating characteristic) Curve

Independent Component Analysis

Principal Component Analysis

 - first principle component
 - second principle component
 
Kernel Based Principal Component Analysis

kNN
 - Why not Manhattan distance? dimension restriction

Online learning algorithm

Decision tree 
Gini index, node entropy

Bagging algorithm (random forest)

Gradient boosting algorithm

	- improves accuracy by reducing both bias and variance in a model

Ensemble learner

k-means clustering

Type I error: false positive
Type II error: false negative


L1: Laplacean prior
L2: Gaussian prior

Ridge vs Lasso vs LARS

 - Lasso: variable selection, parameter shrinkage
 - Ridge: parameter shrinkage; works best when LS estimates have higher variance
 
Forward Selection, Backward Selection, Stepwise Selection

Correlation, covariance

k-fold, LOOCV

Bias variance relation
 - [!https://www.analyticsvidhya.com/wp-content/uploads/2015/07/error-of-a-model.png]
 - Bias is to quantify how much on an average are the predicted values different from the actual value.
 - Variance is to quantify how are the prediction made on same observation different from each other.
 
Ordinary least square (OLS)

Recall (true positive rate), precision


Say you had a 60% chance of actually having the flu after a flu test, but out of people who had the flu, the test will be false 50% of the time, and the overall population only has a 5% chance of having the flu. Would you actually have a 60% chance of having the flu after having a positive test?

Bayes’ Theorem says no. It says that you have a (.6 * 0.05) (True Positive Rate of a Condition Sample) / (.6*0.05)(True Positive Rate of a Condition Sample) + (.5*0.95) (False Positive Rate of a Population)  = 0.0594 or 5.94% chance of getting a flu.



Probability and likelihood: https://stats.stackexchange.com/questions/2641/what-is-the-difference-between-likelihood-and-probability#2647


Fourier transform

Deep learning: an unsupervised learning algorithm that learns representations of data through the use of neural nets

Difference between a generative and discriminative algorithm: https://stackoverflow.com/questions/879432/what-is-the-difference-between-a-generative-and-discriminative-algorithm


time-series model selection: https://stats.stackexchange.com/questions/14099/using-k-fold-cross-validation-for-time-series-model-selection


F1 score: https://en.wikipedia.org/wiki/F1_score

Python:
	- pandas
	- scikit-learn
