
### Strandedness in (short-read) RNA-Seq experiments
Strandedness in RNA-seq describes whether the sequencing protocol preserves information about which DNA strand the original RNA transcript came from. In stranded (directional) libraries, read orientation can be used to distinguish sense from antisense transcription, improving quantification accuracy for overlapping genes and isoforms. In unstranded libraries, this information is lost, and reads are compatible with transcripts on either strand. Correctly specifying strandedness during analysis is essential, as using the wrong setting can bias expression estimates, particularly for antisense-overlapping genes and at the transcript level.

### A Warning!!!
While strandedness is broadly classified as forward-stranded or reverse-stranded, the many different tools available have played fast and loose with these terms which can cause significant confusion i.e. TopHat. Below is a table highlighting the strandedness parameters for tools commonly used in the lab. For a comprehensive list, take a look [here](https://rnabio.org/module-09-appendix/0009/12/01/StrandSettings/)

| Tool | RF (reverse read = strand) |	FR (forward read = strand)  |	Unstranded
|------|-----------------------------------|-----------------------------------------|------------
|IGV (5p to 3p read orientation) |	F2R1 |	F1R2 |	F2R1 or F1R2
|TopHat (--library-type parameter) |	fr-firststrand |	fr-secondstrand |	fr-unstranded
|HISAT2 (--rna-strandness parameter) |	R/RF |	F/FR |	NONE
|STAR |	n/a (STAR doesn’t use library strandedness info for mapping) |	NONE |	NONE
|Picard CollectRnaSeqMetrics (STRAND_SPECIFICITY parameter) |	SECOND_READ_TRANSCRIPTION_STRAND |	FIRST_READ_TRANSCRIPTION_STRAND |	NONE
|Kallisto quant (parameter) |	--rf-stranded |	--fr-stranded |	NONE
|FeatureCounts (-s parameter) |	2 |	1 |	0
|Example methods/kits: | dUTP, NSR, NNSR, Illumina TruSeq Strand Specific Total RNA, NEBNext Ultra II Directional, Watchmaker RNA Library Prep Kit with Polaris Depletion	| Ligation, Standard SOLiD, NuGEN Encore, 10X 5’ scRNA data | Standard Illumina, NuGEN OvationV2, SMARTer universal low input RNA kit (TaKara), GDC normalized TCGA data

### Easy ways to infer strandedness
[RSeQC](https://rseqc.sourceforge.net/) (An RNA-seq Quality Control Package) provides a number of useful modules that can comprehensively evaluate high throughput sequence data especially RNA-seq data. Some basic modules quickly inspect sequence quality, nucleotide composition bias, PCR bias and GC bias, while RNA-seq specific modules evaluate sequencing saturation, mapped reads distribution, coverage uniformity, strand specificity, transcript level RNA integrity etc.

### Setting up and running RSeQC to infer strandedness
RSeQC includes a python script for determining strandedness. To do this, it is easiest to subsample 10,000-20,000 paired-reads from the fastq files of one of your samples. These should be aligned with STAR or HiSat2 to the target genome (not transcriptome) and parsed to a BAM file. A corresponding BED12 file containing the gene annotations should also be created or downloaded. Together, these can be fed to the infer_experiment script. 

```
# install using pip
pip3 install RSeQC

infer_experiment.py -i aligned.bam -r genes.bed
```

### Interpreting the output
A typical output would look something like 
```
This is PairEnd Data
Fraction of reads failed to determine: 0.02

Fraction of reads explained by:
1++,1--,2+-,2-+ : 0.91
1+-,1-+,2++,2-- : 0.07
```
For paired-end data: <br>
1++,1--,2+-,2-+ dominant → reverse-stranded → kallisto --rf-stranded <br>
1+-,1-+,2++,2-- dominant → forward-stranded → kallisto --fr-stranded <br>
If neither wins decisively (i.e. if  both patterns are ~0.5), strandedness was not preserved <br>



