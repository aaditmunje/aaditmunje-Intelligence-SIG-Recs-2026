# Experiment Log — Part C: Image-Based Product Matching

## 1. Implementation

- Loaded the Shopee Product Matching dataset: 34,250 listings and 32,412 unique images.
- Verified that the images are RGB JPGs (sample size: 1024 × 1024).
- Used pretrained ResNet18 and removed its final classification layer to get 512-D image embeddings.
- Generated embeddings for all 32,412 images in batches on GPU.
- L2-normalized the embeddings and used cosine similarity to compare image pairs.
- Created positive pairs from the same `label_group` and negative pairs from different groups.
- Selected the matching threshold using validation F1 and evaluated it on the test set.

## 2. Results

| Metric | Validation | Test |
|---|---:|---:|
| Threshold | 0.70 | 0.70 |
| Precision | 0.9237 | 0.9274 |
| Recall | 0.8686 | 0.8808 |
| F1 | 0.9048 | **0.9036** |

## 3. Relevant experiments

- Tested thresholds from 0.10 to 0.95 on the validation set.
- Lower thresholds gave higher recall but many false positives.
- Higher thresholds improved precision but missed more matching products.
- Threshold **0.70** gave the best validation F1.

## 4. Observations and conclusions

- ResNet18 gave a strong image-only baseline with a test F1 of **0.9036**.
- There were **114 false positives** and **197 false negatives**.
- False positives often came from different products with similar packaging or product photography.
- False negatives often happened when the same product was shown very differently, such as a normal product photo vs. a promotional/text image.
- Overall, ResNet18 captures visual similarity well, but visual similarity does not always mean the products are the same.

### Error Analysis

ResNet18 gave 114 false positives and 197 false negatives at the 0.70 threshold.

### ResNet50 Experiment

- Used pretrained ResNet50 with the final classification layer removed.
- Extracted 2048-D embeddings for the same 32,412 images.
- Used the same image pairs and evaluation procedure as ResNet18.
- Best validation threshold was 0.45.

| Metric | Validation | Test |
|---|---:|---:|
| Threshold | 0.45 | 0.45 |
| Precision | 0.9477 | 0.9477 |
| Recall | 0.9198 | 0.8996 |
| F1 | 0.9198 | **0.9230** |

ResNet50 improved test F1 from 0.9036 to 0.9230 compared to ResNet18. The larger model produced more useful visual representations for this matching task, although it also required more computation and a different similarity threshold.

For false positives, I noticed that different products can look very similar. For example, a hand sanitizer and a skincare serum had a similarity of 0.83 because both were white pump bottles on a plain background. A hand sanitizer and Vitamin D3 bottle also got 0.82.

For false negatives, the main issue was that the same product could be shown very differently. One pair of alphabet-letter products had a similarity of only 0.47 even though they belonged to the same group. Another pair had one normal product photo and one text/price image, giving a similarity of 0.48.

This showed me that ResNet18 captures visual appearance well, but visual similarity does not always mean the products are actually the same. 
