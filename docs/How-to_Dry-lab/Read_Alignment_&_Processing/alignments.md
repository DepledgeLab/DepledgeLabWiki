



## Overview

A sequence read (or read-pair) may have one or more primary alignments against a reference genome. 

A single primary alignment is the most common outcome but where multiple primary alignments occur, one will (usually randomly) be set as the primary alignment while the remainder will be flagged as secondary alignments.
These are usually all flagged with a mapping quality (mapQ) score of 0.

Secondary alignments are common in low complexity regions and/or repeat regions or when structural variation is present. For instance, reads aligning against the terminal repeats of VZV or HSV-1 will usually have two equally
good alignments, one of which will be set as the 'primary'.

Supplementary alignment occurs when two non-overlapping sections of a sequence read align to distinct locations. These are most commonly seen where structural variation is present




