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

