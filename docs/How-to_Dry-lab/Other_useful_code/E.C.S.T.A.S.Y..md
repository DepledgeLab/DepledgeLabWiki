---
tags:
    - Miscellaneous
    - Bash
    - Python
    - R

---

# **E**rik’s **C**ode **S**nippets **T**o **A**id and **S**upport **Y**ou (E.C.S.T.A.S.Y.)

!!! warning "Work in progress"

    This site is currently under construction, many parts still have an unfinished look or are yet to be written.<br>
    For a full view of currently available pages check out the [tag directory](../../tags.md).<br>
    ~Erik

Hello there!

Every once in a while one finds themselves in need of code. Not rarely one needs larger scripts or entire programs.
If you have come here in search for this, you are at the wrong place. In fact, this place is quite the opposite! This place is meant to be a (unordered) collection of (miscellaneous) code snippets and pieces I have created/collected at some point that might (or might not) be useful. If you are looking for proper pipelines and scripts, check out the [How to Dry lab - Analyses](../Analyses_Scripts/dorado.md) section. If you are looking for general HPC/bash commands, check out [How to Dry lab - Work with the HPC - Useful Commands](../hpc/commands.md). Have fun! <br>
~Erik
<br>
<br>

## HPC/bash related stuff


#### Calculate average (mean) QS from a bam file by a list of reads
``` bash
samtools view XXX.bam |\
 grep -f list_of_read_idsXXX.txt - |\
 sed 's/.*qs:.:\([0-9]*\.\?[0-9]*\).*/\1/' |\
 awk '{total += 10^(-$1/10)} END {print -10*log(total/NR)/log(10)}'
```
<br>

#### Get number of softclipping events (5' and 3') from bam file by softclipping length

For both leading and trailing softclipping per read:
``` bash
samtools view XXX.bam |\
cut -f6 |\
 grep 'S' |\
 sed -r 's/(^[0-9]+S)?([0-9]+[NMDIH=X])+([0-9]+S$)?/\1 \3/' |\
 sort -b -t'S' -k1,1n -k2,2n |\
 uniq -c
```

For leading and trailing softclipping independently:
``` bash
samtools view XXX.bam |\
 cut -f6 |\
 grep 'S' |\
 sed -r 's/(^[0-9]+S)?([0-9]+[NMDIH=X])+([0-9]+S$)?/\1\n\3/' |\
 sort -n |\
 uniq -c
```

For leading + trailing softlipping per read:<br>
[TODO]
<br><br>

#### Carrying a specific (or multiple) SAM tag (e.g. pt:i:) from the uBAM over to the aligned SAM/BAM
``` bash
# Use -T flag to write comments into the, fastq file; use -y to carry them over to SAM/BAM
# In case of mutliple tags just list them comma separated
samtools fastq -Tpt,MM,ML $BAM_DIREC/"$NAME"."$MODNAME".dorado."$DORVER".bam | \
minimap2 -ax splice -k14 -uf -y --secondary=no $HYB_REF_GX - > \
$OUT/"$NAME"."$MODNAME".dorado."$DORVER"."$HYB_NAME".sam
```
<br>

#### Get poly(A)-tail length distribution (and read_id, reference) from a bam file
``` bash
awk '{col_pt=""; for(i=12;i<=NF;i++){if($i~/^pt/){col_pt=$i; break}}; sub(/pt:i:/,"",col_pt); print $1,$3,col_pt}' <(samtools view XXX.bam)
```
!!! note
    You could also use a more straight forward approach with `grep`, but this would exclude reads without the pt:i: tag (which is the case for reads dorado couldn't determine the poly(A)-tail length), which might be inconvenient.
<br>

#### Get the number of (multiple) modifications from a bam file (order in which they appear)(with probability filtering (>98.04%)) per read in addition to read id, sequence length and poly(A)-tail length
``` bash
awk '{col_ml=""; col_pt=""; for(i=12;i<=NF;i++){if($i~/^pt/){col_pt=$i}; if($i~/^MM/){col_mm=$i}; if($i~/^ML/){col_ml=$i}}; sub(/pt:i:/,"",col_pt); n_mod=split(col_mm, arr_mm, ";")-1; sub(/ML:B:C,/,"",col_ml); n_ml=split(col_ml, arr_ml, ","); for(j=1;j<=n_mod;j++){ n_entr[j]=split(arr_mm[j], b, ",")-1}; start=1; end=n_entr[1]; for(k in n_entr){end=start+n_entr[k]-1; amount[k]=0; for(l=start;l<=end;l++){if(arr_ml[l]>250){amount[k]++}}; start=start+n_entr[k]}; printf "%s %s %s %s ", $1, $3, length($10), col_pt; for(m in amount){printf "%s ", amount[m]}; print "" }'
```
As awk multi-line:
``` awk
{
    col_ml = ""
    col_pt = ""
    # Find col_pt, col_mm, col_ml in fields >= 12
    for (i = 12; i <= NF; i++) {
        if ($i ~ /^pt/) { col_pt = $i }
        if ($i ~ /^MM/) { col_mm = $i }
        if ($i ~ /^ML/) { col_ml = $i }
    }
    # Clean up strings
    sub(/pt:i:/, "", col_pt)
    n_mod = split(col_mm, arr_mm, ";") - 1
    sub(/ML:B:C,/, "", col_ml)
    n_ml = split(col_ml, arr_ml, ",")

    # Find number of entries for each modification
    for (j = 1; j <= n_mod; j++) {
        n_entr[j] = split(arr_mm[j], b, ",") - 1
    }

    start = 1
    end = n_entr[1]

    # For each modification, count how many ML elements are >250 (>98.04%)
    for (k in n_entr) {
        end = start + n_entr[k] - 1
        amount[k] = 0
        for (l = start; l <= end; l++) {
            if (arr_ml[l] > 250) {
                amount[k]++
            }
        }
        start = start + n_entr[k]
    }

    # Print output
    printf "%s %s %s %s ", $1, $3, length($10), col_pt
    for (m in amount) {
        printf "%s ", amount[m]
    }
    print ""
}
```
<br>

#### Get all reads from a bam file (with the help of a dorado sequencing summary file) originating from one specific pod5 file and within a specific poly(A)-tail length range
``` bash
# Here: pod5 nr. 100 from pass as an example; poly(A)-tail range 50-200
awk '$1~/.*pass.*100.pod5/{print $2}' XXX.sequencing_summary.txt |\
 grep -f - <(samtools view XXX.bam) |\
 grep "pt:i:" |\
 awk '{polyA=""; polyA=int(gensub(/.*pt:i:([0-9]*).*/, "\\1", "g")); if(polyA>=50 && polyA<=200){print $1,$3,polyA}}' - >\
 XXX.txt
```
<br>

#### Get the number of bases at the last SEQ position across a bam file
``` bash
samtools view XXX.bam |\
 cut -f 10 |\
 sed -r 's/.*(.$)/\1/g' |\
 sort |\
 uniq -c
```
<br>

#### Get the distribution of poly(same-base) at end of SEQ across bam (e.g. A)
``` bash
samtools view XXX.bam |\
 cut -f 10 |\
 sed -r -n 's/(.*[^A])(A+$)/\2/p' |\
 sort |\
 uniq -c
```
<br>


#### Extract poly(A)-tail trace information from the dorado verbose log file
``` bash
# Generate verbose dorado basecalling log
$DORVERBIN basecaller -r $DORMOD --estimate-poly-a -vv $IN/$NAME/ > \
$BAM_DIREC/"$NAME"."$MODNAME".dorado."$DORVER".bam 2> \
$OUT/"$NAME"."$MODNAME".dorado."$DORVER".vlog

# Get corresponding lines from the file
awk '/^.{26}\[trace\] [a-f0-9]{8}-[a-f0-9\-]{27} PolyA/' XXX.vlog

# Extract relevant data to table
awk 'BEGIN {print "read_id polya_bases signal_anchor signal_range_start signal_range_end signal_length samples_per_base trim read_len"} /^.{26}\[trace\] [a-f0-9]{8}-[a-f0-9\-]{27} PolyA/ {id=gensub(/^.{34}([a-z0-9-]{36}).*/,"\\1","g"); sub(/^.{83}/,""); gsub(/[^[:digit:] \.]/,""); gsub(/  +/," "); print id,$0}' XXX.vlog >\
 XXX.txt
```
As an awk multi-line:
``` awk
BEGIN {
    print "read_id polya_bases signal_anchor signal_range_start signal_range_end signal_length samples_per_base trim read_len"
}

# Get lines containing the poly(A) trace information
/^.{26}$trace$ [a-f0-9]{8}-[a-f0-9\-]{27} PolyA/ {
    # Extract the read id
    id = gensub(/^.{34}([a-z0-9-]{36}).*/, "\\1", "g")

    # Remove the first 83 characters from the current record
    sub(/^.{83}/, "")

    # Remove all non-digit, non-dot, non-space characters
    gsub(/[^[:digit:] \.]/, "")

    # Replace sequences of multiple spaces with a single space
    gsub(/  +/, " ")

    # Print the id followed by the cleaned data
    print id, $0
}
```
!!! note
    Since dorado v1.0.0 this shouldn't be necessary anymore because the poly(A)-tail tracing information is now placed under its own flag in the uSAM file. (Similarly, extracting the poly(A)-tail length via `pt:i:` can instead be obtained from the sequencing summary file.)

<br>

#### Get overview of a pod5 file
``` bash
# Create conda environment and install the pod5 package, then run:
pod5 inspect summary XXX.pod5
```
<br>

#### Extract important stats (poly(A)-tail length, modifications (here: filter of >98.04%)) from a bam file (and list of ids; here: 99% barcode confidence) by read_id and multiplex groups (WarpDemuX)
``` bash
awk 'BEGIN {print "read_id barcode reference seq_length polyA_len n_inosine n_m6A n_pseU"} FNR==NR {a[$1]=$2; next}; $1 in a {col_ml="";col_pt="NA"; for(i=12;i<=NF;i++){if($i~/^pt/){col_pt=$i}; if($i~/^MM/){col_mm=$i}; if($i~/^ML/){col_ml=$i}}; sub(/pt:i:/,"",col_pt); n_mod=split(col_mm, arr_mm, ";")-1; sub(/ML:B:C,/,"",col_ml); n_ml=split(col_ml, arr_ml, ","); for(j=1;j<=n_mod;j++){ n_entr[j]=split(arr_mm[j], b, ",")-1}; start=1; end=n_entr[1]; for(k in n_entr){end=start+n_entr[k]-1; amount[k]=0; for(l=start;l<=end;l++){if(arr_ml[l]>250){amount[k]++}}; start=start+n_entr[k]}; printf "%s %s %s %s %s ",$1,a[$1],$3,length($10),col_pt; for(m in amount){printf "%s ", amount[m]}; print ""}' id_sample_keytable-filt0.99.txt <(samtools view XXX.bam) > XXX.txt
```
As an awk multi-line:
``` awk
WIPWIPWIPWIPWIP
```
!!! warning
    In this iteration the modification names are not extracted automatically and instead have to be added manually, for this, the order of modification appearance in the bam file has to be checked (see the relevant MM/ML tags in the SAM/BAM file). <br>
    This could be automated if the order of the tags gets parsed first and copied to the output file headers (or translated to proper names).
<br>


#### Old method to open a RStudio session running on the hpc (New: Use OnDemand)
``` bash
cd /project/sysviro/R/01_R_Studio
sbatch startrstudio.sbatch
cat rstudio-server.<user>.out # get user, password and adress (type into browser) from here

scancel -f <jobID> # to end the session (do not forget!)
```
<br>


## R related stuff

#### Transform ASCII QUAL QS (SAM format) to decimal
``` R
# Edit string first to add escape backslashes, e.g. here:
# https://www.browserling.com/tools/add-slashes

# Transform ASCII
library(stringr)
qual <- "ASCIISTRING"
qual <- str_split_1(qual, "")
qual_num <- list()
qual_prob <- list()
for (i in 1:length(qual)) {
    qual_num <- append(qual_num, (as.integer(charToRaw(qual[i])) - 33))
    qual_prob <- append(qual_prob, 10^(-qual_num[[i]]/10))
}
qual_num <- unlist(qual_num) # Numeric Phred QS
qual_prob <- unlist(qual_prob) # Probability

# Example: Calculate mean QS
-10*log10((mean(qual_prob)))
```
<br>


## MS Word related stuff

#### Placing a space every third letter in a continuous string (e.g. DNA sequence)

1. Search (++ctrl+f++) :material-arrow-right: Replace (small dropdown menu) :material-arrow-right: More :material-arrow-right: Find what: `^1` :material-arrow-right: Replace with nothing (removes newlines)
2. Find what: `([A-Z]{3})` :material-arrow-right: Replace with `\1 ` (includes a space!)
<br>
