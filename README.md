# 🧬 scRNAseq-Figure-Replication

**[BF550 Final Project](https://www.bu.edu/academics/cds/courses/cds-bf-550/)**

## Project Overview

This project replicates key figures from the study:

> Rubenstein, A. B., Smith, G. R., Raue, U., Begue, G., Minchev, K., Ruf-Zamojski, F., Nair, V. D., Wang, X., Zhou, L., Zaslavsky, E., Trappe, T. A., Trappe, S., & Sealfon, S. C. (2020). Single-cell transcriptional profiles in human skeletal muscle. Scientific reports, 10(1), 229. https://doi.org/10.1038/s41598-019-57110-6

The original paper investigates **gene signatures across mononuclear and multinucleated skeletal muscle cell types** using single-cell RNA sequencing (scRNA-seq).

Unlike bulk RNA-seq, single-cell analysis enables:

* Identification of cell-type–specific transcriptional signatures
* Detection of heterogeneous gene expression patterns
* Separation of changes in cell-type proportions from gene expression differences

---

## Objective

The goal of this project was to computationally replicate:

* **Supplementary Figure S9**
* **Figure 3a**

using:

* Hierarchical clustering
* PCA (Principal Component Analysis)
* Louvain clustering
* tSNE visualization
* Marker gene dot plots
* Heatmaps

All analyses were performed in **Python using Scanpy**.

---

## Data Source

Raw count matrices were downloaded from **GEO (Gene Expression Omnibus)**.

* Four skeletal muscle scRNA-seq count files were merged
* Final dataset contained **2,876 cells**, matching the published study

---

## Computational Workflow

### Data Processing

#### Counts Matrix Generation

* Raw count `.csv.gz` files were loaded using **pandas**
* Gene names were set as the dataframe index
* Datasets were merged using an **outer join**
* Missing values were removed
* Data was transposed to cell × gene format
* Converted into an **AnnData object**

Final AnnData object:

* 2,876 cell observations
* Gene expression matrix
* Metadata annotations

---

### Preprocessing (Scanpy)

The following Scanpy preprocessing steps were applied:

* `normalize_total()` → library size normalization
* `log1p()` → log transformation
* `highly_variable_genes()` → feature selection
* `pca(n_comps=10)` → dimensionality reduction
* `neighbors()` → graph construction using 10 PCs

---

### Unsupervised Clustering

* Louvain clustering (`resolution = 0.6`)
* Clusters initially unlabeled
* tSNE computed for visualization

```python
sc.tl.louvain()
sc.tl.tsne()
```

---

## Figure Replication

---

### A. tSNE Clustering (Figure 2A Equivalent)

Unlabeled clusters were visualized using tSNE.

Cluster identities were determined using:

* Marker gene expression
* Dot plots
* Hierarchical clustering via dendrogram

Clusters were manually annotated by mapping:

```
cluster ID → biological cell type
```

Final labeled tSNE shows:

* Endothelial subtypes
* Satellite cells
* Pericytes
* NK cells
* T + B cells
* Myeloid cells
* Smooth muscle cells
* FAP subtypes

---

### B. Marker Gene Dot Plot

To identify cell types:

* Top marker genes per cluster (from Supplementary Table 1) were used
* Scanpy `dotplot()` visualized:

  * Expression level (color intensity)
  * Fraction of expressing cells (dot size)
* Dendrogram computed using PCA correlations between clusters

This step guided manual cluster annotation.

---

### C. Hierarchical Clustering

Hierarchical clustering was computed using:

```python
sc.tl.dendrogram()
```

Clustering was based on:

* Correlation of PCA components
* Cluster-level aggregation

This matches the hierarchical grouping described in the paper.

---

### D. Heatmap (Figure 3a Replication)

A curated marker gene dictionary was constructed using:

* Top-ranked differentially expressed genes
* Supplementary Table 1 from the paper

Heatmap generated using:

```python
sc.pl.heatmap()
```

The resulting heatmap:

* Displays marker expression across annotated clusters
* Recapitulates published cell-type–specific transcriptional signatures
* Reflects hierarchical relationships between cell types

---

## Biological Interpretation

This replication demonstrates:

* Clear transcriptional separation of skeletal muscle cell subtypes
* Distinct endothelial subpopulations
* Immune cell presence (NK, T, B, Myeloid)
* Fibro-adipogenic progenitor (FAP) heterogeneity
* Satellite cell signature specificity

The results confirm the original study’s findings regarding cell-type–specific gene expression patterns.

---

## Technologies Used

* Python
* Scanpy
* Pandas
* NumPy
* scikit-learn
* Matplotlib
* Seaborn
* SciPy

---

## Key Learning Outcomes

This project demonstrates:

✔ Single-cell RNA-seq preprocessing
✔ AnnData object construction
✔ Feature selection via highly variable genes
✔ Dimensionality reduction (PCA, tSNE)
✔ Graph-based clustering (Louvain)
✔ Marker-based cell type annotation
✔ Hierarchical clustering
✔ Figure replication from published literature

