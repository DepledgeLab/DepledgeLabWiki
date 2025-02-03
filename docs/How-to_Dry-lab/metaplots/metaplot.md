---
tags:
    - metaplot
---

# Metaplot

## Overview

Metaplots are density plots / histograms showing the clustering of sites of interest (e.g. RNA modifications) along simplified transcript models. They can be generated from datasets aligned against either the genome
or transcriptome of a well annotated organism, typically the human genome (HG38). Below you will find information on how to generate different types of metaplot.


## Analysis of m6A and pseU on HG38 mRNAs using R
input files:
- modkit BED file containing modification calls against transcriptome alignments (generated using modkit)
- hg38-output.txt data file which described the UTR and CDS regions of all currently annotated HG38 Ensembl transcripts (obtain from Dan or generate yourself (see below)

The R script, txome-metaplot.R (AGDepledge/scripts/), can be used to process such an input file and produce an analysis of m6A and pseU density across sequenced RNAs. The output highlight distinct regions such as 
the 5' UTR (0-1), CDS (1-2), and 3' UTR (2-3).

![metaplot-txome](../../img/txome-metaplot.jpg)




## Analysis of m6A and pseU on Pol III-derived RNAs using R
input files:
- modkit BED files containing modification calls, generated from running bedtools intersect with Pol III tx and tRNAs with genome-level alignments (generated using modkit)
- pol-III-ome-metaplot.R (AGDepledge/scripts/)

In this example we seperate tRNAs and other Pol III RNAs for the metaplot analysis. Note that as these are non-coding RNAs, we do not include any region information (e.g. there is no CDS region). You can adjust the R script to accept as many input files as you like but you will need to make some manual updated to specify which should be plotted.

![metaplot-pol3ome](../../img/pol3ome-metaplot.jpg)


