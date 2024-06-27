# Jaccard-Based-Recommender-System

## Musical instruments recommenders system

This project implements a recommendation system using the Polars DataFrame library. 
The system recommends products to reviewers based on the Jaccard similarity of their review histories.

## Installation

To run this project, you'll need to install the following libraries:

[Polars](https://github.com/pola-rs/polars) is a blazingly fast DataFrame library implemented in Rust and runs on Python.
```bash
!pip install polars
```

[Polars-Distance](https://github.com/ion-elgreco/polars-distance) is a separate package/polars-plug-in that provides 
additional distance functions for Polars. In our case, we will use it to calculate the Jaccard similarity between two lists.

```bash
!pip install polars-distance
```

**Note:** After installing Polars and Polars-Distance, you may need to restart your Jupyter notebook or Python 
environment for the changes to take effect.

## Dataset

The dataset used in this project is from the Amazon product data, specifically the Musical Instruments review dataset 
and its metadata. The dataset can be found [here](https://nijianmo.github.io/amazon/index.html#subsets).

## Data Preparation

1. **Load the Datasets:** The Musical Instruments review dataset and metadata are loaded into Polars DataFrames.
2. **Join and Select Relevant Columns:** The review and metadata DataFrames are joined on the product identifier (ASIN) and the relevant columns (reviewer ID, ASIN, title, and overall rating) are selected.
3. **Filter Unique Reviewers:** The first 3000 unique reviewers are selected, and the reviews are filtered to include only those from these reviewers.
4. **Remove Duplicates:** Duplicates are removed based on reviewer ID and ASIN to ensure each reviewer-product pair is unique.
5. **Discretize Ratings:** The overall ratings are discretized into categories (negative, average, positive) based on their values.

## Data Aggregation

1. **Group by Reviewer:** The reviews are grouped by reviewer ID to count the number of reviews per reviewer. Reviewers with more than five reviews are kept for further analysis.
2. **Create Product Lists:** For each reviewer, a list of reviewed products (ASINs) is created.

## Jaccard Similarity Calculation

1. **Generate Reviewer Pairs:** All pairs of reviewers are generated using a cross join, excluding pairs where the reviewer ID is the same.
2. **Calculate Jaccard Similarity:** The Jaccard similarity is calculated between the lists of products reviewed by each pair of reviewers. This is done simultaneously for all pairs using a lazy DataFrame.

## Recommendation System

1. **Define Recommendation Function:** A function is defined to recommend products to a reviewer based on the Jaccard similarity. The function filters neighbors (reviewers with similar tastes), calculates weighted scores for products based on the neighbors' reviews, and aggregates these scores to recommend top products.
2. **Find Active Reviewers:** Active reviewers (those with a high number of neighbors above a similarity threshold) are identified.
3. **Generate Recommendations:** The recommendation function is used to suggest products to active reviewers, providing a list of top recommended products.