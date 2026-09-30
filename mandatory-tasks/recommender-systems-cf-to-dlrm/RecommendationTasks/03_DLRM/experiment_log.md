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

There are 26 categorical features for which we create embeddings. Then we add "Pairwise feature interactions",
and numerical features. Then we go from the Bottom MLP to the top MLP to finally get the probability of CTR.

## Implementation Plan

- 13 numerical features
- 26 categorical features
- Embedding dimension: 16
- Bottom MLP: 13 → 64 → 16
- 26 categorical embeddings + 1 dense representation = 27 feature vectors
- Pairwise dot-product interactions
- Number of pairwise interactions: 27 × 26 / 2 = 351
- Top MLP: 367 → 128 → 64 → 1
- Sigmoid output for click probability
