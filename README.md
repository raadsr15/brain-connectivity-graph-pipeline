# 🧠 Resting-State fMRI Functional Connectivity Analysis & Graph Verification

This repository implements a complete and reproducible pipeline for resting-state fMRI functional connectivity analysis and brain network verification using graph-theoretic methods. The goal of the project is to construct stable subject-level brain networks from multivariate fMRI time series and to evaluate the consistency of network properties across independent train and test datasets.

The workflow begins with preprocessing multiregional fMRI time series, where each region of interest (ROI) is standardized to remove scale differences and improve numerical stability. Functional connectivity matrices are then computed using Pearson correlation, capturing pairwise statistical dependencies between brain regions for each subject. These dense connectivity matrices are transformed into sparse brain graphs through proportional thresholding, retaining only the strongest connections while preserving inter-subject comparability.

Each subject’s brain network is modeled as an undirected, weighted graph, from which several global graph metrics are extracted. These include mean node strength, mean clustering coefficient, and global efficiency, which collectively characterize network integration, segregation, and overall communication efficiency. Graph connectivity is explicitly verified to ensure metric validity and interpretability.

A key focus of this project is train–test verification. By comparing the distributions of graph metrics between training and evaluation sets, the pipeline assesses whether functional network characteristics remain stable across sessions or data splits. Visual inspection through heatmaps, network plots, and boxplots complements quantitative summaries, providing intuitive validation of reproducibility.

---

## 📊 Dataset Overview

### 🔎 Data Description
The dataset consists of **resting-state fMRI time series recordings** from **15 subjects**, each measured across **116 brain regions** over **217 time points**.

- **Subjects:** 15  
- **Brain Regions (ROIs):** 116  
- **Time Points:** 217  
- **Temporal Resolution:** 2 seconds per time point  
- **Modality:** Resting-state fMRI  

Two NumPy arrays are provided:

- **A (Train set):** `(15, 217, 116)`
- **B (Test/Evaluation set):** `(15, 217, 116)`

Each subject appears in both sets:
> `A[i, :, :]` and `B[i, :, :]` correspond to the **same subject** recorded under different sessions/conditions.

---

## 🧠 Functional Connectivity Representation

For each subject, **functional connectivity matrices** are computed using **Pearson correlation** between all pairs of brain regions.

### 🔹 Processing Steps
1. **Z-score normalization** of each ROI time series  
2. **ROI-to-ROI correlation computation**  
3. **NaN and numerical stability handling**  

This results in one FC matrix per subject:
(Subjects, ROIs, ROIs)
(15, 116, 116)

---

## 🖼️ Functional Connectivity Matrices

### Train Set FC Matrices
<img src="images/fc_train_matrices.png" width="100%"/>

### Test Set FC Matrices
<img src="images/fc_test_matrices.png" width="100%"/>

*Each heatmap represents pairwise functional coupling between 116 brain regions.*

---

## 🕸️ Brain Network Construction

To study the **topological structure** of functional connectivity, FC matrices are converted into **weighted brain graphs**.

### 🔹 Graph Construction Strategy
- Absolute correlation values are used
- Diagonal elements are removed
- Only the **top 10% strongest connections** are retained
- Resulting graph is:
  - Undirected
  - Weighted
  - Sparse

This sparsification ensures comparability across subjects and avoids density-driven bias.

---

## 🧩 Brain Functional Connectivity Networks

### Train Set Networks (Top 10% Edges)
<img src="images/train_brain_networks.png" width="100%"/>

### Test Set Networks (Top 10% Edges)
<img src="images/test_brain_networks.png" width="100%"/>

*Each node corresponds to a brain region; edges represent strong functional interactions.*

---

## 📐 Graph-Theoretic Metrics

For each subject-level brain network, the following **global graph metrics** are computed:

### 🔹 Extracted Metrics
- **Mean Strength**  
  Average weighted degree across nodes
- **Mean Clustering Coefficient**  
  Measures local segregation and triadic connectivity
- **Global Efficiency**  
  Quantifies information integration across the network
- **Connectivity Check**  
  Verifies whether the graph is fully connected

Distances are defined as the inverse of edge weights to compute shortest paths.

---

## 🔍 Train vs Test Verification

To verify the **stability and reproducibility** of extracted brain network properties, metrics from train and test sets are statistically compared.

### 📦 Distribution Comparison
<img src="images/train_test_metrics_boxplot.png" width="100%"/>

- Boxplots compare:
  - Strength
  - Clustering coefficient
  - Global efficiency
- Close alignment indicates consistent network characteristics across datasets

---

## 📑 Final Results Table

All extracted metrics are compiled into a structured dataframe with:

- Group (Train/Test)
- Subject ID
- Number of nodes
- Number of edges
- Mean strength
- Mean clustering
- Global efficiency
- Graph connectivity status

This table enables downstream statistical analysis and reproducibility.

---
