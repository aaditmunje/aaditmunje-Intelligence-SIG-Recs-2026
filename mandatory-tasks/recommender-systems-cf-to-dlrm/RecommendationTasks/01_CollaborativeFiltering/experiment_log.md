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
What does the interaction data look like and what characteristics
could affect collaborative filtering?

**What I checked:**
- Number of users, movies and ratings
- Missing values
- Rating range
- Sparsity
- Ratings per user
- Ratings per movie
- Rating distribution

**Observations:**
The dataset contains 6,040 users, 3,706 movies and 1,000,209
ratings. There are no missing values and ratings range from 1 to 5.

The user-movie matrix has approximately 95.53% sparsity.
The number of ratings per user and per movie is highly
right-skewed, with most users and movies having relatively few
interactions and a smaller number having many interactions.

The rating distribution is also uneven, with ratings of 3, 4 and 5
being much more common than ratings of 1 and 2.

**Mistakes i made:**
Firstly i did ratings.head() to get only 5 rows so i thought that the user_id 
is same like serial number so i can delete it.. later when i printed the entire set
i found out it gives how many ratings each user gives.

My initial plot didnt represent data cleanly so i used Xlim to limit the range of values


**Decision / implication:**
The high sparsity and uneven interaction distribution should be
considered when implementing both collaborative filtering
approaches. The same data split and evaluation protocol will be
used for both methods.

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
