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
User-user collaborative filtering using cosine similarity.
For each user, the 20 most similar users were used to predict
ratings for unseen movies using a similarity-weighted average.

**Similarity measure:**
Cosine similarity.

**Neighbourhood size:**
20 neighbours.

**Why these choices were made:**
Cosine similarity provides a simple way to compare users based
on their rating patterns. A neighbourhood of 20 was chosen as
an initial baseline without making the similarity search
unnecessarily expensive.

**Mistakes i made:**
1) I was already familiar with the Pearson correlation (r factor) because
 I had used it in a ML hackathon hosted by IET 2 days ago, so i tried using that
but it took way too much time to run as the data was like 6040 users * 3683 movies.

2) Then i thought about KNNs to find similarity (nearest neighbours). I didnt think
of this before as i thought KNNs are unsupervised but thats KNN Clustering. 
Nearnest neightbours was perfect for finding similarities.


**Result:**

| Method | RMSE | MAE |
|---|---:|---:|
| Global Mean Baseline | 1.120 | 0.936 |
| Memory-Based CF | 1.043 | 0.819 |

**Observations:**
The memory-based approach performed better than the global
mean baseline on both RMSE and MAE, showing that information
from similar users helps improve rating predictions.

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
