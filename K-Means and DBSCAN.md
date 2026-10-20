
## 1. K- Means

K-Means is a iterative, distance-based clustering algorithm. Its primary goal is to divide a dataset into a pre-defined number ($K$) of distinct, non-overlapping clusters. Every data point belongs to the cluster with the nearest mean (centroid).

### Algorithm Walkthrough

1. **Select** $K$ **(Number of Clusters):** Choose the number of clusters you want the algorithm to identify.
    
2. **Initialize Centroids:** Randomly select $K$ distinct data points from the dataset to act as the initial cluster centers (centroids).
    
3. **Measure Distances:** Compute the distance between every data point and all $K$ centroids using Euclidean distance:
    
    $$\text{Euclidean Distance} = \sqrt{\sum_{i=1}^{n} (x_i - y_i)^2}$$
4. **Assign Points:** Assign each data point to its closest centroid.
    
5. **Update Centroids:** Calculate the mean of all data points assigned to each cluster. These new mean values become the updated centroids.
    
6. **Iterate:** Repeat steps 3 to 5 until the centroids no longer change location (convergence) or a maximum number of iterations is reached.
    

### Assessing Cluster Quality & Finding Optimal $K$

Because K-Means can converge to different results based on its random initialization, the algorithm runs multiple times with different starting centroids, keeping track of the best iteration using **total within-cluster variation (Inertia)**.

**Sum of Squared Errors (Inertia)**

$$\text{Inertia} = \sum_{k=1}^{K} \sum_{x \in C_k} \Vert{} x - \mu_k \Vert{}^2$$

Where $C_k$ is the set of points in cluster $k$, and $\mu_k$ is the mean (centroid) of cluster $k$.

**The Elbow Method**

To find the ideal number of clusters $K$:

- Calculate total inertia for different values of $K$ (e.g., $K = 1, 2, 3, \dots, N$).
    
- Plot $K$ versus Inertia.
    
- As $K$ increases, inertia naturally decreases (reaching 0 when $K$ equals the number of samples).
    
- Look for the **"elbow"** in the plot—the point where adding another cluster yields diminishing returns in variance reduction.
    
### Strengths and Limitations of K-Means

|Strengths|Limitations|
|---|---|
|Simple to understand and implement.|Must specify $K$ in advance.|
|Computationally efficient for large datasets.|Sensitive to initial random centroid locations.|
|Works well when clusters are spherical and well-separated.|Performs poorly on non-spherical, nested, or complex cluster geometry.|
||Sensitive to outliers (which distort the mean).|

## 2. DBSCAN (Density-Based Spatial Clustering of Applications with Noise)


DBSCAN is a density-based clustering algorithm. Instead of assuming spherical shapes or requiring a pre-specified number of clusters, DBSCAN defines clusters as continuous regions of high point density separated by areas of low density. It can discover arbitrary shapes and automatically isolate noise/outliers.

#### Core Hyperparameters

DBSCAN requires two main parameters:

1. **`eps` (**$\epsilon$ **/ Search Radius):** The maximum distance between two samples for one to be considered as in the neighborhood of the other.
    
2. **`min_samples` (Minimum Points):** The minimum number of data points required within the $\epsilon$-neighborhood to classify a point as a **Core Point**.
    

### Point Categorization

DBSCAN categorizes every data point into one of three types:

1. **Core Point:** A point that has at least `min_samples` points within its `eps` radius. Core points form the heart of a cluster.
    
2. **Border Point:** A point that has fewer than `min_samples` points within its `eps` radius, but lies within the `eps` neighborhood of a Core Point. Border points join clusters but cannot expand them.
    
3. **Noise / Outlier:** A point that is neither a Core Point nor within the `eps` radius of any Core Point.
    
### Algorithm Walkthrough

1. **Point Evaluation:** For every point in the dataset, count how many neighboring points lie within distance `eps`. Mark points meeting or exceeding `min_samples` as Core Points.
    
2. **Cluster Creation:** Select an unvisited Core Point to initiate a new cluster.
    
3. **Cluster Expansion:**
    
    - Add all neighboring points within `eps` to the current cluster.
        
    - If a neighbor is also a **Core Point**, recursively add all of _its_ neighbors to the cluster, expanding the cluster boundary.
        
    - If a neighbor is a **Border Point**, add it to the cluster but do not use it to expand further.
        
4. **Repeat:** Repeat the process for all remaining unassigned Core Points.
    
5. **Mark Outliers:** Designate all unassigned points (not reachable from any core point) as noise/outliers (labeled as `-1`).
    

### Feature Scaling Requirement

Because DBSCAN evaluates point density using distance calculations across all features simultaneously, **feature scaling (e.g., Z-score Standardization)** is mandatory prior to training. Without scaling, features with larger numeric magnitudes will dominate the distance metrics.

$$\text{Z-Score Standardized Feature} = \frac{x - \mu}{\sigma}$$

### Strengths and Limitations of DBSCAN

|Strengths|Limitations|
|---|---|
|Does not require pre-defining the number of clusters.|Requires careful tuning of `eps` and `min_samples`.|
|Can identify arbitrary, complex, and nested cluster structures.|Struggles with datasets containing varying densities.|
|Built-in outlier and noise detection.|Sensitive to distance metric choice in very high-dimensional spaces.|

# 3. K-Means vs. DBSCAN Comparison

|Feature|K-Means|DBSCAN|
|---|---|---|
|**Cluster Shape**|Spherical / Convex|Arbitrary shapes (e.g., nested, ring-shaped)|
|**Number of Clusters**|Pre-specified ($K$)|Discovered automatically|
|**Outlier Handling**|Forces outliers into a cluster|Explicitly identifies noise points|
|**Core Concept**|Distance to Centroids|Local Density|
|**Preprocessing Requirement**|Standard scaling recommended|Standard scaling **mandatory**|
