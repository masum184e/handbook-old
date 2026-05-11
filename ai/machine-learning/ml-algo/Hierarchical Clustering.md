## Hierarchical Clustering

Hierarchical clustering is an unsupervised machine learning technique used to group similar data points into clusters based on their similarity or distance. Unlike methods such as K-means, it does not require you to specify the number of clusters in advance.

The result is usually represented as a **tree-like structure** called a **dendrogram**, which shows how clusters are merged or divided step by step.

**Core Idea**

Hierarchical clustering builds a hierarchy of clusters:

* Similar points are grouped first
* Less similar groups are merged later
* The process continues until all points belong to one cluster (agglomerative)
  or until every point becomes isolated (divisive)

## Types of Hierarchical Clustering

There are two major approaches:

### Agglomerative Hierarchical Clustering (Bottom-Up)

This is the most common type.

**Process**

1. Start with each data point as its own cluster
2. Compute distances between clusters
3. Merge the two closest clusters
4. Recompute distances
5. Repeat until only one cluster remains

**Example**

Suppose we have points:

$$
A, B, C, D
$$

Initially:

$$
\{A\},\{B\},\{C\},\{D\}
$$

If A and B are closest:

$$
\{A,B\},\{C\},\{D\}
$$

Then maybe C joins D:

$$
\{A,B\},\{C,D\}
$$

Finally:

$$
\{A,B,C,D\}
$$

### Divisive Hierarchical Clustering (Top-Down)

Less commonly used.

**Process**

1. Start with all points in one cluster
2. Split the cluster recursively
3. Continue until each point becomes its own 

**Example**

Suppose we have points:

$$
A, B, C, D
$$

Initially:

$$
\{A, B, C, D\}
$$

First Split:

$$
\{A,B\},\{C,D\}
$$

Further Split:

$$
\{A,B\},\{C\},\{D\}
$$

Finally:

$$
\{A\}, \{B\}, \{C\}, \{D\}
$$

##  Distance Measures

Hierarchical clustering depends heavily on distance metrics.

Common distance measures include:

### Euclidean Distance

Most common for continuous data.

$$
d(x,y)=\sqrt{\sum_{i=1}^{n}(x_i-y_i)^2}
$$

Measures straight-line distance.

### Manhattan Distance

$$
d(x,y)=\sum_{i=1}^{n}|x_i-y_i|
$$

Measures grid-like distance.

### Cosine Distance

Used in text mining and NLP.

Based on angle between vectors.

## Linkage Methods

After deciding distances between points, we must define distance between clusters.

This is called **linkage criteria**.

### Single Linkage

Distance between two closest points in clusters.

$$
D(A,B)=\min(d(a,b))
$$

**Characteristics**

* Can detect irregular shapes
* Sensitive to noise
* Causes chaining effect

### Complete Linkage

Distance between two farthest points.

$$
D(A,B)=\max(d(a,b))
$$

**Characteristics**

* Produces compact clusters
* More robust to noise

### Average Linkage

Average pairwise distance.

$$
D(A,B)=\frac{1}{|A||B|}\sum d(a,b)
$$

**Characteristics**

* Balanced approach
* Commonly used

### Ward’s Method

Minimizes variance within clusters.

Tries to minimize increase in total within-cluster variance.

Very popular in practice.

## Dendrogram

A dendrogram is the visual representation of hierarchical clustering.

It shows:

* Which clusters merge
* At what distance they merge
* The hierarchy among clusters

Example structure:

```text
          ---------
         |         |
      ----       ----
     |    |     |    |
     A    B     C    D
```

Height indicates cluster distance.

Higher merge height = less similarity.

## Working of Hierarchical Clustering

Suppose we have 5 points:

| Point | Coordinates |
| ----- | ----------- |
| A     | (1,1)       |
| B     | (2,1)       |
| C     | (4,3)       |
| D     | (5,4)       |
| E     | (8,8)       |

### Compute Distance Matrix

Using Euclidean distance.

Approximate full distance matrix:

|   | A    | B    | C    | D    | E    |
| - | ---- | ---- | ---- | ---- | ---- |
| A | 0    | 1    | 3.61 | 5    | 9.9  |
| B | -    | 0    | 2.83 | 4.24 | 9.22 |
| C | -    | -    | 0    | 1.41 | 6.40 |
| D | -    | -    | -    | 0    | 5    |
| E | -    | -    | -    | -    | 0    |

### Agglomerative Clustering (Bottom-Up)

1. Start with single clusters

$$
\{A\}, \{B\}, \{C\}, \{D\}, \{E\}
$$

2. First merge(smallest distance)
    * A and B → distance = 1
$$
\{A, B\}, \{C\}, \{D\}, \{E\}
$$

3. Next Merge
    * C and D → 1.41
$$
\{A, B\}, \{C, D\}, \{E\}
$$

4. Merge clusters

    Now compute distance between clusters:

    Distance between $\{A,B\}$ and $\{C,D\}$ (single linkage minimum):
B → C = 2.828 (smallest)

    So merge:
$$
\{A, B, C, D\}, \{E\}
$$

5. Final merge

    Distance from E to cluster $\{A,B,C,D\}$: E → D = 5.0 (minimum)

    So final merge:

$$
\{A, B, C, D, E\}
$$

### Choosing Number of Clusters

You can “cut” the dendrogram at a chosen height.

Lower cut:

* More clusters

Higher cut:

* Fewer clusters

Example:

```text
Height
  |   ---------------
  |   |             |
  |   E             |
  |   |         ---------
  |   |        |         |
  |   |     ----       ----
  |   |    |    |     |    |
  |   |    A    B     C    D
  |
  +------------------------
```

Cutting midway gives 2 clusters:

* {A,B,C,D}
* {E}

## Question

<details>
    <summary>What happens if noise or outliers are present in hierarchical clustering?</summary>
</details>
<details>
    <summary>Why does hierarchical clustering not require specifying the number of clusters beforehand?</summary>
</details>
<details>
    <summary>Compare hierarchical clustering with K-means clustering.</summary>
</details>
<details>
    <summary>Question 4</summary>
    
Suppose:

Cluster A:
$$
(1,1), (2,2)
$$

Cluster B:
$$
(5,5), (6,6)
$$

Compute:

1. Single linkage distance
2. Complete linkage distance
3. Average linkage distance

</details>
<details>
    <summary>Explain why single linkage may produce chain-like clusters.</summary>
</details>
<details>
    <summary>Question 5</summary>
    
Perform agglomerative hierarchical clustering on:

| Point | Coordinates |
| ----- | ----------- |
| A     | (1,1)       |
| B     | (2,1)       |
| C     | (4,3)       |
| D     | (5,4)       |

Tasks:

1. Compute the distance matrix
2. Identify the first merge
3. Update distances using single linkage
4. Draw the dendrogram
</details>
<details>
    <summary>Question 6</summary>
    
A dendrogram shows:

* A and B merge at height 1
* C joins at height 3
* D joins at height 8

Interpret the clustering structure.

</details>
<details>
    <summary>How can the number of clusters be determined from a dendrogram?</summary>
</details>
<details>
    <summary>What does a large vertical gap in a dendrogram indicate?</summary>
</details>
<details>
    <summary>Show mathematically why hierarchical clustering has higher computational complexity than K-means.</summary>
</details>
<details>
    <summary>Question 7</summary
                 
Prove that Euclidean distance satisfies:

1. Non-negativity
2. Symmetry
3. Triangle inequality
</details>