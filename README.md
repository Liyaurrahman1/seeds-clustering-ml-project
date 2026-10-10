# Seeds Clustering: Discovering Wheat Varieties Without Labels

## Project overview

This project explores whether **unsupervised machine learning** can discover meaningful groups of wheat kernels without using their known variety labels. Using the Seeds dataset, we apply Principal Component Analysis (PCA) for visualization and K-Means for clustering, then compare the resulting clusters with the true varieties **only after the clustering decisions have been made**.

**Central question:** *How much of the real class structure can clustering recover without using the labels?*

## Dataset

The **Seeds dataset** contains **210 wheat kernels**, measured using seven numerical features:

| Feature | Description |
| --- | --- |
| `area` | Kernel area |
| `perimeter` | Kernel perimeter |
| `compactness` | Kernel compactness |
| `kernel_length` | Kernel length |
| `kernel_width` | Kernel width |
| `asymmetry` | Asymmetry coefficient |
| `groove_length` | Length of the kernel groove |

The dataset also contains true variety labels (1, 2, and 3). These labels are held aside during PCA, cluster-number selection, and K-Means fitting; they are used only for subsequent evaluation.

## Method

1. **Preprocess:** Standardize the seven numerical features using `StandardScaler` to make distance-based clustering less sensitive to differences in feature scale.
2. **Visualize:** Fit PCA to the standardized measurements and plot the first two principal components, initially without using variety labels.
3. **Choose the number of clusters:** Fit K-Means for **k = 2 to 7**, inspecting inertia (elbow method) and silhouette scores. Use `n_init=10` and `random_state=42` for reproducibility.
4. **Cluster:** Select **k = 3** based on the elbow plot and fit K-Means using all seven standardized features (not only the two PCA components).
5. **Evaluate:** Reveal true variety labels and examine a cross-tabulation, cluster purity, and a PCA plot colored by variety.
6. **Interpret:** Convert cluster centers back to the original feature units to describe the physical characteristics of each group.

## Results

### PCA

The first two principal components explain:

- **PC1:** 71.87% of total variance
- **PC2:** 17.11% of total variance
- **Combined:** **88.98%** of total variance

The two-dimensional projection shows visible group structure, although some observations overlap.

### Choosing k

The elbow plot shows a pronounced improvement from **k = 2 to k = 3**, followed by smaller reductions in inertia. The **highest silhouette score occurs at k = 2 (approximately 0.466)**; the score for **k = 3 is approximately 0.400**. We selected **k = 3** as a reasonable compromise that captures more apparent structure, while acknowledging that k = 2 separates more strongly according to the silhouette metric.

### Cluster sizes

| Cluster | Kernels |
| --- | ---: |
| 0 | 72 |
| 1 | 67 |
| 2 | 71 |
| **Total** | **210** |

### Agreement with the true varieties

The following cross-tabulation was computed **after** fitting the clusters:

| Cluster | Variety 1 | Variety 2 | Variety 3 | Dominant variety |
| --- | ---: | ---: | ---: | --- |
| 0 | 6 | 0 | 66 | Variety 3 |
| 1 | 2 | 65 | 0 | Variety 2 |
| 2 | 62 | 5 | 4 | Variety 1 |

**Purity = (66 + 65 + 62) / 210 = 0.9190 (91.90%).** In other words, **193 of the 210 kernels** belong to the majority variety within their assigned cluster. Variety 2 is grouped especially cleanly, while there is some overlap between Varieties 1 and 3. Purity describes alignment with the majority varieties; it is not a measure of independent predictive accuracy.

### Cluster profiles

The K-Means centers were inverse-transformed into the original measurement units:

| Cluster | Area | Perimeter | Compactness | Kernel length | Kernel width | Asymmetry | Groove length |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| 0 | 11.857 | 13.248 | 0.848 | 5.232 | 2.850 | 4.742 | 5.102 |
| 1 | 18.495 | 16.203 | 0.884 | 6.176 | 3.698 | 3.632 | 6.042 |
| 2 | 14.438 | 14.338 | 0.882 | 5.515 | 3.259 | 2.707 | 5.121 |

- **Cluster 0:** Relatively small kernels with higher asymmetry (predominantly Variety 3).
- **Cluster 1:** Largest kernels by area and other size measurements (predominantly Variety 2).
- **Cluster 2:** Medium-sized kernels with the lowest mean asymmetry (predominantly Variety 1).

## Interpretation and limitations

Clustering recovered much of the known variety structure **without using labels to build the groups**. Yet the results do not justify treating the clusters as definitive wheat varieties: some varieties overlap, and the preferred number of clusters depends on the diagnostic used. In a real grain-sorting workflow, cluster assignments should be checked with additional evidence before decisions are made. Furthermore, the PCA plot is a two-dimensional summary; K-Means clustering used all seven scaled features.

## Reflection

A useful lesson from this project is that **cluster-number selection involves judgment**: the elbow method favored three groups, while the silhouette metric favored two. Standardizing the measurements and keeping variety labels out of the clustering stage helped make the analysis interpretable and avoided using the answers to choose the model. With more time, a useful extension would be to examine the stability of clusters across random seeds or compare K-Means with a different clustering method, while still keeping labels hidden until final evaluation.

## How to run

1. Open `Final_Project_Option3_Seeds_Clustering.ipynb` in **Google Colab** or Jupyter Notebook.
2. Run the cells from top to bottom. The starter notebook downloads the dataset on first execution, so an internet connection is needed for that step.
3. Inspect the PCA figures, elbow and silhouette diagnostics, cross-tabulation, purity, and cluster profiles in the notebook output.

**Libraries:** NumPy, pandas, Matplotlib, and scikit-learn (as used in the notebook).

## Files

- `Final_Project_Option3_Seeds_Clustering.ipynb` — Completed notebook with code, interpretation, and visible outputs.
- `README.md` — Portfolio overview, approach, findings, limitations, and reflection.

