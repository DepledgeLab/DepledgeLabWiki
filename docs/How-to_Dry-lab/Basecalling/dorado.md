---
tags:
    - Basecalling
    - Dorado
    - DRS
    - Nanopore
---

# Basecalling of Nanopore Datasets with Dorado

---

## Overview

Basecalling is a key part of nanopore data processing. While 'live' basecalling is usually performed during nanopore runs, this is usually run in a basic (fast or high-accuracy) mode. Given that we are usually 
interested in RNA modifications and poly(A) tails, re-basecalling in super-accuracy mode is often desired. The following is a guide to exactly that

## Dorado versions

The Dorado basecaller is periodically updated by nanopore to improve basecalling accuracy and to add new models for poly(A) tail estimation and detection of DNA/RNA modifications. You can always look for the latest version
[here](https://github.com/nanoporetech/dorado). When a new version of Dorado is released, Dan will (usually) download and make it available in the base ```/project/sysviro/``` space. If the latest version is not there then 
please inform Dan immediately so he can download it.

## Basecalling with Dorado
Invariable you will want to perform Dorado basecalling in super-accuracy mode (sup) with RNA modification detection and poly(A) tail detection enabled. This requires significant time (12-24 hours) and GPU power. For applications
in which RNA mods and/or poly(A) tails are not of interest, high-accuracy mode (hac) is sufficient.

Dorado can be launched as follows:
```
/project/sysviro/dorado-0.9.0-linux-x64/bin/dorado
```

While there are multiple sub-arguments that can be specified, all of which are detailed [here](https://github.com/nanoporetech/dorado). However, the most relevant command for basecalling is 'basecaller' e.g. 
```
/project/sysviro/dorado-0.9.0-linux-x64/bin/dorado basecaller
```

### High-accuracy (vanilla) basecalling
For vanilla basecalling, the following setup is appropriate

```
IN=/project/sysviro/data/Nanopore-Pod5/
OUT=/project/sysviro/data/DoradoBasecalling/
filename=XXX

### Run basecalling (high accuracy mode, adapter trimming, BAM output)
/project/sysviro/dorado-0.9.0-linux-x64/bin/dorado basecaller --trim adapters -r $MODEL/rna004_130bps_hac@v3.0.1 $IN/path/to/pod5/ > $OUT/"$filename".hac.trimAdaptor.dorado.0.9.0.bam

### Generate sequencing summary file 
/project/sysviro/dorado-0.9.0-linux-x64/bin/dorado summary $OUT/$filename".hac.trimAdaptor.dorado.0.9.0.bam > $OUT/$filename".hac.trimAdaptor.dorado.0.9.0.sequencing_summary.txt

### Generate FASTQ file from BAM 
module load BEDTools 
bamToFastq -i $OUT/$filename".hac.trimAdaptor.dorado.0.9.0.bam -fq $OUT/$filename".hac.trimAdaptor.dorado.0.9.0.fastq
```

### Super-accuracy (modifications) basecalling
The latest RNA004 super high accuracy (sup) model allows for direct detection of various [RNA modifications](https://github.com/nanoporetech/dorado?tab=readme-ov-file#rna-models), including poly(A) tail estimates. Here
the preferred approach is to generated a single uBAM file where all modifications (inc. poly(A) tail length estimate) is calculated for each read.

STILL NEED TO UPDATE THE BELOW TO ALLOW FOR ALL MODS + TRANSFER OF ALL TAGS TO FASTQ

```
dorado basecaller --trim adaptor $MODEL/rna004_130bps_hac@v3.0.1 sample.pod5 --modified-bases m6A_DRACH > sample.bam
```

To go from an unaligned BAM file with modifications recorded in the MM and ML tags, it is necessary to convert to FASTQ while retaining the tags and then align to a reference using minimap2. This sounds complicated but in principal requires a single (piped) command. Note that SAMtools v1.16 or higher is needed along with minimap2 v2.26 or higher
```
samtools fastq -TMM,ML ${USAM} | minimap2 -x map-ont -a -t ${num_threads} -y --secondary=no ${REFERENCE} - > ${SAMFILE}

#e.g. 
samtools fastq -TMM,ML $OUT/EMC1-ARPE19-96h-1.sup-m6A.trimAdapters.dorado.0.5.1.bam | minimap2 -ax map-ont -L -p 0.99 -y --secondary=no /project/sysviro/data/reference_transcriptomes/VZV-Dumas-v2.0-complete.fasta - > /project/sysviro/users/Dan/VZVm6Atest/EMC1-ARPE19-96h-1.sup-m6A.tx.sam
```








