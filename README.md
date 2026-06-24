# Hepatocellular Carcinoma Differential Expression Analysis with recount3

## Overview

This project performs a differential gene expression analysis of hepatocellular carcinoma (HCC) using publicly available RNA-seq data accessed through the recount3 resource.

The workflow leverages edgeR and limma-voom to identify transcriptional differences between tumor and matched normal liver tissue while accounting for patient-specific effects through a paired experimental design.

---

## Dataset

**Project:** SRP068976

**Study:** RNA-seq of paired hepatocellular carcinoma samples

**Source:** recount3

**Samples:**

* 50 Normal liver samples
* 49 Tumor liver samples
* 99 total RNA-seq samples

All samples correspond to liver tissue, allowing a direct comparison between tumor and normal conditions.

---

## Objectives

* Retrieve RNA-seq data from recount3.
* Explore and curate project metadata.
* Perform quality assessment.
* Filter lowly expressed genes.
* Normalize count data.
* Model paired tumor-normal samples.
* Identify differentially expressed genes.
* Visualize global transcriptomic variation.

---

## Workflow

```text
recount3
    ↓
Metadata Exploration
    ↓
Quality Assessment
    ↓
Gene Filtering
    ↓
edgeR Normalization
    ↓
limma-voom Transformation
    ↓
Linear Modeling
    ↓
Differential Expression Analysis
    ↓
Visualization & Interpretation
```

---

## Methods

### Data Retrieval

The dataset was downloaded directly from recount3 using the Bioconductor package:

* recount3

A `RangedSummarizedExperiment` object was constructed containing:

* 63,856 annotated features
* 99 RNA-seq samples

---

### Experimental Design

Samples were classified according to:

```text
Diagnosis
├── Normal
└── Tumor
```

Because the study contains matched tumor-normal pairs from the same patients, patient identity was incorporated into the statistical model.

Design formula:

```R
~ patient + diagnosis
```

This approach isolates tumor-associated transcriptional changes while controlling for inter-individual variation.

---

### Quality Control

Quality assessment included:

* Assigned gene proportion
* Sample-level QC metrics
* Diagnosis-specific QC comparisons

Results showed:

* Median assigned gene proportion ≈ 77.8%
* Mean assigned gene proportion ≈ 77.1%

No systematic quality differences were observed between tumor and normal samples.

---

### Gene Filtering

Lowly expressed genes were removed prior to analysis:

```R
gene_means > 0.1
```

This step reduces statistical noise and improves power.

---

### Normalization

Normalization was performed using:

* edgeR
* TMM normalization (`calcNormFactors()`)

The normalized counts were subsequently transformed using:

* limma-voom

to estimate the mean-variance relationship and prepare the data for linear modeling.

---

### Differential Expression Analysis

Linear models were fitted using:

* limma
* empirical Bayes moderation (`eBayes()`)

Primary contrast:

```text
Tumor vs Normal
```

Statistical significance was evaluated using:

```text
FDR < 0.05
```

---

## Results

### Differentially Expressed Genes

After multiple-testing correction:

```text
27,995 significant genes
(FDR < 0.05)
```

were identified as differentially expressed between tumor and normal liver tissue.

Representative highly significant genes include:

* CDCA8
* CDKN3
* KIFC1
* KIF20A
* NUF2
* PTTG1

These genes are associated with cell-cycle progression and proliferative processes frequently altered in cancer.

---

### Visualization

The analysis includes:

* Mean-variance trend (voom)
* MA plot
* Volcano plot
* Heatmap of top 50 DEGs
* Multidimensional scaling (MDS)

The heatmap and MDS analyses demonstrate a clear separation between tumor and normal samples, indicating strong global transcriptional differences associated with disease state.

---

## Repository Structure

```text
.
├── data/
├── scripts/
├── figures/
├── results/
├── report/
└── README.md
```

---

## Technologies

* R
* Bioconductor
* recount3
* edgeR
* limma
* voom
* ggplot2
* pheatmap

---

## Skills Demonstrated

* RNA-seq analysis
* Differential expression analysis
* Cancer transcriptomics
* Bioconductor workflows
* Statistical modeling
* Paired experimental design
* High-throughput sequencing analysis
* Data visualization
* Reproducible research

---

## References

* Collado-Torres et al. (2017). Reproducible RNA-seq Analysis Using Recount2.
* Law et al. (2014). Voom: Precision Weights Unlock Linear Model Analysis Tools for RNA-seq Read Counts.
* Ritchie et al. (2015). Limma Powers Differential Expression Analyses for RNA-Sequencing and Microarray Studies.
* Robinson et al. (2010). edgeR: A Bioconductor Package for Differential Expression Analysis of Digital Gene Expression Data.
