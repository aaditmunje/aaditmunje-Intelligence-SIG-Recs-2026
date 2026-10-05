## Experiment — Text/Image Weight Fusion

### Hypothesis

So i looked at a lot of solutions using different boosting, bagging and various ensemble techniques. But, when i looked at the parts B,C we had already gotten
like 90+% F1 score for both. So i thought why not just play with both of their similarity scores and get a good prediction model.

Since text was stronger than image individually, I expected the final system to benefit from giving text a higher weight while still using visual information.

### Experiment

I tested different combinations of character TF-IDF similarity and ResNet50 image similarity. For each combination, the matching threshold was selected using validation F1.

### Results

| Text Weight | Image Weight | Validation F1 | Test F1 |
|---:|---:|---:|---:|
| 0.10 | 0.90 | 0.9442 | 0.9475 |
| 0.25 | 0.75 | 0.9648 | 0.9639 |
| 0.50 | 0.50 | 0.9842 | 0.9802 |
| **0.75** | **0.25** | **0.9909** | **0.9881** |
| 0.90 | 0.10 | 0.9903 | 0.9854 |

### Observation

Giving more weight to text consistently improved performance because character-level TF-IDF was the stronger individual modality. However, the best validation result was obtained with **75% text and 25% image**, showing that the image signal still provided useful additional information.

### Conclusion

The **75:25 text-to-image combination** was selected as the best multimodal configuration based on validation F1.

## Final Model

The final model combines character-level TF-IDF, ResNet50 image similarity, and image pHash agreement.

| Component | Weight |
|---|---:|
| Character TF-IDF | 0.60 |
| ResNet50 | 0.20 |
| pHash | 0.20 |

The final matching threshold was **0.14**, selected using validation F1.

### Final Results

| Metric | Validation | Test |
|---|---:|---:|
| F1 | 0.9915 | **0.9884** |
| Precision | — | **0.9957** |
| Recall | — | **0.9812** |

The final system achieved a test F1 of **0.9884**. Text was the strongest individual signal, while image similarity provided complementary visual information. Adding pHash produced a small additional improvement.
