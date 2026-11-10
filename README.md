# PCA and k-Means on a Young People Survey

An analysis of survey answers about hobbies, music and film tastes, spending habits, personality and phobias, using **Principal Component Analysis** to find the main "personality axes" in the data and **k-Means** to group respondents along them. Built as a homework for the course **Computational Linear Algebra for Large Scale Problems** (Politecnico di Torino, MSc, academic year 2025/2026).

## What's inside

| File | Description |
| --- | --- |
| `PCA_Homework.ipynb` | The full notebook with code, plots and written comments. |
| `PCA_Homework.pdf` | A PDF export of the same notebook (35 pages), the easiest way to read it. The notebook is about 8 MB because of the embedded plots, so GitHub may not preview it. |

## The analysis, step by step

1. **Preprocessing.** 93 survey features are used. The two categorical ones (`Smoking`, `Internet usage`) have a natural order, so they are ordinal-encoded; everything is then standardised to zero mean and unit variance with `StandardScaler`.
2. **Variance and principal components.** Feature variances and cumulative explained variance are compared before and after scaling, showing why PCA on unscaled data would be dominated by large-range features.
3. **Dimensionality reduction.** The data is projected onto the first 5 principal components, which keep 24.03% of the total variance. Each component is named from its strongest loadings:

   | PC | Name | Strongest loadings |
   | --- | --- | --- |
   | 1 | Classical intellectual | reading, art exhibitions, theatre, classical music, opera |
   | 2 | Rational adventurer | maths, PC, science and technology, adrenaline sports; few phobias |
   | 3 | Popular trend icon | shopping, branded clothing, spending on looks, pop, dance |
   | 4 | Happy natural | happiness and energy; low loneliness, anger and internet usage |
   | 5 | Digital pragmatist | finances, internet, PC, techno, sci-fi |

4. **Clustering.** k-Means is run on the 5-dimensional scores for k = 3 to 10; the silhouette score picks **k = 3** (0.157). The clusters are named from their centroids:

   | Cluster | Name | Centroid highlights | Silhouette |
   | --- | --- | --- | --- |
   | 1 | Classical adventurer | high PC1 (1.85) and high PC2 (1.68) | 0.145 |
   | 2 | Anti-academic | very low PC1 (-2.83) | 0.138 |
   | 3 | Cautious humanist | very low PC2 (-1.98), moderate PC1 (0.88) | 0.187 |

5. **External evaluation.** Clusters are compared against demographic labels (gender, education, only child, age, home-town type). Gender is the only label that separates clearly across clusters.
6. **Internal evaluation.** Per-cluster silhouette plots show that "Cautious humanist" is the most cohesive group.

The silhouette values are low in absolute terms, so the clusters overlap considerably; they describe tendencies rather than sharply separated groups.

## Running it

```bash
pip install -r requirements.txt
jupyter notebook
```

Two things to know before re-running:

- **Data files are not included.** The notebook reads `responses_hw.csv` and `columns_hw.csv`, which were distributed through the course and are not redistributed here. They are a version of the public *Young People Survey* dataset (available on Kaggle). Place both files next to the notebook.
- **The seed was replaced.** In the original assignment each student's ID number is used as the random seed that selects a personal subset of rows and features. That ID has been replaced with a placeholder (`123456`) in this public copy, so re-running the notebook selects a different subset and gives different numbers from the saved outputs. The outputs and plots stored in the notebook and PDF are the original ones.

## Tools

Python, NumPy, pandas, scikit-learn (`OrdinalEncoder`, `StandardScaler`, `PCA`, `KMeans`, silhouette metrics), Matplotlib.

## Author

Pietro Fabi
