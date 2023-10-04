# Model to Predict the Winner or Horse Races

## Model Description
**Input:** Horse racing features such as course, race distance, going, horsename, horse age, weight, jockey, trainer, genealogy, starting price, industry ratings 

**Output:** Binary win/didn't win 

**Model Architecture:** Developer model used XGBoostClassifier with hyperparameter tuning using skopt gp_maximize. Also ensemble XGBoostClassifier with model selection and hyperparameter tuning using Microsoft Azure AutoML cloud service

## Performance
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

## Limitations
The model did not take into account the form of horses, jockeys or trainers. That is, results preceeding the race in question. 

## Trade-offs
None recognised
