# Single-Cell RNA-Seq Processing and Integration Pipeline

This repository contains a complete Python pipeline for processing, filtering, and integrating single‑cell RNA‑sequencing data using Scanpy. 

The pipeline is designed to run in JupyterLab (or Google Colab) and includes the following major steps:
* **Data Loading:** Load 10x Genomics H5 files for multiple conditions (e.g., Control1, Control2, and IAV) into AnnData objects and assign sample labels.
* **Quality Control & Filtering:** Compute key QC metrics including total counts (n_counts), number of detected genes (n_genes), and percent mitochondrial reads (percent_mito). Cells are filtered based on:
	* Minimum and maximum detected genes (e.g., 200 to 7500 genes per cell)
	* Total UMI counts between 200 and 60,000
	* Mitochondrial percentage (below 15%)
* **Gene filtering:** Only retain genes expressed in at least a specified number of cells
* **Doublet Detection:** Run doublet detection using Scrublet (via sc.external.pp.scrublet), adding doublet scores and classifications to the cell metadata.
* **Normalization & Log-Transformation**: Normalize total counts per cell (target sum = 10,000) and log-transform the data.
* **Cell Cycle Scoring:** Use predefined cell cycle gene lists to score cells and help control for cell cycle effects in downstream analysis.
* **Saving and Reloading Processed Data:** Preprocessed AnnData objects are saved to disk (in H5AD format) so you can reload them later without re-running the full pipeline.
* **Downstream Analysis for Each Condition:** For each condition, the pipeline performs:
	* Highly Variable Genes (HVG) selection
	* Regression of confounding effects (e.g., mitochondrial percentage and total counts)
	* Scaling and Principal Component Analysis (PCA)
	* Computation of the neighborhood graph, Leiden clustering, and UMAP visualization
	* Differential expression analysis between clusters

* **Merging and Integration**: After processing each dataset individually, control datasets (Saline) are concatenated. In a separate integration step, the pipeline merges the Saline and IAV datasets and applies Scanorama integration to generate a joint latent representation for downstream analysis (neighbors, UMAP, etc.).

_Requirements
	•	Python 3.x
	•	Scanpy
	•	anndata
	•	scvi-tools
	•	Scrublet (via sc.external.pp.scrublet)
	•	Other dependencies: numpy, pandas, matplotlib, igraph, leidenalg_
