# Experiment Log — Part C: Image-Based Product Matching

## 1. Implementation

- Loaded the original **Shopee Product Matching** dataset.
- Verified that the training set contains 34,250 listings and 32,412 unique image files.
- Confirmed that image filenames in the `image` column already include the `.jpg` extension.
- Loaded a sample product image successfully from the `train_images` directory.
- The sample image was 1024 × 1024 pixels and RGB.
- Planned the image matching pipeline as:
  Image → pretrained vision model → image embedding → cosine similarity → matching decision.
- Selected pretrained **ResNet18** as the initial image representation model.
- The final ImageNet classification layer was removed so that the model produces a 512-dimensional image embedding instead of a class prediction.
- GPU availability was checked before large-scale embedding extraction because thousands of images need to be processed.

## 2. Results

- Image loading was successful.
- Sample image dimensions: 1024 × 1024.
- Image format: RGB JPG.
- Number of image files in `train_images`: 32,412.
- No matching performance was measured yet because embedding extraction and pairwise evaluation have not been completed.

## 3. Relevant experiments

- **Initial representation:** pretrained ResNet18.
- ResNet18 was chosen as a simple, established pretrained CNN baseline that provides a compact 512-dimensional representation.
- The final classification layer was removed because the objective is product similarity rather than ImageNet classification.
- The next experiment will evaluate image-pair similarity using cosine similarity between the extracted embeddings.

## 4. Observations and conclusions

- The dataset contains a large number of product images, so computational efficiency is important.
- Images can be loaded directly using the filenames stored in the `image` column.
- Image embeddings provide a way to represent visual information numerically so that two product images can be compared using a similarity metric.
- Large-scale embedding extraction should be performed in batches, preferably on GPU, rather than processing images individually.
- Actual matching performance and threshold selection will be documented after the ResNet18 experiment is completed.
