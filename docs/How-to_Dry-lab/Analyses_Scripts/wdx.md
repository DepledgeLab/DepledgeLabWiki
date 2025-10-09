---
tags:
    - WarpDemuX
    - DRS
    - Nanopore
    - Bash
---

# Multiplexed Nanopore direct RNA Sequencing with WarpDemux

---

## Overview

[WarpDemuX (WDX)](https://github.com/KleistLab/WarpDemuX) is an ultra-fast and high-accuracy adapter-barcoding and demultiplexing tool for Nanopore direct RNA sequencing. It currently provides support for RNA004 chemistry, specifically poly(A) DRS and nano-tRNASeq.

## Installation
Running WarpDemuX requires setting up a conda environment and installing required packages. Below provides a general overview on how to do this but please check the [WarpDemuX Github](https://github.com/KleistLab/WarpDemuX) before doing so in case changes are made to the procedure.

```
# Switch to an interactive node, you will likely need >16Gb of memory for the installation
srun --partition=leinecpu_interactive --job-name="wdx" --cpus-per-task=1 --mem-per-cpu=32G -t8:00:0 --pty /bin/bash

# Navigate to 
cd /project/sysviro/tools

# Grab latest version using Git (optional, usually the version available in /project/sysviro/tools/WarpDemuX is the one to use)
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
The barcode_predictions.all.csv contains a list of all read ids (column 1), the predicted barcode (col 2), the confidence score (col 3), and the individual confidence scores for each barcode (cols 4+).
```
read_id,predicted_barcode,confidence_score,p03,p04,p05,p07,p-1
cd3c0eab-1e24-4059-86e5-0f5845d417d4,5,0.893,0.045,0.0047,0.9384,0.0107,0.0012
4cc652e6-2369-452d-ad5d-668ed5b63198,5,0.964,0.0048,0.0006,0.9786,0.0141,0.0019
58346cc3-6624-4b59-97cb-0ab91c361ad9,5,0.976,0.0077,0.0037,0.9838,0.0036,0.0011
021ff088-6183-4035-9d84-8d5c7c52cfe0,4,0.994,0.002,0.9961,0.0005,0.0008,0.0006
d1a12590-4408-4ce4-a740-0696bf6f5651,4,0.991,0.0033,0.9944,0.0006,0.0011,0.0006
87ce2169-e6c0-4d5b-a7b5-c39df46264a8,5,0.972,0.0046,0.0015,0.982,0.0098,0.0021
09dde30c-5aa6-452c-a0ea-178efe165dba,4,0.966,0.0112,0.9767,0.002,0.0078,0.0024
79bf882c-f6f1-405a-9672-60d76691f72f,5,1.0,0.0,0.0,0.9998,0.0001,0.0001
33bc3975-8fdd-4583-abad-0094bafba834,4,0.997,0.0009,0.998,0.0004,0.0004,0.0004
```
We process this using the following commands to select only high confidence barcodes to produce a list of read ids associated with each demultiplexed sample. In the below example we are compiling lists of read ids associated with barcode 04 and barcode 05 and with probability scores of >90%
```
awk -F',' '$2 == 4 && $3 >= 0.9 {print $1}' predictions_filtered.bam.csv > barcode04.p0.9.list.txt
awk -F',' '$2 == 5 && $3 >= 0.9 {print $1}' predictions_filtered.bam.csv > barcode05.p0.9.list.txt
```

## Filtering of Dorado-basecalled uBAMs
Finally, we can use BBMap to create individual (demultiplexed) uBAMs from a dorado-basecalled uBAM that is generated using the full datasets (multiplexed).
```
module load BBMap
module load SAMtools
filterbyname.sh in=../../EMC1_wt_U1mut_ARPE19_MPlex-2.hac.trimAdapters.dorado.0.8.0.bam out=EMC1_wt_ARPE19-wdx4.hac.trimAdapters.dorado.0.8.0.bam names=EMC1_wt_U1mut_ARPE19_MPlex-2.WDX12_rna004-v0.4.4/barcode04.p0.9.list.txt include=true overwrite=true usejni=t -Xmx16g
```

From here, we can proceed to generating FASTQ files for downstream alignments etc.
