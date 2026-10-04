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



