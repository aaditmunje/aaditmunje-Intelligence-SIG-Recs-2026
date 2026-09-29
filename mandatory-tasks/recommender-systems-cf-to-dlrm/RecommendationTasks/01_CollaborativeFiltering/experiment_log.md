# Collaborative Filtering — Experiment Log

## Task Objective

Compare:
1. Memory-based collaborative filtering
2. Model-based collaborative filtering using matrix factorization

Both approaches will use the same dataset, preprocessing decisions, and evaluation protocol.

---

## Dataset Selection
**Dataset:** MovieLens 1M

**Why this dataset was chosen:**
MovieLens 1M provides approximately one million explicit
1–5 star ratings from around 6,000 users across around
4,000 movies. 

Basically I looked into a variety of possible datasets, like 
initially a few different MovieLens versions such as 100K and 2M.
For the task given to us, I felt that some were too big/small, and the 1M
dataset would be a good enough size for the comparison between
Memory-based CF and Matrix Factorization.

I considered the HetRec dataset too, since it had additional
information like movie tags and was a smaller but richer dataset.
However, I still felt that MovieLens would give me a cleaner setup
to compare the two approaches.

**Users:** ~6,000
**Items:** ~4,000
**Interactions:** ~1,000,000
**Rating range:** 1–5
**Sparsity:** To be calculated during EDA

---

## Experiment 1 — Exploratory Data Analysis

**Question:**

**What I checked:**

**Observations:**

**Decision / implication:**

---

## Experiment 2 — Memory-Based Collaborative Filtering

**Approach:**

**Similarity measure:**

**Neighbourhood size:**

**Why these choices were made:**

**Result:**

**Observations:**

---

## Experiment 3 — Matrix Factorization

**Approach:**

**Latent dimensions:**

**Optimization method:**

**Learning rate:**

**Regularization:**

**Why these choices were made:**

**Result:**

**Observations:**

---

## Experiment 4 — Comparison / Ablation

**Question:**

**Change tested:**

**Result:**

**Observation:**

---

## Final Comparison

| Method | RMSE | MAE | Precision@K | Recall@K |
|---|---:|---:|---:|---:|
| Memory-based CF | | | | |
| Matrix Factorization | | | | |

### Conclusion

**When does memory-based CF work well?**

**When does matrix factorization work well?**

**Main lesson:**
