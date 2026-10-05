# Experiment Log — Part C: Image-Based Product Matching

## 1. Setup

- Dataset: Shopee Product Matching — 34,250 listings and 32,412 unique images.
- Used positive pairs from the same `label_group` and negative pairs from different groups.
- Used pretrained CNNs to generate image embeddings.
- L2-normalized embeddings and used cosine similarity.
- Selected the matching threshold using validation F1 and evaluated it on the test set.
- Generated embeddings in batches on GPU.

---

## 2. Experiment 1 — ResNet18

### Implementation

- Used pretrained ResNet18 with the final classification layer removed.
- Extracted **512-D embeddings** for all 32,412 images.
- Tested thresholds from 0.10 to 0.95 on the validation set.

### Results

Best threshold: **0.70**

| Metric | Validation | Test |
|---|---:|---:|
| Precision | 0.9237 | 0.9274 |
| Recall | 0.8686 | 0.8808 |
| F1 | 0.9048 | **0.9036** |

### Observation

ResNet18 gave a strong image-only baseline. Lower thresholds increased recall but also produced more false positives, while higher thresholds increased precision at the cost of recall.

---

## 3. Experiment 2 — ResNet50

### Implementation

- Used pretrained ResNet50 with the final classification layer removed.
- Extracted **2048-D embeddings** for the same 32,412 images.
- Used the same pairs and evaluation procedure as Experiment 1.

### Results

Best threshold: **0.45**

| Metric | Validation | Test |
|---|---:|---:|
| Precision | 0.9457 | 0.9477 |
| Recall | 0.8953 | 0.8996 |
| F1 | 0.9198 | **0.9230** |

### Observation

ResNet50 improved test F1 from **0.9036 → 0.9230**, giving better visual representations than ResNet18. It also required more computation and had a different optimal threshold.

---

## 4. Experiment Comparison

| Model | Embedding | Threshold | Precision | Recall | F1 |
|---|---:|---:|---:|---:|---:|
| ResNet18 | 512-D | 0.70 | 0.9274 | 0.8808 | 0.9036 |
| **ResNet50** | **2048-D** | **0.45** | **0.9477** | **0.8996** | **0.9230** |

ResNet50 performed better and was selected as the stronger image-based model.

---

## 5. Error Analysis

For ResNet18, there were **114 false positives** and **197 false negatives** at the 0.70 threshold.

- **False positives:** Different products sometimes looked very similar. For example, a hand sanitizer and a skincare serum had a similarity of 0.83 because both were white pump bottles on a plain background.
- **False negatives:** The same product could be shown very differently. One pair of alphabet-letter products had a similarity of only 0.47, while another same-group pair had a normal product photo and a text/price image with a similarity of 0.48.

This showed that ResNet18 captures visual appearance well, but visual similarity does not always mean the products are actually the same.
