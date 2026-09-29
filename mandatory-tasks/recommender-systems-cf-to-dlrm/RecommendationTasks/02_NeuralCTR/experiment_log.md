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

Try with simple neural networks then try making more complex structutres to learn the intricacies.

## Initial Preprocessing Plan

Numerical features will be median-imputed and standardized. My thought process was for std. median might be better as its less affected by extreme values.

Categorical missing values will be treated as a separate category. Categorical values will be converted to integer IDs for embedding layers.
