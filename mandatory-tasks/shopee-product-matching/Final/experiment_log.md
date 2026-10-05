# Experiment Log — Finale: Multimodal Product Matching

## 1. Setup

For the Finale, I built a multimodal product matching system using the text and image approaches I developed in Parts B and C. I also investigated whether the `image_phash` field could provide an additional signal.

The dataset contains **34,250 listings** and **32,412 unique images**.

I kept the same group-based evaluation idea from Parts B and C so that listings from the same `label_group` were not split across train, validation, and test sets.

| Split | Listings |
|---|---:|
| Train | 23,629 |
| Validation | 5,189 |
| Test | 5,432 |

For evaluation, I created balanced positive and negative pairs:

- Positive pair: both listings belong to the same `label_group`
- Negative pair: listings belong to different `label_group`

| Split | Pairs | Positive | Negative |
|---|---:|---:|---:|
| Validation | 3,304 | 1,652 | 1,652 |
| Test | 3,306 | 1,653 | 1,653 |

For every pair, I calculated:

- Character TF-IDF cosine similarity
- ResNet50 image cosine similarity
- pHash agreement

I selected thresholds and fusion weights using the validation set and used the test set only for the final evaluation.

---

## 2. Implementation

### Text Representation

I reused the best text approach from Part B:

- Character-level TF-IDF
- Character n-grams: `(3, 5)`
- `min_df=2`
- Cosine similarity

I chose character TF-IDF because it performed better than both word-level TF-IDF and multilingual sentence embeddings in Part B.

### Image Representation

I reused the best image model from Part C:

- Pretrained ResNet50
- Final classification layer removed
- 2048-dimensional image embeddings
- L2 normalization
- Cosine similarity

I generated an embedding once for each unique image and reused it for all pair comparisons.

### pHash

I used `image_phash` as a binary additional signal:

```text
1 → both listings have the same pHash
0 → pHash values are different
