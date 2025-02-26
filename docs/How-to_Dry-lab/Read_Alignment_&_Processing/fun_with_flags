---
tags:
    - alignment
    - flags
---

# Fun with (alignment) flags

## Overview

Filtering sequence read alignments based on their alignment flag (SAM/BAM column 3) is extremely useful for evaluating the mapping quality of read alignments and inferring strandedness. It is also possible to filter 
SAM/BAM files to retain only alignments with specific alignment flags.

![funwithflags](../../img/funwithflags.jpg)
*** flags relevant to nanopore DRS and derivatives


A common step in processing nanopore datasets is to retain only primary alignments for downstream processing. This can be accomplished when converting between SAM to BAM e.g.

``` samtools view -F 2308 -o filtered.bam in.sam ```






## Further reading

[Alignment flag decoder](http://broadinstitute.github.io/picard/explain-flags.html)
