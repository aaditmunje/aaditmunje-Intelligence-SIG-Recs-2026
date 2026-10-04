# PART A : DATASET EXPLORATION

- When I first looked at the dataset and obtained basic dataset information i found very interesting observations:
  1) 34,250 listings → only 32,412 unique images. So there are 1,838 extra images (more than 1 copy).
  2) Also, TITLES: 34,250 listings → 33,117 unique titles. That's 1,133 repeated title occurrences.
 
So there is a chance of duplicate images, which we will investigate after more analysis. 

- Group structure: All 11,014 product groups contain multiple listings. The median group size is 2 and the mean is 3.11,
- A highly right-skewed distribution. Most products therefore have only a few listings, while a small number have very large groups.
- The largest listing size is 51.

- INTERESTING FINDING: 46 / 1,246 (3.7%) repeated images occur across multiple product groups
96.3% of repeated images occur within a single product group, while 3.7% appear across multiple groups. Therefore, image identity can provide a strong matching signal, but is not always true either (46 exceptions).

- I wanted to deep down into finding out where exactly these exceptions happen :
  1) Same/ similar product ki category but diff identity.
     eg. There's this 1 image Oc1da..jpg where its corresponding to 2 listings (Pushop Waistbag & Waistbag Pushop).

  2) Same STOCK image used across product identities
     eg. There's this bubble wrap pic that's used in multiple groups.
     Even the titles are very noisy, like "bubble wrap", "BUBBLE WARP", "EXTRA BUBBLE WRAP UNTUK PACKING"

# FULL EDA AFTER RUNNING ALL CELLS: 

## 1. Dataset Overview

For this part, I explored the Shopee Product Matching training dataset.

The dataset contains 34,250 listings and 11,014 product groups.

The main columns are:

- `posting_id` — unique ID for each listing
- `image` — image file associated with the listing
- `image_phash` — perceptual hash of the image
- `title` — product title provided by the seller
- `label_group` — identifies listings belonging to the same product

There were no missing values in any of the five columns.

| Property | Value |
|---|---:|
| Number of listings | 34,250 |
| Number of product groups | 11,014 |
| Unique images | 32,412 |
| Unique image pHashes | 28,735 |
| Unique titles | 33,117 |
| Missing values | 0 |

A listing is one row in the dataset, while a product group contains multiple listings that correspond to the same product.

---

## 2. Product Group Analysis

Every product group in the training data contains at least two listings.

The group size statistics were:

- Mean group size: 3.11 listings
- Median group size: 2 listings
- Minimum group size: 2 listings
- Maximum group size: 51 listings

Most groups are small. 6,979 groups contain exactly 2 listings and 1,779 contain exactly 3 listings. There are also a small number of much larger groups, with 7 groups containing 51 listings each.

The distribution is therefore strongly right-skewed.

This is useful for the matching problem because most products only have a few examples, while some products have many different listings.

---

## 3. Image Analysis

There are 32,412 unique images for 34,250 listings, so some images are reused across multiple listings.

I found:

- 1,246 images appearing more than once
- 3,084 listings using these repeated images
- 1,200 repeated images were found within only one product group
- 46 repeated images appeared across multiple product groups

This means that exact image matching is a useful signal, but it is not always enough to identify the product.

For example, the same image was used for different bubble-wrap listings belonging to different product groups. Another example involved the same waistbag image being used for two different product groups.

This suggests that generic or reused seller images can cause false matches if image identity is used by itself.

### pHash analysis

I also checked repeated perceptual hashes.

There were:

- 3,229 repeated pHashes
- 8,744 listings associated with repeated pHashes
- 3,082 repeated pHashes occurred within one product group
- 147 repeated pHashes occurred across multiple product groups

So pHash gives more repeated cases than exact image matching, but it also produces some cross-product matches. This suggests that visually similar images can belong to different products.

---

## 4. Title Analysis

I looked at both the number of characters and the number of words in each title.

| Statistic | Character length | Word count |
|---|---:|---:|
| Mean | 56.16 | 9.41 |
| Median | 53 | 9 |
| 25th percentile | 36 | 6 |
| 75th percentile | 73 | 12 |
| Minimum | 5 | 1 |
| Maximum | 357 | 61 |

Most titles are relatively short, but there is a long tail of very long titles.

The titles also contain different amounts of information. Some are very short, while others include brands, sizes, colours, models, product specifications and extra seller keywords.

### Repeated titles

There were 962 titles appearing more than once, covering 2,095 listings.

Out of these:

- 889 repeated titles occurred within only one product group
- 73 repeated titles occurred across multiple product groups

Therefore, an identical title is a strong signal, but it does not guarantee that two listings belong to the same product. (Just like the images walla part)

The examples also showed that sellers can use the same generic title for different products.

---

## 5. Example Product Groups

### Example 1 — Same product, different listings

I inspected product group `48363793`, which contains three listings.

The three titles were different, including:

- `YAO YAO Kanaya Outer / Kardigan Moscrepe ...`
- `RUFFLE CARDIGAN / KARDIGAN WANITA ...`
- `1KG MUAT 7PCS || (FREE BELT) KANAYA OUTER ...`

Even though the titles are different, all three listings belong to the same `label_group`.

This shows that listings for the same product do not necessarily use exactly the same title.

### Example 2 — Same image, different products

I found cases where exactly the same image was used by listings from different product groups.

One example was a bubble-wrap image that appeared in two different product groups. The titles were variations such as:

- `BUBBLE WRAP`
- `BUBBLE WARP`
- `EXTRA BUBBLE WRAP ...`

This shows that the same image can be reused across different products/listings.

### Example 3 — Same title, different products

I also found identical titles appearing under different `label_group` values.

This means that exact title matching can also produce false matches, especially when sellers use generic product names.

---

## 6. Main Challenges Identified

From the exploration, the main challenges I found are:

1. **Multiple listings for the same product**
   - The same product can appear in several listings with different titles and images.

2. **Image reuse**
   - The same image can be used for different product groups.

3. **Visually similar images**
   - Repeated pHashes occur across different product groups, so visually similar images are not always the same product.

4. **Noisy product titles**
   - Titles vary considerably in length and wording.
   - Sellers add specifications, keywords, sizes and other information.

5. **Spelling and wording variations**
   - Small spelling differences and different ways of describing the same product are common.

6. **Generic titles/images**
   - Generic descriptions and stock-like images can be shared across different products.

7. **Product variations**
   - Different listings may refer to very similar products but still belong to different product groups.

---

## 7. What I Learned from the Exploration

### What makes two listings belong to the same product?

The `label_group` shows the underlying product identity. From the examples, the same product can have different titles and images, so matching cannot depend on exact equality of one field.

### Can titles alone reliably determine whether two listings match?

No.

Titles are useful because they contain product information, but identical titles can occur across different product groups and the same product can have different titles.

### Can images alone reliably determine whether two listings match?

No.

Exact image reuse is a strong signal, but I found 46 repeated images that occur across multiple product groups. Repeated pHashes also occur across different groups.

### What examples are likely to be difficult for a model?

I expect difficult cases to include:

- different listings of the same product with different titles/images
- generic or reused images
- visually similar but different products
- generic or repeated titles
- noisy titles with spelling variations
- products with many similar variants

### What information could be useful for solving the matching problem?

The dataset provides several useful signals:

- Product titles
- Images
- Image pHashes
- Text similarity between titles
- Visual similarity between images
- Exact image matches
- Exact title matches
- Product group information during training

The exploration suggests that combining **textual and visual information** should be more reliable than using only one of them.

---

## 8. Conclusion

The main takeaway from Part A is that Shopee product matching is not simply a duplicate detection problem.

Some duplicate images and titles are strong indicators of a match, but neither is completely reliable. At the same time, listings belonging to the same product can have different titles and images.

Because of this, a useful matching system will likely need to combine information from both the **product title and the product image**, while handling noisy text, reused images and visually similar products.

So basically, this is a very noisy dataset, and a lot of feature selection and feature engineering will need to be done to properly get good accuracy. 



