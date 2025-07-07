# Spectral Clustering for Ad Keyword Analysis

[![Python Version](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository contains a detailed Jupyter Notebook that explores spectral clustering, first on a synthetic dataset to demonstrate core principles, and then on a real-world bipartite graph from a Yahoo advertising dataset to uncover thematic clusters of ad keywords.

## Project Overview

The primary goal of this analysis is to apply graph clustering techniques to a bipartite graph of companies and the advertising terms they purchase. By representing this relationship as a graph, we can leverage the properties of the graph's Laplacian matrix to identify "communities"—groups of highly related advertising terms. These clusters can provide valuable insights for market segmentation, ad recommendation, and understanding competitive landscapes.

The project is structured into two main parts:
1.  A warm-up exercise on a synthetic stochastic block model to visually prove the concept of using the Fiedler vector for graph partitioning.
2.  A deep dive into a real-world Yahoo dataset, including a robust methodology for clustering and interpreting the results.

### Key Features
- **Eigengap Heuristic:** Practical use of the Laplacian's eigenvalue spectrum to determine a natural number of clusters.
- **Comparative Analysis:** Implements two powerful clustering approaches: K-Means on the Fiedler vector and the `SpectralClustering` algorithm from Scikit-learn.
- **Quantitative & Qualitative Evaluation:** Clusters are evaluated using the Silhouette Score and Adjusted Rand Index, and interpreted by examining the terms within them.
- **Rich Visualization:** Includes t-SNE plots to visualize cluster separation and heatmaps of the reordered similarity matrix to demonstrate the effectiveness of the partitioning.

---

## Dataset

The data is from an anonymized 2003 Yahoo advertising dataset, describing a bipartite graph of **2000 companies** and **3000 advertising terms**.

-   `yahoo/us.3k.2k.smat`: A sparse matrix file representing the connections. An entry `(i, j)` indicates that company `j` buys advertising term `i`.
-   `yahoo/us.3k.2k.trms`: A text file containing the names of the 3000 advertising terms, corresponding to the rows of the matrix.

## Methodology

The core of the analysis involves the following steps:

1.  **Graph Construction:** The company-term data is used to build the adjacency matrix `A` of a large bipartite graph containing both companies and terms as nodes.
2.  **Laplacian Matrix:** The graph Laplacian `L = D - A` is computed, where `D` is the diagonal degree matrix. This is done efficiently using `scipy.sparse` matrices.
3.  **Eigendecomposition:** The smallest eigenvalues and corresponding eigenvectors of `L` are calculated. The "eigengap" (the gap between consecutive eigenvalues) suggests that `k=2` and `k=3` are strong candidates for the number of clusters.
4.  **Fiedler Vector:** The eigenvector corresponding to the second-smallest eigenvalue (the Fiedler vector) is extracted. Its values provide a 1D embedding of the graph's nodes that is ideal for partitioning.
5.  **Clustering:** The advertising terms are clustered into `k` groups using:
    *   **Fiedler-based K-Means:** Applying K-Means to the term components of the Fiedler vector.
    *   **Scikit-learn SpectralClustering:** Using the `SpectralClustering` algorithm on a precomputed term-term affinity matrix (`A_terms.T @ A_terms`).
6.  **Evaluation & Interpretation:** The resulting clusters are visualized with t-SNE, evaluated with quantitative metrics, and interpreted by analyzing their constituent keywords to reveal their underlying themes.

## Summary of Results

The analysis successfully identified meaningful partitions in the data. Partitioning the terms into **two clusters (k=2)** yielded the most robust and interpretable results.

-   **Cluster 1 (Broad Commerce):** A large, comprehensive cluster containing a wide variety of general commercial products, services, and brand names (e.g., `accessory computer`, `12a1970 lexmark`).
-   **Cluster 2 (Online Services & Ads):** A smaller, more thematically focused cluster centered on online services, web advertising, and personal ads (e.g., `advertise site web`, `accept card credit online`, `ads personal single`).

This `k=2` partition was highly consistent across both clustering methods (Adjusted Rand Index ≈ 0.99) and earned a higher Silhouette Score, indicating a clear and natural division in the data. The results demonstrate the power of spectral clustering in extracting actionable business intelligence from complex, unstructured graph data.

---

## How to Run

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/Spectral-Clustering-for-Ad-Keywords.git
cd Spectral-Clustering-for-Ad-Keywords
```

### 2. Install Dependencies
This project requires several Python libraries. A virtual environment is recommended. Install the dependencies using pip:

```bash
pip install numpy pandas matplotlib scipy scikit-learn seaborn jupyterlab
```

### 3. Run the Jupyter Notebook
The dataset is included in the `yahoo/` directory. Start Jupyter and open the notebook to execute the analysis and reproduce the results.

```bash
jupyter lab Analysis.ipynb
```

## License

This project is licensed under the MIT License.
