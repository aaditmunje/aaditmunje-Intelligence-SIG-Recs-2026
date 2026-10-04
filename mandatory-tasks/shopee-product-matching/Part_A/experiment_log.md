# DATA ANALYSIS

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

  # Some Questions we can answer alr :
  
Q1. How many product listings are there?
34,250.
     
Q2. How many different products/product groups are there?
11,014.

Q3. Does every product have multiple listings?
Yes. Every one of the 11,014 groups has at least 2 listings.

Q4. What is the typical number of listings for a product?
2 listings is the most common, and the median group size is 2. The mean is 3.11 because a few products have many listings.

Q5. What is the largest product group?
51 listings. There are 7 groups with 51 listings.

Q6. Are images always unique for every listing?
No. 34,250 listings but only 32,412 unique images.
So some listings reuse exactly the same image.

Q7. If two listings have exactly the same image, can we automatically say they're the same product?
No. We found 46 images that are used across multiple product groups.
So same image = strong signal, but not guaranteed same product.

Q8. Are titles always unique?
No. 34,250 listings but only 33,117 unique titles.
So some titles are reused.



