# Bioinformatics Workflows for Epigenomic Sequencing Analysis

A collection of bioinformatics workflows developed for processing and analyzing **next-generation sequencing (NGS)** and **epigenomic data**, including ChIP-seq, MeDIP-seq, and Reduced Representation Bisulfite Sequencing (RRBS).

The repository documents end-to-end analysis procedures spanning raw sequencing data processing, genome alignment, quality control, methylation analysis, peak detection, reproducibility assessment, genomic annotation, and statistical evaluation.

## Overview

The workflows cover four major analysis areas:

| Workflow         | Purpose                                                                                          |
| ---------------- | ------------------------------------------------------------------------------------------------ |
| **ChIP-seq**     | Read processing, alignment, QC, peak calling, signal normalization, and reproducibility analysis |
| **MeDIP-seq**    | DNA methylation enrichment, coverage analysis, CpG analysis, and regional comparisons            |
| **RRBS**         | Bisulfite alignment, CpG methylation calling, and promoter-level methylation analysis            |
| **ROC Analysis** | Benchmarking genomic peak calls using ROC curves and AUC                                         |

These scripts and command references were developed for Linux/HPC environments and combine command-line bioinformatics tools with R-based statistical analysis.

---

## ChIP-seq Analysis

`Bioinformatics-ChIP` contains workflows for processing and analyzing chromatin immunoprecipitation sequencing data.

### Workflow

```text
Raw sequencing reads
        │
        ▼
Quality trimming
        │
        ▼
Genome alignment
        │
        ▼
SAM/BAM processing
        │
        ▼
Read filtering & duplicate handling
        │
        ▼
Quality control
        │
        ▼
Peak calling
        │
        ▼
Reproducibility / IDR analysis
        │
        ▼
Signal normalization & visualization
```

### Major analysis steps

**Read processing**

* FASTQ/SRA conversion
* Sequence quality trimming
* Single-end and paired-end sequencing support

**Genome alignment**

Alignment workflows include:

* Bowtie
* Bowtie2
* BWA

Human genome references used in the original analyses include **hg19** and **GRCh38**.

**Alignment processing**

SAMtools and BEDTools are used for:

* SAM-to-BAM conversion
* Sorting and indexing
* Mapping-quality filtering
* Removal of low-quality or unmapped reads
* Duplicate handling
* Genome coverage analysis
* tagAlign generation

**ChIP-seq quality control**

The workflow includes:

* Alignment statistics
* Library complexity analysis
* PCR Bottleneck Coefficient (PBC)
* Cross-correlation analysis
* Fragment-length estimation
* Replicate correlation

**Peak calling**

Peak detection workflows include:

* MACS / MACS2
* SPP

**Reproducibility analysis**

Replicate consistency is evaluated using **Irreproducible Discovery Rate (IDR)** analysis.

The workflow includes procedures for:

* Comparing biological replicates
* Filtering peaks by IDR thresholds
* Removing genomic blacklist regions
* Generating conservative peak sets

**Signal normalization**

Genome-wide signal tracks can be generated and compared using tools such as:

* `bamCoverage`
* `bamCompare`
* `bigwigCompare`
* `multiBigwigSummary`

Normalized BigWig tracks support downstream visualization and genome-wide comparison.

---

## MeDIP-seq Analysis

`Bioinformatics-MeDIP` contains workflows for analyzing **Methylated DNA Immunoprecipitation sequencing (MeDIP-seq)** data.

Much of the analysis is implemented using the R/Bioconductor package **MEDIPS**.

### Analysis workflow

```text
Aligned sequencing reads
        │
        ▼
MEDIPS data objects
        │
        ├── Saturation analysis
        ├── CpG coverage
        ├── CpG enrichment
        ├── Calibration analysis
        └── Sample correlation
        │
        ▼
Region-specific methylation analysis
```

### Quality-control analyses

The workflow evaluates sequencing and enrichment quality using:

* Saturation analysis
* Sequence coverage
* CpG enrichment
* Calibration plots
* Pearson correlation between samples

### Genomic regions

Methylation signal can be evaluated across different genomic contexts, including:

* Genome-wide regions
* Promoters
* CpG islands
* Model-based CpG islands

BEDTools and SAMtools are used to extract reads overlapping genomic regions before downstream analysis.

### Comparative analysis

The workflow supports comparisons between samples generated using different DNA input amounts and biological replicates.

This allows evaluation of:

* Signal reproducibility
* CpG enrichment
* Regional coverage
* Sample-to-sample correlation
* Sequencing depth effects

---

## RRBS Analysis

`Bioinformatics-RRBS` contains a workflow for analyzing **Reduced Representation Bisulfite Sequencing** data.

### Workflow

```text
FASTQ
  │
  ▼
Bismark alignment
  │
  ▼
Methylation extraction
  │
  ▼
CpG-level methylation calls
  │
  ▼
methylKit
  │
  ▼
Promoter-level methylation analysis
```

### Bisulfite sequencing analysis

**Bismark** is used to:

* Align bisulfite-converted sequencing reads
* Extract methylated and unmethylated read counts
* Generate BedGraph files
* Generate coverage files
* Produce CpG reports

### Methylation analysis

The R/Bioconductor package **methylKit** is used for downstream methylation analysis.

The workflow converts CpG calls into data containing:

* Chromosome
* Genomic position
* DNA strand
* Methylation ratio
* Sequencing coverage

### Promoter methylation

Promoter regions are generated from RefSeq transcription start sites and intersected with methylation calls.

This enables summarization of methylation levels across promoter regions for downstream biological analysis.

---

## ROC and AUC Analysis

`Bioinformatics-ROC` contains a workflow for evaluating genomic prediction or peak-calling results against reference datasets.

### Genomic feature preparation

RefSeq gene annotations are converted into BED format and used to define promoter regions around transcription start sites.

BEDTools is then used to identify:

* Peaks overlapping reference regions
* Positive observations
* Negative observations

### ROC analysis

The resulting scores and binary labels are analyzed in R using **ROCR**.

The workflow calculates and visualizes:

* True-positive rate
* False-positive rate
* ROC curves
* Area Under the Curve (AUC)
* Performance as a function of prediction cutoff

This provides a quantitative framework for comparing genomic signal-detection approaches.

---

## Tools and Technologies

### Sequence processing and alignment

* SRA Toolkit
* Sickle
* Bowtie
* Bowtie2
* BWA
* Bismark

### Genomic data processing

* SAMtools
* BEDTools
* UCSC utilities
* IGVTools

### ChIP-seq

* MACS / MACS2
* SPP
* IDR
* ENCODE/AQUAS ChIP-seq pipeline

### DNA methylation

* MEDIPS
* methylKit
* Bismark methylation extractor
* BSgenome

### Statistical analysis

* R
* ROCR
* verification

### Signal processing and visualization

* deepTools
* BigWig / BedGraph utilities
* Genome coverage tools

### Scripting

* Bash
* AWK
* sed
* R

---

## Repository Structure

```text
Bioinformatics/
│
├── Bioinformatics-ChIP
│   ChIP-seq preprocessing, alignment, QC,
│   peak calling, IDR, and signal analysis
│
├── Bioinformatics-MeDIP
│   MeDIP-seq QC, CpG enrichment,
│   regional analysis, and MEDIPS workflows
│
├── Bioinformatics-RRBS
│   Bisulfite alignment, methylation calling,
│   and promoter methylation analysis
│
├── Bioinformatics-ROC
│   Genomic intersections, ROC curves,
│   and AUC evaluation
│
└── README.md
```

---

## Example ChIP-seq Pipeline

A representative workflow is:

```text
FASTQ
  │
  ├── Quality filtering
  │
  ▼
BWA / Bowtie2
  │
  ▼
SAMtools
  │
  ├── Sort
  ├── Filter
  ├── Mark/remove duplicates
  └── Index
  │
  ▼
BEDTools
  │
  ▼
SPP / MACS2
  │
  ▼
IDR
  │
  ▼
High-confidence peaks
  │
  ▼
BigWig signal tracks
```

The workflow combines sequence processing, quality control, statistical peak detection, replicate reproducibility analysis, and genome-browser-ready visualization tracks.

---

## Skills Demonstrated

This repository demonstrates experience with:

* Next-generation sequencing analysis
* ChIP-seq
* DNA methylation analysis
* MeDIP-seq
* RRBS
* Bisulfite sequencing
* Genome alignment
* SAM/BAM/BED genomic file processing
* Peak calling
* Biological replicate QC
* IDR analysis
* Genomic annotation and interval operations
* CpG and promoter analysis
* ROC/AUC statistical evaluation
* R/Bioconductor
* Linux command-line bioinformatics
* Bash/AWK data processing
* High-performance computing workflows

---

## Reproducibility Note

This repository contains research workflow notes and command references rather than a packaged, turnkey pipeline.

The original commands were developed in an HPC environment and contain environment-specific paths, reference-genome locations, module versions, and filenames. Running the workflows on another system therefore requires adapting these paths and installing the corresponding bioinformatics tools.

Because bioinformatics software and command-line interfaces evolve over time, some commands may also require modification for current versions of the tools.

The repository is preserved as a record of the analysis methodology and computational workflows used in these projects.
