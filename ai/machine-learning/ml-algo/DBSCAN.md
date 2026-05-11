## DBSCAN

**DBSCAN** stands for **Density-Based Spatial Clustering of Applications with Noise**.

It is a clustering algorithm used in machine learning and data mining to group together points that are **closely packed**, while marking points in sparse regions as **outliers (noise)**.

Unlike algorithms such as K-Means, DBSCAN:

* does **not require specifying the number of clusters beforehand**
* can find **arbitrarily shaped clusters**
* naturally detects **noise/outliers**

DBSCAN is especially useful when:

* clusters are irregularly shaped
* data contains noise
* you do not know the number of clusters in advance

**Core Idea**

DBSCAN defines clusters based on **density**.

A region is considered dense if it contains enough nearby points.

The algorithm uses two important parameters:


## Important Terminology

### Epsilon (ε)

This is the **maximum distance** between two points for them to be considered neighbors.

Think of ε as the radius of a circle around each point.

### MinPts

Minimum number of neighboring points required to form a dense region.

If a point has at least `MinPts` neighbors within distance ε, it becomes a **core point**.

## Types of Points in DBSCAN

DBSCAN classifies points into 3 categories.

### Core Point

A point is a core point if:

* it has at least `MinPts` points within ε distance

Example:

If `MinPts = 5`, and a point has 6 nearby points inside ε radius, it is a core point.

### Border Point

A point that:

* has fewer than `MinPts` neighbors
* but lies within ε distance of a core point

It belongs to a cluster but does not expand the cluster.

### Noise Point (Outlier)

A point that:

* is neither a core point nor a border point

These points are treated as anomalies.

## Visual Intuition

Imagine stars in space.

* Dense star groups → clusters
* Isolated stars → noise

DBSCAN grows clusters outward from dense regions.

## Mathematical Concept

A point $ q $ is directly density reachable from point $ p $ if:

* $ q $ lies within ε neighborhood of $ p $
* $ p $ is a core point

The ε-neighborhood is:
$$
N_{\varepsilon}(p)={q\in D\mid dist(p,q)\leq \varepsilon}
$$

Where:

* $ D $ = dataset
* $ dist(p,q) $ = distance between points

## Working of DBSCAN Algorithm

Consider the following 2D points:

| Point | Coordinates |
| ----- | ----------- |
| A     | (1,1)       |
| B     | (1,2)       |
| C     | (2,1)       |
| D     | (2,2)       |
| E     | (8,8)       |
| F     | (8,9)       |
| G     | (9,8)       |
| H     | (25,25)     |

Suppose:

* ε = 1.5
* MinPts = 3


### Cluster Around A

Pick an Unvisited Point($A$), and check how many neighbors it has within ε(1.5) radius.

Nearby points:

* B
* C
* D

Total neighbors = 4 including A itself.

Thus:

* A is a core point
* A start a new cluster

### Expand Cluster

Now examine B, C, D(neighbors of A).

Each also has enough neighbors.

So all become core points and belong to:

**Cluster 1**

$$
\{A,B,C,D\}
$$

### Move to E
Move to E as $\{A,B,C,D\}$ already visited.

Neighbors within ε:

* F
* G

Including itself = 3 points.

Thus:

* E is a core point

Similarly F and G are connected.

So:

### Cluster 2

$$
{E,F,G}
$$

## Move to H

H has no nearby neighbors.

Thus:

* H is labeled as noise/outlier

### Final Result

| Cluster   | Points  |
| --------- | ------- |
| Cluster 1 | A,B,C,D |
| Cluster 2 | E,F,G   |
| Noise     | H       |

## Why DBSCAN Works Well

1. Finds Arbitrary Shapes  
  K-Means prefers circular clusters where DBSCAN can discover:
    * spirals
    * curves
    * irregular blobs
2. Handles Noise Naturally   
  Outliers are automatically separated.  
  Useful in:
    * fraud detection
    * anomaly detection
    * sensor data cleaning
3. No Need to Specify Number of Clusters
  - Unlike K-Means.
  - DBSCAN determines clusters automatically.

## Choosing Parameters

### Choosing ε

Common methods:

**k-distance graph**

1. Compute distance to k-th nearest neighbor
2. Sort distances
3. Look for elbow point

The elbow gives a good ε.

### Choosing MinPts

Rule of thumb:

$$
MinPts \geq dimensions + 1
$$

Often:

* 4 for 2D data
* 6+ for higher dimensions



## Additional

### Advantages of DBSCAN

| Advantage        | Explanation                            |
| ---------------- | -------------------------------------- |
| No need for K    | Automatically determines cluster count |
| Handles noise    | Detects outliers                       |
| Arbitrary shapes | Finds non-circular clusters            |
| Robust           | Works well for spatial data            |

### Disadvantages of DBSCAN

| Limitation                       | Explanation                                      |
| -------------------------------- | ------------------------------------------------ |
| Sensitive to ε                   | Bad ε causes poor clustering                     |
| Struggles with varying densities | Dense and sparse clusters together are difficult |
| High-dimensional data issue      | Distance becomes less meaningful                 |

### DBSCAN vs K-Means

| Feature                     | DBSCAN    | K-Means          |
| --------------------------- | --------- | ---------------- |
| Need K?                     | No        | Yes              |
| Handles noise?              | Yes       | No               |
| Shape flexibility           | Arbitrary | Mostly spherical |
| Sensitive to initialization | No        | Yes              |
| Works with varying density  | Limited   | Poor             |

### Time Complexity

Using spatial indexing structures like KD-Trees:

$$
O(n \log n)
$$

Without indexing:

$$
O(n^2)
$$

