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

1 → both listings have the same pHash
0 → pHash values are different

# 11. Conclusion

In this Finale, I started with the text and image matching approaches from the previous parts and investigated whether they could work better together.

The experiments showed that the **text representation was the strongest individual signal**. Character-level TF-IDF achieved a test F1 of **0.9799**, while the ResNet50 image representation achieved **0.9279**. However, the image information was still useful when combined with text.

A simple 50/50 fusion gave a test F1 of **0.9802**, but the weight experiment showed that equal weighting was not optimal. Giving more importance to text improved the result, with the **75% text + 25% image** configuration achieving a validation F1 of **0.9909** and a test F1 of **0.9881**.

I then investigated whether the perceptual hash could provide additional information. Adding pHash produced the best validation result with the following configuration:

Character TF-IDF : 60%
ResNet50         : 20%
pHash            : 20%
Threshold        : 0.14
