# An Investigation into the Use of Machine Learning to Predict the Winner or Horse Races 

## NON-TECHNICAL EXPLANATION OF THE PROJECT
This project seeks to develop a machine learning model which can predict the winner of a horse race using readily available data including race location, the going, starting odds, jockey, starting odds and industry ratings.
A secondary objective was to evaluate the relative performance of a developer derived optimised model compared to models calculated to be optimal by the Microsoft Azure cloud machine learning service, so called AutoML.

Both models produced a precision of around 63%. The Precision metric answers the question “Out of all the samples that the model classified as positive, how many were actually positive?” which is important in cases such as this where the cost of a false positive is high.

The developer model produced slightly more accurate results than the AutoML generated one.

## DATA
The dataset is available from Kaggle: https://www.kaggle.com/datasets/hwaitt/horse-racing/data and consists of two .csv files containing horse and race data for every year from 1990 to 2020. The two files are linked by the rid (raceid) field. 
Raw horse dataset consists of 4107315 records of 28 features.
Raw race dataset consists of 396572 records of 19 features.
After merging the two datafiles and preprocessing the final dataset to be applied to train and test models consisted of 2291748 records with 14
Features.
The target was identified as ‘res_win’ which is a binary variable returning True if a horse won the particular race and False if it didn’t.
## MODEL 
Sklearn XGBoostClassifier was selected as the base model for the developer part of the project for the following reasons: 
Performance: XGBoost often delivers superior results compared to many other algorithms when the hyperparameters are tuned correctly.
Handling Imbalanced Datasets:Scale_pos_weight: XGBoost has a built-in parameter, scale_pos_weight, that helps handle class imbalance.
Versatility: XGBoost can handle both binary and multiclass classification problems.
Robustness to Overfitting: Due to its boosting nature, when properly tuned, XGBoost can be more robust to overfitting, especially when working with small datasets.
Flexibility:
Tree Pruning: Unlike GBM (Gradient Boosting Machine), which stops splitting a node when it encounters a negative loss, XGBoost splits up to the max_depth specified and then prunes the tree backward and removes splits beyond which there is no positive gain.
Regularization: XGBoost has options to regularize the model, which can further prevent overfitting.
Parallel and Distributed Processing: XGBoost is can utilize both parallel and distributed computing, which makes it faster than many other algorithms.

By coincidence, Azure AutoML produced an optimised ensemble model containing 10 XGBoostClassifier models each with a set of tuned hyperparameters

## HYPERPARAMETER OPTIMSATION
For the developer model hyperparameters were tuned using skoptimize gp_maximize library on the following parameter grid:
param_grid = [
    Integer(450, 500, name='n_estimators'),
    Integer(3, 8, name='max_depth'),
    Real(0.01, 0.99, name='learning_rate'),
    Categorical(['gbtree', 'dart'], name='booster'),
    Real(0.01, 10, name='gamma'),
    Real(0.50, 0.90, name='subsample'),
    Real(0.8, 0.85, name='colsample_bytree'),
    Integer(1, 50, name='reg_lambda'),
]
The Azure AutoML models were optimised automatically.
 
## RESULTS
Preliminary data investigation showed that, as expected, the dataset is highly unbalanced.
 
A correlation matrix shows the greatest negative correlation between res_win and the feature RPR which is the horse rating produced by The Racing Post, a horse racing newspaper.

 

The performance metrics for the developer model were:
Accuracy: 88.40%
F1 score: 31.50%
ROC AUC: 86.33%
Confusion matrix:  [[392933   7218]
 [ 45969  12230]]
Precision: 62.89%
Recall: 21.01%

The performance metrics for the AutoML model were:
Accuracy: 88.50%
F1 score: 33.90%
ROC AUC: 87.06%
Confusion matrix:  [[392149   8002]
 [ 44689  13510]]
Precision: 62.80%
Recall: 23.21%

Both models produced similar results with a, at first sight, impressive looking accuracy of 88%. However, because of the unbalanced nature of the data and the fact that anyone using the model would want to pick the winner of a race and if they did so not want to lose, the most important metric of model performance was deemed to be precision (the ratio of true positives to true positives plus false positives). The developer model scored a slightly higher 62.89%, the AutoML model 62.80%. 
The profitability of the model depends on the odds received on a bet when the bet is placed as well as the outcome. It was not investigated if the model’s precision was sufficient to produce a trading profit.

For secondary objective of the project, evaluating a developer produced model comopared to an AutoML model, it appears the developer produced model has marginally outperformed!

