---
tags:
    - alignment
    - pseudoalignment
    - kallisto
---

# Performing pseudoalignment with kallisto

---

Pseudoalignment is a rapid, k-mer-based computational method for RNA-seq and metagenomic data analysis that identifies which reference transcripts a read is compatible with, rather than finding its exact, base-by-base alignment location. 
It uses a de Bruijn graph of the transcriptome to map reads quickly, providing sufficient accuracy for quantifying gene expression while being much faster than traditional alignment. 

### Key aspects of pseudoalignment include:
- Speed and Efficiency: It skips the time-consuming process of mapping base-to-base alignments, often allowing millions of reads to be processed in minutes on standard computers
- Mechanism: It breaks reads into k-mers and maps them against a transcriptome de Bruijn graph (T-DBG) to find the set of transcripts that match the read
- Tools: Primary tools utilizing this method include Kallisto, Salmon, and Sailfish
- Applications: It is primarily used for RNA-seq transcript abundance quantification and metagenomic read assignment
- Limitations: Because it does not provide exact positional information (e.g., specific splice sites or exact base mismatches), it is not suitable for applications requiring high-precision mapping, such as de novo assembly or detecting single nucleotide polymorphisms (SNPs)
- Pseudoalignment is designed to handle non-uniquely mapped reads by identifying the potential transcripts of origin, which can then be further refined using algorithms like Expectation-Maximization


### Standard pseudoalignment code
The code snippet below will allow you to quickly align paired-end RNASeq data. Note that input fastq files should not be preprocessed (i.e. no adapter or quality trimming)
```
REF=/project/sysviro/data/reference_transcriptomes/kallisto/gencode.v47.transcripts

/project/sysviro/tools/kallisto/kallisto quant -i $REF -o $OUT -b 100 --bias --rf-stranded $IN_1.fq.gz $IN_2.fq.gz
```
-b 100  :  bootstrapping is a computational technique used to estimate the technical variance and uncertainty in transcript abundance quantification <br>
--bias  :  enables a model that learns the empirical fragment length distribution and fragment start position distribution from the data to improve estimation <br>
--fr-stranded : Read 1 aligns to the for strand of the transcript, Read 2 aligns to the rev strand (i.e. first read = sense). Typical for TruSeq protocols <br>
--rf-stranded : Read 1 aligns to the rev strand of the transcript, Read 2 aligns to the for strand (i.e. first read = antisense). Typical for dUTP based methods (DNBSEQ, NEB Ultra II Directional) <br>
<b>Note: It is absolutely critical to pay attention to which stranded flag is being used!!! </b>


### Choice of reference transcriptome
The optimal choice of (human) reference transcriptome is Gencode. We currently use the v47 assembly but later versions are now [available](https://www.gencodegenes.org/human/). The Comprehensive gene annotation for the CHR regions is sufficient and the GTF file can be downloaded using wget i.e. by right-clicking on the GTF link and selecting 'copy link' and pasting this after wget e.g. 
```
wget https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_47/gencode.v47.annotation.gtf.gz
```

### Converting transcript counts into gene counts
Given that most downstream applications involve differential gene expression analysis using DeSeq2 or edgeR, it is usually desirable to convert the reported transcript-level counts produced by kallisto into gene-level counts. This is especially useful for pathway analyses as most programs do not accept transcript IDs. The R snippet below should be sufficient to merge the kallisto outputs from all subfolders (samples) within a folder and to produce a gene-level counts file that is ready for input into e.g. DeSeq2.

```
library(tximport)
library(rhdf5)
library(rtracklayer)

### SET WORKING DIRECTORY ### You will need to edit this and direct it your downloaded kallisto folder
setwd("C:/Users/depledgd/Dropbox/RNASeq/kallisto")

### IMPORT ANNOTATION ### Import gencode v47 annotation (the same GTF file used for the pseudoalignment)
library(rtracklayer)
gtf <- import("../gencode.v47.annotation.gtf") 
gtf <- gtf[gtf$type == "transcript"]
t2g <- unique(data.frame(tx = gtf$transcript_id, gene = gtf$gene_id))

### USE TXIMPORT TO SUMMARIZE TRANSCRIPT COUNTS INTO GENE COUNTS
## For multiple samples, each named as a folder in the kallisto directory (can be abundance.h5 or abundance.tsv file). 
accessions <- list.dirs(full.names=FALSE)[-1]
kallisto.dir<-paste0(accessions)
kallisto.files<-file.path(kallisto.dir,"abundance.tsv")
names(kallisto.files)<- accessions
tx.kallisto <- tximport(kallisto.files, type = "kallisto", tx2gene = t2g, countsFromAbundance ="no", ignoreAfterBar=TRUE)

### GENERATE TWO COLUMN OUTPUT FORMAT
counts<-as.data.frame(tx.kallisto$counts)

### ROUND VALUES (DESEQ2 DOES NOT LIKE FRACTIONS), AND WRITE TO OUTPUT FILE
write.table(round(counts), "allcounts.txt", row.names = TRUE, quote = FALSE, sep = "\t")
```
The output should be a text file containing all gene names in the first column with respective counts for each sample shown in the additional columns
















