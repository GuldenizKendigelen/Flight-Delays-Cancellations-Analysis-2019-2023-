# Model Results

## Flight Delay Prediction

The objective was to predict whether a flight would arrive more than 15 minutes late.

### Random Forest

- Recall: 64.9%
- Precision: 30.3%
- F1 Score: 0.413
- ROC-AUC: 0.646
- Average Precision: 0.349

Random Forest was selected based on validation performance, particularly Average Precision.

## Flight Cancellation Prediction

### Random Forest

- Accuracy: 82.23%
- Precision: 4.35%
- Recall: 27.34%
- F1 Score: 0.075
- ROC-AUC: 0.614

The relatively low precision reflects the strong class imbalance in flight cancellations.
