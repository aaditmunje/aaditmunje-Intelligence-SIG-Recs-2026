# Neural CTR Prediction — Experiment Log

## Task Objective

We basically have to build a simple vanilla neural network for ctr (binary class). Then using different architectures ill try to get best possible architecture.
The final selected model will be evaluated once on the untouched test set.

## Dataset Inspection

The training dataset contains 40,000 rows and 39 input features:
13 numerical features and 26 categorical features.

The target is a binary click label.

The positive class represents 1,280 of the 40,000 training examples, giving a click rate of 3.2%. Therefore, the dataset is highly imbalanced and accuracy alone may not be sufficient to evaluate the models. So we'll probably have to use other threshold metrics like FF1, ROC-AUC etc.

The dataset also contains a lot of missing values in both numerical and categorical features. So we will have to remove them (feature selection).

The categorical features have very different cardinalities, ranging from only a few categories to more than 13,000 categories. This makes embeddings more suitable than directly one-hot encoding all categorical features. OHE will make very sparse representations (lots of 0s) and thus inaccurate predictions.

## Initial Preprocessing Plan

Numerical features will be median-imputed and standardized. My thought process was for std. median might be better as its less affected by extreme values.

Categorical missing values will be treated as a separate category. Categorical values will be converted to integer IDs for embedding layers.

## Experiment 1 — NN-1 Baseline

### Architecture

- 26 categorical features with 16-dimensional embeddings
- 13 numerical features
- Embeddings and numerical features concatenated
- Dense layers: 128 → 64
- ReLU activation
- Sigmoid output
- Adam optimizer
- Batch size: 256
- Epochs: 8

### Validation Results

| Metric | Score |
|---|---:|
| ROC-AUC | 0.599 |
| PR-AUC | 0.052 |
| Log Loss | 0.322 |
| Accuracy | 0.938 |
| F1 | 0.064 |

### Observation

My first model clearly showed overfitting. From epoch 1 to epoch 8 i could clearly tell the difference in the train AUC increasing and val AUC decreasing.

Training AUC increased from 0.565 in the first epoch to 0.995 by the eighth epoch, while validation AUC decreased from 0.717 to 0.567.

Training loss continued to decrease while validation loss increased.

The accuracy is around 94% but that is obviously the case here cause the CTR is only about 3% so its showing almost all the people didnt click throguh which
makes sense. Its not a good metric.

### Decision

The baseline was overfitting, so the next experiment I thought should be firstly keep the same architecture but try reduce overfitting.

From what i know overfitting cal be reduced by 2 things- 
1) Increasing data - Like data augmentation in CNNs / getting more rows in ANNs - (NOT EASIBBLE HERE)
2) Reducing complexity : Early Stopping, Add dropout.

Trying with simple neural networks then try making more complex structutres to learn the intricacies.




