---
tags:
    - alignment
    - pseudoalignment
    - kallisto
---

# Performing pseudoalignment with kallisto

---

Pseudoalignment is a rapid, k-mer-based computational method for RNA-seq and metagenomic data analysis that identifies which reference transcripts a read is compatible with, rather than finding its exact, base-by-base alignment location. It breaks reads into k-mers and maps them against a transcriptome de Bruijn graph to quickly find the set of transcripts that match the read. This provides sufficient accuracy for quantifying gene expression while being much faster than traditional alignment. 

### Key aspects of pseudoalignment include:
- Speed/Efficiency: It skips the time-consuming process of generating base-to-base alignments, often allowing millions of reads to be processed in minutes on standard computers
- Method: Pseudoalignment is designed to handle non-uniquely mapped reads by identifying the potential transcripts of origin, which can then be further refined using algorithms like Expectation-Maximization. 
- Tools: Primary tools utilizing this method include [Kallisto](https://pachterlab.github.io/kallisto/manual.html), [Salmon](https://salmon.readthedocs.io/en/latest/salmon.html), and [Sailfish](https://www.cs.cmu.edu/~ckingsf/software/sailfish/index.html). See [here](https://tinyheero.github.io/2015/09/02/pseudoalignments-kallisto.html) for a detailed description of how pseudoalignment works
- Applications: It is primarily used for RNA-seq transcript abundance quantification and is only suitable for use with high-resolution transcriptome annotations (e.g. Gencode Human/Mouse etc)
- Limitations: Because it does not provide exact positional information (e.g., specific splice sites or exact base mismatches), it is not suitable for applications requiring high-precision mapping

### Standard pseudoalignment code
The code snippet below will allow you to quickly align paired-end RNASeq data. Note that input fastq files should not be preprocessed (i.e. no adapter or quality trimming). <i> See script_templates folder in sysviro for a SLURM compatible example. </i>
```
# define variables
FASTA=/project/sysviro/data/reference_transcriptomes/kallisto/gencode.v47.transcripts.fa
REF=/project/sysviro/data/reference_transcriptomes/kallisto/gencode.v47.transcripts

# index reference transcriptome (note: this only needs to be performed once)
/project/sysviro/bin/kallisto index $REF

/project/sysviro/bin/kallisto quant -i $REF -o $OUT -b 100 --bias --rf-stranded $IN_1.fq.gz $IN_2.fq.gz
```

-b 100  :  bootstrapping is a computational technique used to estimate the technical variance and uncertainty in transcript abundance quantification <br>
--bias  :  enables a model that learns the empirical fragment length distribution and fragment start position distribution from the data to improve estimation <br>
--fr-stranded : Read 1 aligns to the for strand of the transcript, Read 2 aligns to the rev strand (i.e. first read = sense). Typical for ligation-based protocols <br>
--rf-stranded : Read 1 aligns to the rev strand of the transcript, Read 2 aligns to the for strand (i.e. first read = antisense). Typical for dUTP based methods (DNBSEQ, NEB Ultra II Directional) <br>
<b> Note: It is absolutely critical to pay attention to which stranded flag is being used!!! For unstranded data, these flags should be omitted. If you are at all unsure about whether your data is stranded or not then read the stranded section and run RSeQC </b>


### Choice of reference transcriptome
The optimal choice of (human) reference transcriptome is Gencode. We currently use the v47 assembly but later versions are now [available](https://www.gencodegenes.org/human/). The Comprehensive gene annotation for the CHR regions is sufficient and the GTF file can be downloaded using wget i.e. by right-clicking on the GTF link and selecting 'copy link' and pasting this after wget e.g. 
```
wget https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_47/gencode.v47.annotation.gtf.gz
wget https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_47/gencode.v47.transcripts.fa.gz

gunzip gencode.v47.annotation.gtf.gz
gunzip gencode.v47.transcripts.fa.gz
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
















