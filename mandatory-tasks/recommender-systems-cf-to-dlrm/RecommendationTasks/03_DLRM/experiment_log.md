## DLRM Paper Notes

Paper:
"Deep Learning Recommendation Model for Personalization and Recommendation Systems"
Naumov et al., 2019

### Main idea

DLRM is designed for recommendation systems where we have:
- Numerical (dense) features
- Categorical (sparse) features

The categorical features are represented using embedding tables.
The numerical features are passed through a bottom MLP.

DLRM explicitly models interactions between the feature representations
using pairwise dot products.

The resulting interaction features are combined with the dense
representation and passed through a top MLP to predict the probability
of a click.

### Core Architecture

We create embeddings for 26 categorical features. Then we add "Pairwise feature interactions",
and numerical features. Then we go from the Bottom MLP to the top MLP to get the CTR probability.

## Implementation Plan

- 13 numerical features

## 5. DLRM Baseline Results

The DLRM model was trained using 16-dimensional embeddings, a bottom
MLP of 13 → 64 → 16, pairwise feature interactions, and a top MLP
of 128 → 64 → 1.

Early stopping based on validation ROC-AUC stopped training after
3 epochs. Training took approximately 28.5 seconds.

### Validation Results

| Metric | DLRM |
|---|---:|
| ROC-AUC | 0.664 |
| PR-AUC | 0.073 |
| Log Loss | 0.145 |
| Accuracy | 0.968 |
| F1 @ 0.5 | 0.000 |

### Observation

DLRM performed substantially better than the original NN-1 baseline
from Task 02, improving ROC-AUC from 0.599 to 0.664 and reducing
log loss from 0.322 to 0.145.

However, NN-2 from Task 02 achieved higher validation ROC-AUC,
PR-AUC and lower log loss than the current DLRM model.

The F1 score at the default 0.5 threshold was 0 because the model
did not produce positive predictions at that threshold. Since the
dataset is highly imbalanced, accuracy alone is not sufficient to
judge performance.

I will try to reove the explicit pairwise interaction component 
while keeping the remaining architecture similar. 
This shoud help determine how much the interaction component contributes
to DLRM's performance.
- 26 categorical features
- Embedding dimension: 16
- Bottom MLP: 13 to 64 to 16
- 26 categorical embeddings + 1 dense representation = 27 feature vectors
- Pairwise dot-product interactions
- Number of pairwise interactions: 27 × 26 / 2 = 351
- Top MLP: 367 to 128 to 64 to 1
- Sigmoid output for click probability (as prob b/e 0 and 1).
