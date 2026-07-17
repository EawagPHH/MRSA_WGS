# Genomic insights into methicillin-resistant *Staphylococcus aureus* from Swiss wastewater and clinical samples

This repository contains the analysis code and the wastewater/genomic data underlying the manuscript "Genomic insights into methicillin-resistant *Staphylococcus aureus* from Swiss wastewater and clinical samples" (Kamer, Conforti, et al.).

## Repository Overview

The repository includes:

- The R Markdown analysis script (`wgs_mrsa.Rmd`) used to generate every table and figure in the manuscript and its supplementary material, from sequencing QC through genomic typing, antimicrobial resistance gene content, virulence factor content, and SNP-based relatedness analysis.
- `gunc.sh`, a Slurm submission script used to run Genome UNClutterer (GUNC) v1.0.6 on a subset of assembled genomes to detect inter-species contamination in assembled contigs; only contigs taxonomically classified as *S. aureus* were retained, decontamination was evaluated with QUAST v5.2.0, and decontaminated genomes were re-annotated with Bakta before being carried forward into `wgs_mrsa.Rmd`. Cluster-specific paths have been replaced with placeholders (`/path/to/your/scratch/...`) — edit these before running on your own system.
- The `data/` folder, containing the input files the script reads directly:
  - `sequencing.csv`, `sample_annotations.csv` — per-sample sequencing/QC log and isolate metadata (source, location, collection date, ST/SCCmec/spa calls).
  - `args_abricate.txt`, `gene_and_class.csv` — Abricate antimicrobial resistance gene (ARG) screening results and the ARG-to-antibiotic-class lookup table.
  - `vfs_abricate.txt` — Abricate virulence factor gene screening results.
  - `distances_no_recomb.tab` — pairwise, recombination-masked core-genome SNP distance matrix.
  - `230214_ARA_BAG.shp` (+ `.dbf`/`.prj`/`.shx`) — wastewater treatment plant (WWTP) catchment shapefile.
  - `swissBOUNDARIES3D_1_4_TLM_KANTONSGEBIET.shp` (+ `.cpg`/`.dbf`/`.prj`/`.shx`) — Swiss canton boundary shapefile, used as the base map.
- A pre-defined folder structure, so the script runs without modification as long as the directory layout is preserved (the script reads everything via relative `data/...` paths).

The upstream bioinformatic pipeline that produced the sequencing outputs used here — read trimming and QC, contamination screening, genome assembly, annotation, pangenome/phylogeny construction, and ST/SCCmec/spa typing — was implemented as a Snakemake workflow and is maintained in a separate repository: https://github.com/sheenaconforti/ecoli-wgs. `gunc.sh` documents one additional decontamination step run alongside that pipeline.

## How to Use This Repository

- Download or clone the entire repository.
- Maintain the provided folder structure (the `data/` directory already contains the correct paths expected by the script).
- Open `wgs_mrsa.Rmd` in RStudio (or render with `rmarkdown::render()`) to reproduce the tables and figures reported in the manuscript and supplementary material.
- `gunc.sh` is a standalone Slurm cluster script (not run by `wgs_mrsa.Rmd`) — it documents the decontamination step applied upstream, before the genomes were assembled/typed. It is not needed to reproduce the tables and figures from the data already provided in `data/`.

If you have any questions or require access to additional data, please contact the corresponding authors.
