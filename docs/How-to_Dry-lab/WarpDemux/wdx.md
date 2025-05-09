---
tags:
    - WarpDemux
    - WDX
    - DRS
    - Nanopore
---

# Multiplexed Nanopore direct RNA Sequencing with WarpDemux

---

## Overview

[WarpDemuX](https://github.com/KleistLab/WarpDemuX) is an ultra-fast and high-accuracy adapter-barcoding and demultiplexing tool for Nanopore direct RNA sequencing. It currently provides support for RNA004 chemistry, specifically poly(A) DRS and nano-tRNASeq.

## Installation
Running WarpDemux (WDX) requires setting up a conda environment and installing required packages. Below provides a general overview on how to do this but please check the [WarpDemux Github](https://github.com/KleistLab/WarpDemuX) before doing so in case changes are made to the procedure.

```
# Switch to an interactive node, you will likely need >16Gb of memory for the installation
srun --partition=leinecpu_interactive --job-name="wdx" --cpus-per-task=1 --mem-per-cpu=32G -t8:00:0 --pty /bin/bash

# Navigate to 
cd /project/sysviro/tools

# Grab latest version using Git (optional, usually the version available in /project/sysviro/tools/WarpDemux is the one to use)
module load git
git clone --recursive https://github.com/KleistLab/WarpDemuX.git

# Set up a new conda environment (this is installed into your home directory)
module load Anaconda3/2023.03-1
module load GCC

conda env create -n WDX -f WarpDemuX/environment.yml

conda activate WDX

pip install -e WarpDemuX/

conda deactivate WDX
```

<br><br>

## Running WDX
While WDX comprises two stages (preparation, prediction), typical usage is to run these together via the demux command

```
module load Anaconda3/2023.03-1

conda activate WDX

warpdemux demux -i /project/sysviro/data/Nanopore-Pod5/P2_WDX-6/Multiplexed2_STM2457_treatment_HEVandVZV/20250415_1926_P2S-01949-A_PBC47053_0d982d23/pod5/ -o /project/sysviro/data/Nanopore-Pod5/P2_WDX-6/Multiplexed2_STM2457_treatment_HEVandVZV/20250415_1926_P2S-01949-A_PBC47053_0d982d23/ -m WDX4_rna004_v0_4_4 -j 20 --batch_size_output 40000 --save_boundaries true

conda deactivate WDX
```

Of critical importance is selecting the correct model (-m). The [WarpDemux Github](https://github.com/KleistLab/WarpDemuX?tab=readme-ov-file#models) has a list of all available models and the barcodes and chemistries associated with each. So, if you are demultiplexing a run using RNA004 chemistry and the WDX barcodes 03, 05, and 07, you would choose model WDX4_rna004_v0_4_4. 

<b>!!!Note that current RNA004 models are limited to a maximum of four barcodes in a run. If you multiplex to greater depths than this then we will need to generate a new model with the help of the original developers!!!</b>

<br>

## WDX output & parsing
Following the demux step, an output folder will be generated in the output directory. The folder name will reflect the model used and the date of the demux run. The output folder will contain a  subfolder called predictions which contains large number of .csv files, each file containing predictions for 40000 reads. These should be combined into a single file
```
cat barcode_predictions_* > barcode_predictions.all.csv
```





- latest gencode predictions (obtained via UCSC Table browser) in genepred format
- reduced modkit BED files containing modification calls against genome alignments
- assorted python and R scripts (AGDepledge/scripts/)

While more complicated than analyses of transcriptome-level alignments, genome-level alignments may be preferred in many cases. The following described how to generate genome-level metaplots when using standard DRS approaches. This approach can be applied to all modifications that can be natively called by Dorado.

#### 1. Navigate to [UCSC Table browser](https://genome.ucsc.edu/cgi-bin/hgTables)
- set assembly to Dec. 2013 HG38
- set track to All Gencode V47 (or later version)
- set table to Basic(wgEncodeGencodeBasicV47)
- set output file name to hg38_gencode_v47.genePred
- download

