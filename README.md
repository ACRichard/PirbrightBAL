# PirbrightBAL-muir

This repository contains analysis markdown files and scripts for analysing porcine BAL scRNAseq data as a collaborative work between the Babraham institute and Pirbright Institute. Results are now published in PLOS Pathogens, titled; 'Single-cell analysis reveals lasting immunological consequences of influenza infection and respiratory immunisation in the pig lung'.

Original unprocessed data can be downloaded from GEO(GSE249866).
https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE249866


To replicate the analysis performed in the published paper, download the fastq.gz files from GEO(GSE249866) and perform alignment/counting using `cellranger count` (cellranger-7.0.0) using default parameters and Sus scrofa genome (genome assembly 11.1, Ensembl release 107) to regenerate files used in the original analysis. Example cellranger commands can be found in '/cluster scripts'. 

Directory should look like this for each sample folder:

- sample1 `folder`
  - molecule_info.h5
  - raw_feature_bc_matrix.h5 `also available on GEO`
  - filtered_feature_bc_matrix.h5 `also available on GEO`
  - raw_feature_bc_matrix `folder`
    - barcodes.tsv.gz
    - features.tsv.gz
    - matrix.mtx.gz
  - filtered_feature_bc_matrix `folder`
    - barcodes.tsv.gz
    - features.tsv.gz
    - matrix.mtx.gz

Scripts in this directory are run in the following order:

1. **processing_all.Rmd** 
*Reads in cellranger output (see above) and performs initial filtering and QC.*
2. **clustering_all.Rmd** 
*Clustering and cluster inspection.*
3. **differential_all.Rmd** 
*Cell type annotation followed by an initial differential expression analysis utilising a pseudobulk approach.*

The following scripts can be run in any order, provided the scripts above have been run. 

- **Validation_vs_PBMC.Rmd** 
*Comparative analysis between the clusters and cell types identified in the pig BAL against the reference PBMC transcriptome published by Herrera-Uribe et al. 2021 (https://doi.org/10.3389%2Ffgene.2021.689406).*
- **diff-co_NEBULA.Rmd** 
*Differential expression and co-expression analysis with NEBULA as well as topGO enrichment of the identified genes.*
- **Treg_pseudobulk_volcanoes.Rmd** 
*Short script to generate volcano plots of differentially expressed genes from Tregs derived from the pseudobulk analysis in differential_all.Rmd. This script was used for responding  to reviewers comments and doesn't generate data for the related paper.*
- **diff-co.Rmd**
*Initial look at differential co-expression using dcanr. Not utilised in paper.*

`shiny_app/data.rds` to use for data exploration in the shiny app `shiny_app/app.R` is generated by **clustering all.Rmd** (unannotated data) and **differential_all.Rmd** (annotated and normalised data). 
