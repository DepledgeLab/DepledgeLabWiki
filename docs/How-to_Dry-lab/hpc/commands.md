---
tags:
    - Guide
    - Bash

---

!!! warning "Work in progress"

    This site is currently under construction, many parts still have an unfinished look or are yet to be written.<br>
    For a full view of currently available pages check out the [tag directory](../../tags.md).<br>
    ~Erik

This is a collection of general HPC/bash related commands. If you are looking for something more specific, check out the respective [Pipeline & Script](../Analyses_Scripts/dorado.md) section or [here](../Other_useful_code/E.C.S.T.A.S.Y..md).

<br>
<br>

#### Starting an interactive (CPU-)session

``` bash
srun --partition=leinecpu_interactive --job-name="insert_name_here" --cpus-per-task=1 --mem-per-cpu=1G -t4:00:0 --pty /bin/bash
```

To change the memory, number of cores or time change the parameters accordingly.

!!! note
    You might still stumble across a very similar command in some place containing `#!bash --partition=interactive` (that doesn't work). This is the old name of the partition and is no longer used.
<br>

#### Starting an interactive (GPU-)session

``` bash
srun --partition=leinegpu_interactive --job-name="insert_name_here" --cpus-per-task=1 --mem-per-cpu=1G -t4:00:0 --pty /bin/bash
```
<br>

#### Get stats on run job
``` bash
# (format can be changed, only examples are being shown)
# job that already ran
seff <jobID>

# job that is still running
sstat <jobID>.batch --format=JobID,MaxRSS

# detailed list on jobs from a specific time
sacct --starttime YYYY-MM-DD --format=User,JobID,Jobname,partition,state,time,start,end,elapsed,AveRSS,MaxRss,MaxVMSize,nnodes,ncpus,nodelis
```
!!! tip
    For a more graphical and user-friendly overview you can check out (active) jobs at the [MHH OnDemand site](https://leineood.mh-hannover.local) at jobs --> expand job --> detailed metrics
<br>

#### Check current hpc resources in detail
``` bash
sinfo -o "%.13n %.20P %.14G %.5a %.9T %.10e %.8m %.4c %.15C"
```
<br>

#### Check hpc partition details
``` bash
scontrol show partition
```
<br>

#### Observe GPU (NVIDIA card) usage of a running job live
``` bash
watch -n 1 srun --jobid=<jobID> nvidia-smi
```
(Cannot be run in an interactive session.)
<br><br>

#### Open process manager

``` bash
htop
```
<br>

#### Get total disk usage of a project folder
``` bash
df -h /project/sysviro
```

<br>

#### Get storage usage of current directory (recursive)(excluding backups)
``` bash
du -hLlxs . --exclude "./.snapshots*" 2>/dev/null # total
du -hd1 . --exclude "./.snapshots*" 2>/dev/null # subdirectories + total
```
<br>

#### Get a list of all users in a project folder
``` bash
getent group gs-mhh-hpc-sysviro
```
<br>
