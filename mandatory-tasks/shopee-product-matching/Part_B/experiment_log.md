## Part B - Text-Based Product Matching

# MY THINKING

- So basically I've now understood the dataset. It's pretty noisy and has a lot of disturbances. Im planning to use 3 different approaches.

  A) Baseline: Word TF-IDF: I started with word-level TF-IDF because it is simple and provides a measurable baseline based on lexical overlap.
    Itll prolly give a poor score but its a good baseline to start off with. 
  
  B) Experiment 1: Character TF-IDF: I tested character n-grams because the Part A exploration showed spelling variations, abbreviations and noisy titles.
   Character features can capture similarity even when complete words differ. This should be helpful for our dataset.
  
  C) Experiment 2: Multilingual embeddings: I will then test pretrained multilingual sentence embeddings to see whether semantic representations could improve
    matching when titles use different wording. Embeddings should ideally give a more useful result.

  - I'm planning to use cosine similarity that i used previously in another task for all 3 so that i can get comparable results and then use Precision/ Recall/
  F1 score to get accuracy and then finally generate a similarity score.

# Error Analysis

- Once i actually did this, I found out a lot of good insights. I checked both the false positives and false negatives in this experiment.
- A false positive is a wrong result that claims a condition is present when it is absent, while a false negative is a wrong result that claims a condition is absent   when it is actually present

- The False negatives (61) showed that the same product can have completely different/noisy seller titles.

# Experiment Log — Part B

## 1. Implementation

Created a binary matching dataset from the Shopee training data.

- `1` = two listings belong to the same `label_group`
- `0` = two listings belong to different `label_group`s
- Product groups were split into train, validation and test before creating pairs to avoid product-level leakage.
- The resulting pairs were balanced between matches and non-matches.

Three approaches were tested:

1. Word-level TF-IDF baseline
2. Character-level TF-IDF
3. Multilingual sentence embeddings

Cosine similarity were used for all three approaches. Thresholds were selected using the validation set and final performance was measured on the held-out test set.

## 2. Results

| Experiment | F1 |
|---|---:|
| Word TF-IDF | 0.9690 |
| Character TF-IDF | **0.9791** |
| Multilingual embeddings | 0.9098 |

Character TF-IDF gave the best result.

## 3. Relevant experiments

### Baseline — Word TF-IDF

Used word and two-word features with cosine similarity.

Result:
- Precision: 0.9943
- Recall: 0.9449
- F1: 0.9690
- Threshold: 0.10

### Experiment 1 — Character TF-IDF

Used character n-grams from 3 to 5 characters.

Result:
- Precision: 0.9956
- Recall: 0.9631
- F1: 0.9791
- Threshold: 0.10

### Experiment 2 — Multilingual Sentence Embeddings

Used the pretrained `paraphrase-multilingual-MiniLM-L12-v2` model.

Result:
- Precision: 0.9228
- Recall: 0.8972
- F1: 0.9098
- Threshold: 0.40

## 4. Observations and conclusions

Character-level TF-IDF performed better than word-level TF-IDF. This suggests that character-level information is useful for handling spelling variations, abbreviations and product-specific strings.

The multilingual embedding model performed worse than both TF-IDF approaches. This suggests that general semantic similarity was less useful than fine-grained lexical information for these product titles.

Error analysis showed that false positives were usually caused by unrelated products sharing common keywords. False negatives occurred when listings of the same product had very different or incomplete titles.

The main limitation of the text-only approach is therefore that some products cannot be matched reliably from their titles alone. Image information could help with these cases.

- ## Results

| Experiment | Representation | Similarity | Threshold | Precision | Recall | F1 |
|---|---|---|---:|---:|---:|---:|
| Baseline | Word TF-IDF (1–2 grams) | Cosine | 0.10 | 0.9943 | 0.9449 | 0.9690 |
| Experiment 1 | Character TF-IDF (3–5 grams) | Cosine | 0.10 | 0.9956 | 0.9631 | **0.9791** |
| Experiment 2 | Multilingual Sentence Embeddings | Cosine | 0.40 | 0.9228 | 0.8972 | 0.9098 |

Character-level TF-IDF performed the best on the held-out test pairs.

The character-level model improved F1 from 0.9690 for the word-level baseline to 0.9791. The multilingual sentence embedding model performed worse, with an F1 of 0.9098.

This suggests that fine-grained lexical information was more useful for these product titles than general semantic similarity. Product titles contain many product-specific details such as model names, sizes, abbreviations, spelling variations and keywords, which character-level features can capture well.

## Conclusion

I started with word-level TF-IDF as the baseline and then tested character-level TF-IDF and pretrained multilingual sentence embeddings.

The character-level TF-IDF model performed best with an F1 score of 0.9791.

The improvement over word-level TF-IDF suggests that character-level features are useful for this dataset because product titles contain spelling variations, abbreviations, product codes and other noisy text.

The multilingual embedding model performed worse than both TF-IDF approaches. This was useful because it showed that a more sophisticated semantic representation is not automatically better for this problem. In these titles, exact product-specific lexical information appears to be more important than general semantic similarity.

However, the false-negative examples also showed that text alone cannot solve every case. Some listings of the same product have very different or incomplete titles. This suggests that a stronger product matching system should eventually combine textual and visual information.



