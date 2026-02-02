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

Key aspects of pseudoalignment include:
- Speed and Efficiency: It skips the time-consuming process of mapping base-to-base alignments, often allowing millions of reads to be processed in minutes on standard computers.
- Mechanism: It breaks reads into k-mers and maps them against a transcriptome de Bruijn graph (T-DBG) to find the set of transcripts that match the read.
- Tools: Primary tools utilizing this method include Kallisto, Salmon, and Sailfish.
- Applications: It is primarily used for RNA-seq transcript abundance quantification and metagenomic read assignment.
- Limitations: Because it does not provide exact positional information (e.g., specific splice sites or exact base mismatches), it is not suitable for applications requiring high-precision mapping, such as de novo assembly or detecting single nucleotide polymorphisms (SNPs). 
- Pseudoalignment is designed to handle non-uniquely mapped reads by identifying the potential transcripts of origin, which can then be further refined using algorithms like Expectation-Maximization. 


Standard pseudoalignment code








